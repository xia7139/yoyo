+++
title = "awk命令实现一个汇编解释器"
author = ["Cheng Xia"]
date = 2023-12-10
draft = false
+++

本文主要参考一本介绍awk命令的书[^fn:1]。 <br/>

虚拟计算机这个概念经常出现在计算机体系结 构或系统编程的基础课程中. 虚拟计算机有一个累加器, 十条指令, 按字编址且大小为 1000 字的 内存, 我们假设内存的一个 “字” 可以保存 5 个十进制数位, 如果某个字存放的是一条指令, 那么 前两个数位表示操作码, 最后三个数位表示内存地址. 所有的汇编语言指令如下： <br/>

| 操作码 | 指令    | 意义                                |
|-----|-------|-----------------------------------|
| 01  | get     | 从输入读取一个数, 并存放到累加器中  |
| 02  | put     | 把累加器的值写到输出                |
| 03  | ld M    | 把地址为 M 的内存单元的值读取到累加器中 |
| 04  | st M    | 把累加器的值存放到地址为 M 的内存单元中 |
| 05  | add M   | 把地址为 M 的内存单元的值与累加器的值相加, 再把结果存放到累加器中 |
| 06  | sub M   | 把地址为 M 的内存单元的值与累加器的值相减, 再把结果存放到累加器中 |
| 07  | jpos M  | 如果累加器的值为正, 则跳转到内存地址 M |
| 08  | jz M    | 如果累加器的值为零, 则跳转到内存地址 M |
| 09  | j M     | 跳转到内存地址 M                    |
| 10  | halt    | 停止执行                            |
| x   | const C | 伪操作符, 用于定义一个常量 C        |

汇编语言程序由语句序列组成, 每一条语句都包括三个部分: 标号, 操作符, 操作数, 任意一 个部分都可以省略, 标号如果存在, 则必须是所在行的第一个字段. 程序可以包含 awk 形式的注 释. 这里有一个简单的汇编语言程序, 它的功能是输出多个整数的和, 0 表示输入结束。(标号指的就是loop、done等这些记录代码位置的标记，大部分程序行直接以一个tab开头，没有标号，这样标号就是空。) <br/>
汇编脚本文件，program-file，内容如下: <br/>

```text
# print sum of input numbers (terminated by zero)
	ld zero # initialize sum to zero
	st sum
loop get # read a number
	jz done # no more input if number is zero
	add sum # add in accumulated sum
	st sum # store new value back in sum
	j loop # go back and read another number
done ld sum # print sum
	put
	halt
zero const 0
sum const
```

对应的目标程序由整数序列组成, 这些整数其实就是程序的机器码形式, 当目标程序运行时, CPU 从内存中读取指令, 译码并执行. 上面程序的机器码如下： <br/>
![](/ox-hugo/01_machine_code_example.png) <br/>

这里机器码实际上就是指前面图中冒号右侧的5位数字表示的列。这里冒号右边是表示内存地址，冒号右侧5位数字之后，其实是对5位数字的解释，标明了数字和代码的对应关系。 <br/>

第一个字段是内存地址, 第二个字段是编码后的指令. 内存地址 0 存放的是汇编语言程序的第一 条指令: ld zero。 <br/>
汇编程序对源程序进行汇编时需要遍历两次. 第一次遍历利用字段分割操作对源程序进行 词法与语法检查: 读取汇编语言源程序, 忽略注释, 为每一个标号分配内存地址, 把操作符与操作 数的中间表示形式写到一个临时文件中. 第二次遍历读取临时文件, 根据第一次遍历时计算的结果, 把符号化的操作数转换成内存地址, 对操作符与操作数进行编码, 把最终的机器语言程序保存到数组 mem 中。 <br/>
我们将会开发一个解释器来完成另一半的工作, 解释器可以用来模拟计算机执行机器语言程 序时所表现出的行为. 解释器循环地从 mem 中读取指令, 把指令译码成操作符与操作数, 再模拟指令的执行. 变量 pc 用来模拟程序计数器 (program counter)。awk脚本文件，asm，内容如下: <br/>

```text
# asm - assembler and interpreter for simple computer 
# usage: awk -f asm program-file data-files... 
BEGIN {
	srcfile = ARGV[1]
	ARGV[1] = "" # remaining files are data
	tempfile = "asm.temp"
	#给每一个操作符一个编号
	n = split("const get put ld st add sub jpos jz j halt", x)
	for (i = 1; i <= n; i++) # create table of op codes
		op[x[i]] = i-1

	# ASSEMBLER PASS 1，去掉注释、将标号的位置记录在systab数组、将操作符和操作数写入单独的临时文件
	FS = "[ \t]+"
	while (getline <srcfile > 0) {
		sub(/#.*/, "") # strip comments
		symtab[$1] = nextmem # remember label location
		if ($2 != "") { # save op, addr if present
			print $2 "\t" $3 >tempfile
			nextmem++
		}
	}
	close(tempfile)

	# ASSEMBLER PASS 2，将操作符和操作数装入mem数组模拟的内存，在这个过程中，会同时完成标号的翻译
	nextmem = 0
	while (getline <tempfile > 0) {
		if ($2 !~ /^[0-9]*$/) # if symbolic addr,
			$2 = symtab[$2] # replace by numeric value
		mem[nextmem++] = 1000 * op[$1] + $2 # pack into word
	}

	# INTERPRETER，用pc变量模拟程序计数器、acc变量模拟累加器，执行整个脚本程序
	for (pc = 0; pc >= 0; ) {
		addr = mem[pc] % 1000
		code = int(mem[pc++] / 1000)
		if (code == op["get"]) { getline acc }
		else if (code == op["put"]) { print acc }
		else if (code == op["st"]) { mem[addr] = acc }
		else if (code == op["ld"]) { acc = mem[addr] }
		else if (code == op["add"]) { acc += mem[addr] }
		else if (code == op["sub"]) { acc -= mem[addr] }
		else if (code == op["jpos"]) { if (acc > 0) pc = addr }
		else if (code == op["jz"]) { if (acc == 0) pc = addr }
		else if (code == op["j"]) { pc = addr }
		else if (code == op["halt"]) { pc = -1 }
		else { pc = -1 }
		}
}
```

数组 symtab 记录标号的内存地址, 如果当前输入行没有标号, 那么内存地址赋值给 `symtab[""]` 。 <br/>
标号是汇编语句的第一个字段, 操作符前面有一个空格符. 第一次遍历前, 把 FS 设置成 `[ \t]+` , 于是字段分隔符变成由多个空格符和制表符组成的序列. 比较特殊的是, 前导空格也被 当作字段分隔符, 所以 `$1` 总是标号, 而 `$2` 总是操作符. <br/>
因为伪操作符 const 的 “操作码” 是 0, 所以在第二次遍历时, 语句 <br/>
`mem[nextmem++] = 1000 * op[$1] + $2 # pack into word` <br/>
可以同时用来存放常数与指令。 <br/>
运行效果如下： <br/>

```text
$ ls
asm		program-file
$ awk -f asm program-file 
11
22
11
10
0
54
$ ls
asm		asm.temp	program-file
$ cat asm.temp 
ld	zero
st	sum
get	
jz	done
add	sum
st	sum
j	loop
ld	sum
put	
halt	
const	0
const	
$
```

这是一个看起来非常有意思的例子，所以在这里贴下，内容和书上差不多，基本没变。 <br/>

[^fn:1]: The AWK programming language, (Alfred V.Aho, Brian W.Kernighan, Peter J.Weinberger), 翻译：<https://github.com/wuzhouhui/awk> <br/>