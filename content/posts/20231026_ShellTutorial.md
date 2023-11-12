+++
title = "Shell语法说明"
author = ["Cheng Xia"]
date = 2023-10-26
draft = false
+++

<div class="ox-hugo-toc toc">

<div class="heading">Table of Contents</div>

- [命令行执行说明](#命令行执行说明)
- [命令行基本语法说明](#命令行基本语法说明)
    - [shell meta和command meta](#shell-meta和command-meta)
- [行分隔符](#行分隔符)
- [多条命令组合](#多条命令组合)
- [命令描述符](#命令描述符)
- [重定向和文件描述符](#重定向和文件描述符)
- [字符串操作](#字符串操作)
    - [按照长度截取](#按照长度截取)
    - [按照内容截取](#按照内容截取)
- [case语句](#case语句)
    - [基本语法](#基本语法)
    - [场景示例](#场景示例)
- [if条件判断](#if条件判断)
    - [基本语法](#基本语法)
    - [字符串判断](#字符串判断)
    - [数字大小](#数字大小)
    - [条件判断中的关系运算](#条件判断中的关系运算)
        - [or](#or)
        - [and](#and)
        - [取反](#取反)
    - [文件和目录是否存在](#文件和目录是否存在)
- [循环](#循环)
    - [while](#while)
    - [for](#for)
    - [break](#break)
- [常用命令](#常用命令)
    - [hostname](#hostname)
        - [常用选项](#常用选项)
        - [经典场景](#经典场景)
    - [ls](#ls)
        - [常用选项](#常用选项)
        - [查看占用文件块的大小](#查看占用文件块的大小)
        - [指定时间格式](#指定时间格式)
        - [遍历全部子目录](#遍历全部子目录)
    - [date](#date)
    - [find](#find)
        - [查找修改时间在15天以内的文件](#查找修改时间在15天以内的文件)
        - [按照文件名查找](#按照文件名查找)
        - [打印时间](#打印时间)
        - [查找某个时间区间内的文件](#查找某个时间区间内的文件)
        - [find命令后跟多个操作](#find命令后跟多个操作)
    - [tar](#tar)
        - [常用选项说明](#常用选项说明)
        - [打包](#打包)
            - [简单将同一目录下的多个文件打成一个tar](#简单将同一目录下的多个文件打成一个tar)
            - [查看tar包中的文件列表](#查看tar包中的文件列表)
            - [往存量tar中添加文件](#往存量tar中添加文件)
        - [查看压缩包中的内容](#查看压缩包中的内容)
        - [解包](#解包)
            - [解压到当前目录](#解压到当前目录)
            - [解压到指定目录](#解压到指定目录)
    - [cut](#cut)
        - [常用选项](#常用选项)
        - [典型场景](#典型场景)
    - [read](#read)
        - [选项](#选项)
        - [场景](#场景)
    - [cp](#cp)
    - [cat](#cat)
    - [touch](#touch)
        - [选项](#选项)
        - [场景](#场景)
    - [uniq](#uniq)
        - [场景](#场景)
    - [awk](#awk_command_introduction)
        - [场景](#场景)
            - [输出行中的指定字段](#输出行中的指定字段)
            - [输出文件中的指定行](#输出文件中的指定行)
                - [行号筛选](#行号筛选)
                - [根据内容筛选](#根据内容筛选)
            - [输出指定行的指定字段](#输出指定行的指定字段)
        - [奇怪问题](#奇怪问题)
    - [comm](#comm)
        - [常用选项](#常用选项)
        - [场景](#场景)
    - [echo](#echo)
        - [常用选项](#常用选项)
        - [场景](#场景)
    - [tee](#tee)
        - [常用选项](#常用选项)
        - [场景](#场景)
    - [grep](#grep)
        - [常用选项](#常用选项)
        - [场景](#场景)
- [典型场景](#典型场景)
    - [获取bash的进程的pid](#获取bash的进程的pid)
    - [获取bash的版本](#获取bash的版本)
    - [读取内容时忽略第一行](#读取内容时忽略第一行)
    - [同时输出多行](#同时输出多行)

</div>
<!--endtoc-->

本文主要介绍shell用法，基本是基于bash。 <br/>


## 命令行执行说明 {#命令行执行说明}

参考[^fn:1] <br/>
命令行，就是shell prompt和Carriage Return字符(CR)之间的所有文字。一般来说，命令行的格式如下： <br/>

```text
command-name options argument
```

再执行时，shell会根据IFS(Internal Field Seperator)将命令行中的文字拆解为单词(或者叫字段，word)，然后，将其中的meta字符先做处理，最后再重组整个命令行，最后执行。 <br/>
IFS是shell内部预设的分隔符可以由一个或者多个如下按键字符组成： <br/>

-   空格键(Space) <br/>
-   制表键(Tab) <br/>
-   回车键(Enter) <br/>

其中command-name每个命令必不可少的，可以是如下内容： <br/>

-   明确路径指定的外部命令 <br/>
-   命令别名(alias) <br/>
-   自定义函数(function) <br/>
-   shell内置的命令 <br/>
-   $PATH变量之下的外部命令。 <br/>


## 命令行基本语法说明 {#命令行基本语法说明}

参考[^fn:2] <br/>
命令行中所有的字符基本可以分为两类： <br/>

-   字面字符，也就是普通的字符，没有特殊含义； <br/>
-   meta字符，具有特殊保留含义的字符。 <br/>

字面字符就是一般的字符，需要特殊注意的主要是meta字符。常见的meta字符如下： <br/>

-   IFS, 由 &lt;space&gt; 或 &lt;tab&gt; 或 &lt;enter&gt; 三者之一組成(我们常用 space )； <br/>
    IFS是用来分隔命令行的单词(word)的，命令行按照单词(word)来进行处理。 <br/>
-   CR, 由 &lt;enter&gt; 产生。 <br/>
    用来结束命令行，按下enter之后，命令会开始运行。 <br/>
-   =, 设定变量。 <br/>
-   $, 用作变量或者运算替换。 <br/>
-   &gt;, 重定向stdout。 <br/>
-   &lt;, 重定向stdin。 <br/>
-   |, 管道操作符。 <br/>
-   &amp;, 重定向文件描述符，或者将命令置于后台运行。 <br/>
-   (), 将其中的命令放在nested shell中执行，或用于运算或者命令替换。 <br/>
-   {}, 将其中的命令放在匿名函数(non-named function)中执行，或在变量替换中用来界定范围。 <br/>
-   ;, 在前一个命令结束时，忽略其返回值，继续执行下一个命令。 <br/>
-   &amp;&amp;, 在前一个命令执行成功之后，才执行后一个命令。 <br/>
-   ||, 前一个命令执行失败，才执行后一个命令。 <br/>
-   !, 执行历史列表中的命令。 <br/>

如果要在命令行中，将这些meta字符的特殊含义关闭，就需要用到转义。转义有三种方式： <br/>

-   单引号(hard quote)，其中所有的meta字符都被关闭； <br/>
-   双引号(soft quote)，除某些(比如$)之外的其他大部分meta字符都被关闭； <br/>
-   反斜线(escape)，紧跟在反斜线后面的meta字符被关闭。 <br/>

如下几个例子： <br/>

-   关闭meta字符空格 <br/>
    ```text
    # 空格字符没有转义，shell认为这个命令的含义是A=B，然后是IFS，执行C命令，会报错C找不到。
    $ A=B C
    C: command not found
    $ echo $A
    
    # 将空格放在双引号中，这样空格的meta含义就被关闭了，shell将其解释为普通的字符。
    $ A="B C"
    $ echo $A
    B C
    
    # 采用单引号关闭meta，效果也一样。
    $ A='B C'
    $ echo $A
    B C
    ```
-   关闭meta字符回车 <br/>
    ```text
    # 这里<enter>被放在单引号(hard quote)中，就不会被shell解释为CR字符了。它就是一个简单的换行符(new line)。
    $ A='B
    > C
    > '
    # 这里双引号中的meta$不会被关闭，而其他meta字符均被关闭，因此，原样输出变量A的内容(包括换行)。
    $ echo "$A"
    B
    C
    
    # 这里由于$A没有出现在双引号(soft quote)中，因此，这里变量替换完成后，命令重组时enter会被解释成IFS，而不是解释为换行符(new line)。
    $ echo $A
    B C
    $ 
    ```
    这个例子中，最后的情况需要特别注意，也就是说，当变量中出现了enter，但没有被转义时，这时候会被shell认为是IFS，不是原样输出为换行符。这个特别关键，而且比较反直觉。需要特别注意。如果采用双引号做前面的转义，效果一样，如下： <br/>
    ```text
    $ A="B
    > C
    > "
    $ echo "$A"
    B
    C
    
    $ echo $A
    B C
    $ 
    ```
-   escape关闭CR字符 <br/>
    ```text
    $ A=B\
    > C\
    > 
    $ echo $A
    BC
    $ echo "$A"
    BC
    $
    ```
    从这个例子，可以看出，通过反斜线对enter字符进行转义时，对其关闭了meta字符的特性，同时，也没有让它保留为一个IFS字符。也就是说，经过这个escape转义，这个enter字符，什么都不是了，和没有一样。这块儿和前面的单引号或者双引号转义，还是有区别的。 <br/>
-   escape关闭空格字符 <br/>
    ```text
    $ A=B\ C
    $ echo $A
    B C
    $ echo "$A"
    B C
    $ echo '$A'
    $A
    $
    ```
    这个例子看，反斜线对于空格字符的转义，没有前面对于enter那样狠，转义之后，空格还作为一个普通的文本字符存在。 <br/>
-   单双引号组合使用 <br/>
    ```text
    $ A=B\ C
    $ echo '"$A"'
    "$A"
    $ echo "'$A'"
    'B C'
    $
    ```
    比较好理解，单引号中的全部meta字符都被关闭了，所以，原样输出，都没有做变量替换。而双引号在外面的情况中，先做了变量替换，然后又将替换后的内容做了原样输出。 <br/>


### shell meta和command meta {#shell-meta和command-meta}

参考[^fn:2] <br/>
除了前面的shell内置的meta字符外，好多shell命令有自己特有的meta字符。对于这两种meta字符有交集的情况，就需要额外注意。 <br/>
比如大括号{}在shell原生的含义是将其中的命令在一个匿名函数中执行(也可以简单视为一个语句块)。同时，在awk命令中，也会用大括号{}来区分awk的命令区段(BEGIN、MAIN和END)。如果在命令行中直接执行： <br/>

```text
$ echo -e "asdf\nqwert" > a.txt
$ cat a.txt 
asdf
qwert
$ awk {print $0} a.txt 
awk: line 2: missing } near end of file
$
```

这个报错看起来很奇怪，原因就是meta字符大括号没有被关闭，这样shell会认为 `{print $0}` 是一个命令块(command block)，但是在同一行中又没有分号”;“作为命令结束标识，因此就会报错。 <br/>
解决的方式比较简单，关闭shell内置meta即可： <br/>

```text
$ cat a.txt 
asdf
qwert
# 单引号全部关闭shell内置meta
$ awk '{print $0}' a.txt 
asdf
qwert

# 双引号关闭大部分，反斜线单独关闭$
$ awk "{print \$0}" a.txt 
asdf
qwert

# 通过反斜线逐一关闭各个meta
$ awk \{print\ \$0\} a.txt 
asdf
qwert
$
```

到这里，如果前面命令中 `$0` 的0是来自一个shell变量A，可以用如下方案： <br/>

```bash
A=0
awk "{print \$$A}" 1.txt
awk \{print\ \$$A\} 1.txt
awk '{print $'$A'}' 1.txt
awk '{print $'"$A"'}' 1.txt
```


## 行分隔符 {#行分隔符}

shell语句中，分号和换行都可以作为行分隔符，因此，每条语句的末尾不用额外加分号作为结束。 <br/>


## 多条命令组合 {#多条命令组合}

参考[^fn:3] <br/>

| 多命令执行符 | 格 式                | 作 用                                    |
|--------|--------------------|----------------------------------------|
| ;            | 命令1 ; 命令2        | 多条命令顺序执行，命令之间没有任何逻辑关系 |
| &amp;&amp;   | 命令1 &amp;&amp; 命令2 | 命令1正确执行( `$?=0` )，命令2才执行；否则，命令2不会执行 |
| &vert;&vert; | 命令1 &vert;&vert; 命令2 | 如果命令1执行不正确( `$?≠0` )，则命令2才会执行；否则，命令2不会执行 |


## 命令描述符 {#命令描述符}


## 重定向和文件描述符 {#重定向和文件描述符}

Bash中0、1、2是三个文件描述符，分别对应stdin、stdout和stderr，默认他们都指向terminal，也就是终端。默认Bash从终端读取输入，然后，输出到终端。可以通过重定向操作改变这几个文件描述符的指向。 <br/>
重定向操作有如下几种： <br/>

-   输出重定向， `n>file` ，n缺省为1。 <br/>
-   输入重定向，  `n<file` ，n缺省为0。 <br/>
-   文件标识符复制， `n>&m` 。 <br/>
    这个比较难理解，功能上说，是让原来输出到文件描述符n的内容，重定向到文件描述符m。原理上说，实际是把文件描述符m的值赋给了n，这样，效果上来说，和前面的功能描述不冲突。 <br/>
-   当有重定向操作时，从左到右设置。 <br/>
    比如要把1和2两个文件描述符都重定向到一个文件，需要用 `1>file 2>&1` (等价于 `>file 2>&1` )。如果反过来 `2>&1 >file` 先把文件描述符1指向的值terminal赋给2，然后，再把1重定向到file，这样文件描述符2还是指向terminal，没有实现这个效果。 <br/>
-   `<&-` 或者 `0<&-` 关闭文件描述符0(stdin)， `>&-` 或者 `1>&-` 关闭文件描述符1(stdout)。 <br/>
-   `1<&3-` 文件描述符3赋值给1，之后马上关闭。 <br/>
-   `sed 's/foo/bar/' a.txt >a.txt` 意图是大概是想将sed文件的输出再重定向回a.txt，但实际效果并不是这样。因为重定向操作在命令之前执行，这里用的重定向操作符是 `>` ，在执行时会将文件清空。这样，sed命令启动之前，文件已经被清空了，sed读不到任何内容，所以效果非预期。 <br/>
    要实现前面的效果，可以通过管道将sed的输出给到cat命令，然后将cat命令的输出重定向回原文件。如下： <br/>
    ```bash
    sed 's/foo/bar/' a.txt | cat >a.txt
    ```

管道 <br/>
连接左边命令的标准输出到右边命令的标准输入。实现上来说，它创建了一个特殊的文件，左边命令将这个文件打开为输出目的地，右边的命令将这个文件打开为输入源。如下是一个模拟管道的例子： <br/>

```text
$ echo "asdf" 1>a.txt && cat 0<a.txt
asdf
$
```

这个例子中，a.txt接受echo的输出，然后，输入给cat(cat命令如果不带参数，会默认从stdin读入一行并原样输出)，作用就相当于管道。 <br/>

重定向的复杂例子： <br/>

```bash
{
  {
    cmd1 3>&- |
      cmd2 2>&3 3>&-
  } 2>&1 >&4 4>&- |
    cmd3 3>&- 4>&-

} 3>&2 4>&1
```

这里大括号的效果应该就是启动了一个子shell，有独立的文件描述符环境。执行时实际的效果应该如下： <br/>

```text
						   cmd2

					   ---       +-------------+
				       -->( 0 ) ---->| 1st pipe    |
				      /	   ---       +-------------+
				     /
				    /	   ---       +-------------+
	 cmd 1                	   /	  ( 1 ) ---->| /dev/pts/5  |
				  /	   ---       +-------------+
				 /
 ---       +-------------+	/	   ---       +-------------+
( 0 ) ---->| /dev/pts/5  |     /	  ( 2 ) ---->| /dev/pts/5  |
 ---       +-------------+    /		   ---       +-------------+
			     /
 ---       +-------------+  /                       cmd3
( 1 ) ---->| 1st pipe    | /
 ---       +-------------+                 ---       +-------------+
			     ------------>( 0 ) ---->| 2nd pipe    |
 ---       +-------------+ /               ---       +-------------+
( 2 ) ---->| 2nd pipe    |/
 ---       +-------------+                 ---       +-------------+
					  ( 1 ) ---->| /dev/pts/5  |
					   ---       +-------------+

					   ---       +-------------+
					  ( 2 ) ---->| /dev/pts/5  |
					   ---       +-------------+
```

exec命令 <br/>
这个命令的作用是通过一个指定的程序替代原生的内置shell，如果没有指定程序，exec命令可以修改当前shell的文件描述符。 <br/>
如下例子中按行读取并输出一个文件： <br/>

```text
while read -r line;do echo "$line";done < file
```

如果是想边按行读文件输出，并接收读入，如下命令是不行的： <br/>

```text
while read -r line;do echo "$line"; read -p "Press any key" -n 1;done < file
```

需要用exec命令先将读取的文件绑定一个新的文件描述符，然后通过给read命令的-u参数指定从这个文件描述符读取。如下： <br/>

```bash
exec 3<file
while read -u 3 line;do echo "$line"; read -p "Press any key" -n 1;done
```

参考[^fn:4]。 <br/>
链接[^fn:5]中解释了 `1<&3-` 。 <br/>


## 字符串操作 {#字符串操作}


### 按照长度截取 {#按照长度截取}

通过 `${varibale:start:length}` 的方式，可以截取变量从位置start(下标从0开始)开始长度为length的字符串。如下示例： <br/>

```text
$ foo="asdfqwer"
$ echo ${foo:2:4}
dfqw
$ echo ${foo:1:4}
sdfq
$ echo ${foo:0:4}
asdf
$
```


### 按照内容截取 {#按照内容截取}

-   ${variable##\*string}，从左向右截取最后一个string后的字符串 <br/>
    ```text
    $ foo="asdfqwdfer"
    $ echo ${foo##*df}
    er
    ```
-   ${variable#\*string}，从左向右截取第一个string后的字符串 <br/>
    ```text
    $ foo="asdfqwdfer"
    $ echo ${foo#*df}
    qwdfer
    ```
-   ${variable%%string\*}，从右向左截取最后一个string后的字符串 <br/>
    ```text
    $ foo="asdfqwdfer"
    $ echo ${foo%%df*}
    as
    ```
-   ${variable%string\*}，从右向左截取第一个string后的字符串 <br/>
    ```text
    $ foo="asdfqwdfer"
    $ echo ${foo%df*}
    asdfqw
    ```

其中， `*` 只是一个通配符，可以不要。另外，这里面配置的 `*string` 、 `string*` 等截取模式实际上就是在字符串中将这些去掉，只要剩下的内容，这样，就好记了。 <br/>


## case语句 {#case语句}


### 基本语法 {#基本语法}

```text
case expression in
    pattern1)
	statement1
	;;
    pattern2)
	statement2
	;;
    pattern3)
	statement3
	;;
    ……
    *)
	statementn
esac
```

注意，这里用两个分号，分隔了各个语句块。从上往下依次匹配，命中一个之后，就执行对应的语句块，然后跳出case语句。如果都没有命中，就会执行最后的\*语句。 <br/>


### 场景示例 {#场景示例}

简单示例： <br/>

```text
$ i=1
$ case $i in 1) echo this is 1;; 2) echo this is 2;;esac
this is 1
$ i=2
$ case $i in 1) echo this is 1;; 2) echo this is 2;;esac
this is 2
$ 
```


## if条件判断 {#if条件判断}


### 基本语法 {#基本语法}

if语句的基本语法[^fn:6]如下： <br/>

```text
if [ condition ] 
then 
    command1 
    command2 
    ... 
fi
```

其中，condition是一个条件表达式，可以使用比较运算符（如-eq、-ne、-lt、-gt、-le、-ge）、逻辑运算符（如&amp;&amp;、||、!）等进行组合。例如： <br/>

```bash
if [ $a -eq $b ] 
then 
    echo "a equals b" 
fi
```


### 字符串判断 {#字符串判断}

字符串相等： <br/>

```bash
a=asdf
b=qwer
if [ $a = $b ]; then
 echo "a = b"
else
 echo "a !=b"
fi
```

字符串不相等： <br/>

```bash
a=asdf
b=qwer
if [ $a != $b ]; then
 echo "a != b"
else
 echo "a =b"
fi
```

字符串是否为空，长度是否为零： <br/>

```bash
a=asdf
# 字符串长度为0
if [ -z $a ]
then
   echo "字符串长度为0"
fi
# 字符串长度不为0
if [ -n "$a" ]
then
   echo "字符串长度不为0"
fi
# 字符串是否为空
if [ $a ]
then
   echo "字符串不为空"
else
   echo "字符串为空"
fi
```

字符串是否匹配正则： <br/>

```bash
str=123
if [[ $str =~ ^[0-9]+$ ]] 
then 
    echo "str is a number" 
fi
str=123
if [[ $str =~ ^[[:digit:]]+$ ]] 
then 
    echo "str is a number" 
fi
```

这里的正则是posix规范的[^fn:7]，而非一般的perl或者javascript[^fn:8]，如下： <br/>
![](/ox-hugo/01_RegexCompare.png) <br/>


### 数字大小 {#数字大小}

一般判断： <br/>

```bash
i=1
if [ $i -lt 3 ]; then
 echo "$i, little than 3"
fi
```

带else的判断： <br/>

```bash
i=5
if [ $i -lt 3 ]; then
 echo "$i, little than 3"
else
 echo "$i, not little than 3"
fi
```


### 条件判断中的关系运算 {#条件判断中的关系运算}


#### or {#or}

```bash
name="asdf"
if [ -d $name ] || [ -f $name ]; then
 echo "$name, exists."
else
 echo "$name, not exists."
fi
```


#### and {#and}

```bash
name="asdf"
if [ ! -d $name ] && [ ! -f $name ]; then
 echo "$name, not exists."
fi
```


#### 取反 {#取反}

条件中带取反的判断： <br/>

```bash
i=1
if [ ! $i -gt 3 ]; then
 echo "$i, not great than 3"
fi
```


### 文件和目录是否存在 {#文件和目录是否存在}

文件是否存在： <br/>

```bash
touch asdf
fname="asdf"
if [ -f $fname ]; then
 echo "$fname, file exists."
fi
```

目录是否存在： <br/>

```bash
mkdir asdf
dname="asdf"
if [ -d $dname ]; then
 echo "$dname, directory exists."
fi
```


## 循环 {#循环}


### while {#while}

参考[^fn:9] <br/>

```bash
i=1
while [ $i -lt 10 ]
do
echo $i;
i=$(($i+1));
done
```


### for {#for}

参考[^fn:9] <br/>

```bash
for ((i=1;i<10;i++)) ; do
echo $i
done
```


### break {#break}

```bash
for ((i=1;i<10;i++))
do
 if [ $i -lt 5 ]; then
  echo $i;
 else
  break
 fi
done
```


## 常用命令 {#常用命令}


### hostname {#hostname}


#### 常用选项 {#常用选项}

参考：[^fn:10] <br/>

-   -i, --ip-address, Display the IP address(es) of the host. <br/>
    显示ip地址。 <br/>


#### 经典场景 {#经典场景}

获取ip地址： <br/>

```text
hostname -i
```


### ls {#ls}


#### 常用选项 {#常用选项}

-   -a, Include directory entries whose names begin with a dot (‘.’). <br/>
    显示所有的文件，包括.开头的文件。 <br/>
-   -l, (The lowercase letter “ell”.) List files in the long format, as described in the The Long Format subsection below. <br/>
    以列表的方式显示结果，这里-l默认展示的是last modified time。 <br/>
-   -t, Sort by descending time modified (most recently modified first).  If two files have the same modification timestamp, sort their names in ascending lexicographical order.  The -r option reverses both of these sort orders. <br/>
    按照时间降序排序。 <br/>
-   -r, Reverse the order of the sort. <br/>
    结果反向排序。 <br/>
-   -c, Use time when file status was last changed for sorting or printing. <br/>
    指定显示change time，默认是修改时间。 <br/>
-   -u, Use time of last access, instead of time of last modification of the file for sorting (-t) or long printing (-l). <br/>
    指定显示上次访问时间，默认是修改时间。 <br/>
-   -s, Display the number of blocks used in the file system by each file. <br/>
    以块为单位列出文件占用的大小。 <br/>
-   -R, Recursively list subdirectories encountered. <br/>
    依次递归遍历全部子目录。 <br/>


#### 查看占用文件块的大小 {#查看占用文件块的大小}

```text
$ ls -sl .
total 24
 8 -rw-r--r--  1 chengxia  staff  2048 10 27 08:14 a.txt
16 -rw-r--r--  1 chengxia  staff  4608 10 27 08:15 all.tar
 0 drwxr-xr-x  5 chengxia  staff   160 10 27 08:20 all0
 0 drwxr-xr-x  5 chengxia  staff   160 10 26 11:13 all1
 0 -rw-r--r--  1 chengxia  staff     0 10 25 11:13 b.txt
 0 -rw-r--r--  1 chengxia  staff     0 10 27 11:13 c.txt
```

这里输出的total指的是文件块占用的多少。 <br/>


#### 指定时间格式 {#指定时间格式}

在苹果电脑上，需要用下方式： <br/>

```text
$ ls -alrt -D "%Y-%m-%d %H:%M:%S" ./React/
total 48
-rw-r--r--@  1 chengxia  staff   1338 2022-09-12 23:00:44 01_single-file-example.html
drwxr-xr-x   2 chengxia  staff     64 2022-09-13 17:10:51 src
-rw-r--r--   1 chengxia  staff    423 2022-11-03 23:14:30 NodeJS.iml
-rw-r--r--   1 chengxia  staff    594 2022-11-04 19:10:04 React.iml
drwxr-xr-x   3 chengxia  staff     96 2022-11-05 23:18:40 00_NodeJSHelloWorld
drwxr-xr-x   9 chengxia  staff    288 2022-11-18 23:48:40 00_React
-rw-r--r--@  1 chengxia  staff  10244 2022-11-24 08:18:37 .DS_Store
drwxr-xr-x   9 chengxia  staff    288 2022-12-20 21:59:45 00_NodeJSTrial
drwxr-xr-x   4 chengxia  staff    128 2022-12-27 09:03:18 02_ReactLikeButtonOnClick
drwxr-xr-x  12 chengxia  staff    384 2023-03-13 23:30:09 .
drwxr-xr-x   6 chengxia  staff    192 2023-03-13 23:31:12 00_WebpackTrial
drwxr-xr-x  24 chengxia  staff    768 2023-09-20 19:33:50 ..
ChengdeMacBook-Pro:TrialProject chengxia$
```

在公司win电脑的git bash上，需要用如下格式： <br/>

```text
ls -alrt --time-style="+%Y-%m-%d %H:%M:%S" ./React/
```


#### 遍历全部子目录 {#遍历全部子目录}

```text
$ tree tar_test
tar_test
├── a.txt
├── all.tar
├── all0
│   ├── a.txt
│   ├── b.txt
│   └── c.txt
├── all1
│   ├── a.txt
│   ├── b.txt
│   └── c.txt
├── b.txt
├── c.txt
└── tar_test2
    ├── a.txt
    ├── all.tar
    ├── asdf
    │   ├── a.txt
    │   ├── b.txt
    │   └── c.txt
    ├── b.txt
    └── c.txt

4 directories, 17 files
$ ls -Rlt tar_test
total 24
-rw-r--r--  1 chengxia  staff     0 11  8 02:11 c.txt
-rw-r--r--  1 chengxia  staff     0 11  8 02:11 b.txt
-rw-r--r--  1 chengxia  staff     5 11  8 02:11 a.txt
drwxr-xr-x  8 chengxia  staff   256 11  6 02:37 tar_test2
drwxr-xr-x  5 chengxia  staff   160 10 27 08:20 all0
-rw-r--r--  1 chengxia  staff  4608 10 27 08:15 all.tar
drwxr-xr-x  5 chengxia  staff   160 10 26 11:13 all1

tar_test/tar_test2:
total 16
drwxr-xr-x  5 chengxia  staff   160 11  6 02:32 asdf
-rw-r--r--  1 chengxia  staff  3072 11  6 02:28 all.tar
-rw-r--r--  1 chengxia  staff     7 11  6 02:27 a.txt
-rw-r--r--  1 chengxia  staff     0 11  6 02:27 b.txt
-rw-r--r--  1 chengxia  staff     0 11  6 02:27 c.txt

tar_test/tar_test2/asdf:
total 8
-rw-r--r--  1 chengxia  staff  7 11  6 02:27 a.txt
-rw-r--r--  1 chengxia  staff  0 11  6 02:27 b.txt
-rw-r--r--  1 chengxia  staff  0 11  6 02:27 c.txt

tar_test/all0:
total 8
-rw-r--r--  1 chengxia  staff     0 10 27 11:13 c.txt
-rw-r--r--  1 chengxia  staff  2048 10 27 08:14 a.txt
-rw-r--r--  1 chengxia  staff     0 10 25 11:13 b.txt

tar_test/all1:
total 8
-rw-r--r--  1 chengxia  staff     0 10 27 11:13 c.txt
-rw-r--r--  1 chengxia  staff  2053 10 26 11:03 a.txt
-rw-r--r--  1 chengxia  staff     0 10 25 11:13 b.txt
$ 
```

类似地，检查每个子目录下有哪些文件： <br/>

```text
$ find tar_test -type d ! -name '.' | while read dir_name; do echo "$dir_name: $(ls $dir_name | wc -l)";done
tar_test:        7
tar_test/all0:        3
tar_test/all1:        3
tar_test/tar_test2:        5
tar_test/tar_test2/asdf:        3
$
```


### date {#date}

获取当前时间： <br/>

```text
$ date
2023年10月27日 星期五 10时35分52秒 CST
$ date "+DATE: %Y-%m-%d%nTIME: %H:%M:%S"
DATE: 2023-10-27
TIME: 10:35:55
$ date "+%Y%m%d_%H%M%S"
20231027_105958
```

也可以通过%N来获取到纳秒(毫秒的千分之一)，但是macOS上不支持[^fn:11]。 <br/>


### find {#find}


#### 查找修改时间在15天以内的文件 {#查找修改时间在15天以内的文件}

```text
find . -maxdepth 1 -type f -mtime -15
```

mtime说明[^fn:12]： <br/>
如何更好的理解find -mtime +N/-N/N，这里小结下： <br/>
-mtime n : n为数字，意思为在n天之前的“一天之内”被更改过内容的文件 <br/>
-mtime +n : 列出在n天之前（不含n天本身）被更改过内容的文件名 <br/>
-mtime -n : 列出在n天之内（含n天本身）被更改过内容的文件名 <br/>
举个栗子：find $HOME -mtime 0 <br/>
Search  for  files  in  your home directory which have been modified in the last twenty-four hours.  This command works this way because the time since each file was last modified is divided by 24 hours and  any remainder  is  discarded.   That means that to match -mtime 0, a file will have to have a modification in the past which is less than 24 hours ago. <br/>
将根目录下24小时内更改过内容的文件列出： <br/>
find / -mtime 0 <br/>
场景举例： <br/>
找“5天之内被更改过的档案名”find / -mtime -5 ； <br/>
找“5天前的那一天被更改过的档案名”find / -mtime 5 ； <br/>
找“5天之前被更改过的档案名”find / -mtime +5。 <br/>


#### 按照文件名查找 {#按照文件名查找}

查找文件名中不包含as的文件： <br/>

```bash
find . -maxdepth 1 -type f ! -name "*as*"
```

查找文件名中包含as的文件： <br/>

```bash
find . -maxdepth 1 -type f -name "*as*"
```


#### 打印时间 {#打印时间}

参考[^fn:13] <br/>

```bash
find . -maxdepth 1 -type f -printf "%p %TY-%Tm-%Td %TH:%TM:%TS %Tz\n"
```

这里面%p表示文件名，%T表示是修改时间，可以换成%C表示拿到状态改变的时间，换成%A拿到上次访问的时间[^fn:14]。 <br/>
不过，macos中的find命令，并不支持-printf选项[^fn:15]。 <br/>


#### 查找某个时间区间内的文件 {#查找某个时间区间内的文件}

```text
$ ls -lt -D "%Y-%m-%d %H:%M:%S" *.txt
-rw-r--r--  1 chengxia  staff  0 2023-11-08 02:11:53 c.txt
-rw-r--r--  1 chengxia  staff  0 2023-11-08 02:11:33 b.txt
-rw-r--r--  1 chengxia  staff  5 2023-11-08 02:11:23 a.txt
$ find . -maxdepth 1 -type f -name "*.txt" -newermt '2023-11-08 02:11:10' ! -newermt '2023-11-08 02:11:40'
./b.txt
./a.txt
$
```


#### find命令后跟多个操作 {#find命令后跟多个操作}

可以对于每条查找到的文件结果，执行多条exec操作，如下： <br/>

```text
$ ls *.txt
Math.txt	Mathv.txt	a.txt
$ find . -maxdepth 1 -type f -name "*.txt" -exec echo "filename:"{} \; -exec echo filename2nd:{} \;
filename:./Mathv.txt
filename2nd:./Mathv.txt
filename:./a.txt
filename2nd:./a.txt
filename:./Math.txt
filename2nd:./Math.txt
$
```

参考[^fn:16] <br/>


### tar {#tar}

tar命令的使用 <br/>
tar(英文全拼：tape archive)命令用于备份文件。tar是用来建立，还原备份文件的工具程序，它可以加入，解开备份文件内的文件。[^fn:17] <br/>


#### 常用选项说明 {#常用选项说明}

-   -c, Create a new archive containing the specified items. The long option form is --create. <br/>
    创建tar包。 <br/>
-   -v, --verbose, Produce verbose output.  In create and extract modes, tar will list each file name as it is read from or written to the archive.  In list mode, tar will produce output similar to that of ls(1).  An additional -v option will also provide ls-like details in create and extract mode. <br/>
    verbose模式，会打印tar操作的文件等信息。 <br/>
-   -f file, --file file, Read the archive from or write the archive to the specified file. <br/>
    指定打包或者解压的包名。 <br/>
-   -r, Like -c, but new entries are appended to the archive.  Note that this only works on uncompressed archives stored in regular files. The -f option is required.  The long option form is --append. <br/>
    将文件追加到tar包末尾。 <br/>
-   -u, Like -r, but new entries are added only if they have a modification date newer than the corresponding entry in the archive.  Note that this only works on uncompressed archives stored in regular files.  The -f option is required.  The long form is --update. <br/>
    只有当要添加的文件，比tar包中原来的文件修改时间新时，才将这个文件添加到tar包中。 <br/>
-   -t, List archive contents to stdout. The long option form is --list. <br/>
    查看tar包中的内容。 <br/>
-   -C directory, --cd directory, --directory directory, In c and r mode, this changes the directory before adding the following files.  In x mode, change directories after opening the archive but before extracting entries from the archive. <br/>
    一般用来指定解压文件的目的目录。 <br/>
-   -x, Extract to disk from the archive.  If a file with the same name appears more than once in the archive, each copy will be extracted, with later copies overwriting (replacing) earlier copies.  The long option form is --extract. <br/>
    解压指定的tar包。 <br/>
-   -k, --keep-old-files, (x mode only) Do not overwrite existing files.  In particular, if a file appears more than once in an archive, later copies will not overwrite earlier copies. <br/>
    在解包时，如果在解压的目的目录，已经有了同名文件，不进行覆盖。如果一个包中，同名文件出现了多次，后面的文件也不覆盖前面的文件。 <br/>
-   --keep-newer-files, (x mode only) Do not overwrite existing files that are newer than the versions appearing in the archive being extracted. <br/>


#### 打包 {#打包}


##### 简单将同一目录下的多个文件打成一个tar {#简单将同一目录下的多个文件打成一个tar}

```text
$ ls -alt
total 8
drwxr-xr-x   5 chengxia  staff   160 10 26 09:40 .
-rw-r--r--   1 chengxia  staff  2053 10 25 22:43 a.txt
-rw-r--r--   1 chengxia  staff     0 10 25 22:40 c.txt
-rw-r--r--   1 chengxia  staff     0 10 25 22:40 b.txt
drwxr-xr-x  19 chengxia  staff   608 10 25 22:38 ..
$ tar -cvf all.tar *.txt
a a.txt
a b.txt
a c.txt
$ ls -alt
total 24
-rw-r--r--   1 chengxia  staff  5120 10 26 09:41 all.tar
drwxr-xr-x   6 chengxia  staff   192 10 26 09:41 .
-rw-r--r--   1 chengxia  staff  2053 10 25 22:43 a.txt
-rw-r--r--   1 chengxia  staff     0 10 25 22:40 c.txt
-rw-r--r--   1 chengxia  staff     0 10 25 22:40 b.txt
drwxr-xr-x  19 chengxia  staff   608 10 25 22:38 ..
$
```


##### 查看tar包中的文件列表 {#查看tar包中的文件列表}

```text
$ tar -tvf all.tar 
-rw-r--r--  0 chengxia staff    2053 10 25 22:43 a.txt
-rw-r--r--  0 chengxia staff       0 10 25 22:40 b.txt
-rw-r--r--  0 chengxia staff       0 10 25 22:40 c.txt
$
```


##### 往存量tar中添加文件 {#往存量tar中添加文件}

```text
$ touch d.txt
$ ls -al
total 24
drwxr-xr-x   7 chengxia  staff   224 10 26 09:52 .
drwxr-xr-x  19 chengxia  staff   608 10 25 22:38 ..
-rw-r--r--   1 chengxia  staff  2053 10 25 22:43 a.txt
-rw-r--r--   1 chengxia  staff  5120 10 26 09:41 all.tar
-rw-r--r--   1 chengxia  staff     0 10 25 22:40 b.txt
-rw-r--r--   1 chengxia  staff     0 10 25 22:40 c.txt
-rw-r--r--   1 chengxia  staff     0 10 26 09:52 d.txt
$ tar -rvf all.tar d.txt 
a d.txt
$ tar -tvf all.tar 
-rw-r--r--  0 chengxia staff    2053 10 25 22:43 a.txt
-rw-r--r--  0 chengxia staff       0 10 25 22:40 b.txt
-rw-r--r--  0 chengxia staff       0 10 25 22:40 c.txt
-rw-r--r--  0 chengxia staff       0 10 26 09:52 d.txt
$
```

但是，这里需要注意，如果tar包中已经有了同名文件，这个添加操作，不会覆盖其中的文件，只是在tar包中添加一个同名文件： <br/>

```text
$ ls -alt 
total 24
-rw-r--r--   1 chengxia  staff  5632 10 26 09:53 all.tar
drwxr-xr-x   7 chengxia  staff   224 10 26 09:52 .
-rw-r--r--   1 chengxia  staff     0 10 26 09:52 d.txt
-rw-r--r--   1 chengxia  staff  2053 10 25 22:43 a.txt
-rw-r--r--   1 chengxia  staff     0 10 25 22:40 c.txt
-rw-r--r--   1 chengxia  staff     0 10 25 22:40 b.txt
drwxr-xr-x  19 chengxia  staff   608 10 25 22:38 ..
$ echo "this is d.txt" > d.txt 
$ ls -alt 
total 32
-rw-r--r--   1 chengxia  staff    14 10 26 09:56 d.txt
-rw-r--r--   1 chengxia  staff  5632 10 26 09:53 all.tar
drwxr-xr-x   7 chengxia  staff   224 10 26 09:52 .
-rw-r--r--   1 chengxia  staff  2053 10 25 22:43 a.txt
-rw-r--r--   1 chengxia  staff     0 10 25 22:40 c.txt
-rw-r--r--   1 chengxia  staff     0 10 25 22:40 b.txt
drwxr-xr-x  19 chengxia  staff   608 10 25 22:38 ..
$ tar -rvf all.tar d.txt 
a d.txt
$ tar -tvf all.tar 
-rw-r--r--  0 chengxia staff    2053 10 25 22:43 a.txt
-rw-r--r--  0 chengxia staff       0 10 25 22:40 b.txt
-rw-r--r--  0 chengxia staff       0 10 25 22:40 c.txt
-rw-r--r--  0 chengxia staff       0 10 26 09:52 d.txt
-rw-r--r--  0 chengxia staff      14 10 26 09:56 d.txt
$ 
```

如果想实现效果：只有当要添加的文件，比tar包中原来的文件修改时间新时，才将这个文件添加到tar包中，需要使用-u选项。 <br/>

```text
$ tar -tvf all.tar 
-rw-r--r--  0 chengxia staff    2053 10 25 22:43 a.txt
-rw-r--r--  0 chengxia staff       0 10 25 22:40 b.txt
-rw-r--r--  0 chengxia staff       0 10 25 22:40 c.txt
-rw-r--r--  0 chengxia staff       0 10 26 09:52 d.txt
-rw-r--r--  0 chengxia staff      14 10 26 09:56 d.txt
-rw-r--r--  0 chengxia staff    2053 10 20 12:23 a.txt

# 更新文件修改时间为一个旧的时间点
$ touch -m -t 202310191113.33 b.txt 
$ ls -alt
total 40
-rw-r--r--   1 chengxia  staff  9728 10 26 10:04 all.tar
-rw-r--r--   1 chengxia  staff    14 10 26 09:56 d.txt
drwxr-xr-x   7 chengxia  staff   224 10 26 09:52 .
-rw-r--r--   1 chengxia  staff     0 10 25 22:40 c.txt
drwxr-xr-x  19 chengxia  staff   608 10 25 22:38 ..
-rw-r--r--   1 chengxia  staff  2053 10 20 12:23 a.txt
-rw-r--r--   1 chengxia  staff     0 10 19 11:13 b.txt

# 通过指定-u选项试图将b.txt再添加到tar包中，会发现，实际加不进去，因为时间戳老
$ tar -uvf all.tar b.txt 
$ tar -tvf all.tar 
-rw-r--r--  0 chengxia staff    2053 10 25 22:43 a.txt
-rw-r--r--  0 chengxia staff       0 10 25 22:40 b.txt
-rw-r--r--  0 chengxia staff       0 10 25 22:40 c.txt
-rw-r--r--  0 chengxia staff       0 10 26 09:52 d.txt
-rw-r--r--  0 chengxia staff      14 10 26 09:56 d.txt
-rw-r--r--  0 chengxia staff    2053 10 20 12:23 a.txt
$ touch -m -t 202410191113.33 b.txt 
$ tar -uvf all.tar b.txt 
a b.txt
$ tar -tvf all.tar 
-rw-r--r--  0 chengxia staff    2053 10 25 22:43 a.txt
-rw-r--r--  0 chengxia staff       0 10 25 22:40 b.txt
-rw-r--r--  0 chengxia staff       0 10 25 22:40 c.txt
-rw-r--r--  0 chengxia staff       0 10 26 09:52 d.txt
-rw-r--r--  0 chengxia staff      14 10 26 09:56 d.txt
-rw-r--r--  0 chengxia staff    2053 10 20 12:23 a.txt
-rw-r--r--  0 chengxia staff       0 10 19  2024 b.txt
$ 
```

注意，这里tar不支持在打包的时候，覆盖包中已有的同名文件。原因可能是因为，这个命令诞生于磁带打包场景，磁带这种存储介质，只适合追加写，更新前面的内容(随记写)效率不佳。参考[^fn:18] <br/>


#### 查看压缩包中的内容 {#查看压缩包中的内容}

```text
$ tar -tvf all.tar 
-rw-r--r--  0 chengxia staff       7 11  6 02:27 a.txt
-rw-r--r--  0 chengxia staff       0 11  6 02:27 b.txt
-rw-r--r--  0 chengxia staff       0 11  6 02:27 c.txt
```


#### 解包 {#解包}


##### 解压到当前目录 {#解压到当前目录}

```text
$ touch a.txt b.txt c.txt
$ echo in_tar > a.txt 
a$ tar -cvf all.tar *.txt
a a.txt
a b.txt
a c.txt
$ echo out_of_tar > a.txt
$ cat a.txt 
out_of_tar
# 指定不覆盖当前文件
$ tar -xkvf all.tar 
x a.txt
x b.txt
x c.txt
$ cat a.txt 
out_of_tar
# 默认会覆盖当前文件
$ tar -xvf all.tar 
x a.txt
x b.txt
x c.txt
$ cat a.txt 
in_tar
$
```


##### 解压到指定目录 {#解压到指定目录}

```text
$ tar -tvf all.tar 
-rw-r--r--  0 chengxia staff       7 11  6 02:27 a.txt
-rw-r--r--  0 chengxia staff       0 11  6 02:27 b.txt
-rw-r--r--  0 chengxia staff       0 11  6 02:27 c.txt
$ tar -xvf all.tar -C asdf
x a.txt
x b.txt
x c.txt
$ ls asdf/
a.txt	b.txt	c.txt
$ ls -lt asdf/
total 8
-rw-r--r--  1 chengxia  staff  7 11  6 02:27 a.txt
-rw-r--r--  1 chengxia  staff  0 11  6 02:27 b.txt
-rw-r--r--  1 chengxia  staff  0 11  6 02:27 c.txt
$ 
```


### cut {#cut}


#### 常用选项 {#常用选项}

-   -d delim, Use delim as the field delimiter character instead of the tab character. <br/>
    指定分隔符，默认的分隔符是tab。 <br/>
-   -b list, The list specifies byte positions. <br/>
    指定按照字节截取。 <br/>
-   -c list, The list specifies character positions. <br/>
    指定按照字符截取。 <br/>
-   -f list, The list specifies fields, separated in the input by the field delimiter character (see the -d option).  Output fields are separated by a single occurrence of the field delimiter character. <br/>
    指定输出的字段，默认输出字段之间用一个分隔符分隔。 <br/>
-   -n Do not split multi-byte characters.  Characters will only be output if at least one byte is selected, and, after a prefix of zero or more unselected bytes, the rest of the bytes that form the character are selected. <br/>
    和-b选项一起使用。指定不拆散多字节字符，只有当多字节字符的全部字符都被选中时，才输出该字符。否则，不输出。 <br/>
-   -w Use whitespace (spaces and tabs) as the delimiter.  Consecutive spaces and tabs count as one single field separator. <br/>
    指定连续的空白字符作为分隔符，连续的空格或者tab被认为是一个分隔符。这在处理类似ls的输出时比较有用。 <br/>


#### 典型场景 {#典型场景}

根据tab分隔： <br/>

```text
$ echo -e "asdf\tqwer\ttyui" | cut -f 1,2
asdf	qwer
```

这里echo命令的-e选项是为了启用反斜杠转义[^fn:19]。 <br/>

指定分隔符为空格： <br/>

```text
$ echo "asdf qwer tyui" | cut -d " " -f 1,3
asdf tyui
```

指定按照字节或则字符截取： <br/>

```text
$ echo "asdf qwer tyui" | cut -b 1-4,6-9
asdfqwer
$ echo "asdf qwer tyui" | cut -c 1-4,6-9
asdfqwer
$ echo "我asdf qwer tyui" | cut -c 1-4,6-9
我asd qwe
$ echo "我asdf qwer tyui" | cut -b 1-4,6-9
我adf q
```

指定不分割多字节字符： <br/>

```text
$ echo "我asdf qwer tyui" | cut -n -b 1-1

$ echo "我asdf qwer tyui" | cut -n -b 1-2

$ echo "我asdf qwer tyui" | cut -n -b 1-3
我
```

指定连续的空白字符为分隔符： <br/>

```text
$ echo -e "asdf\t\tqwer tyui" | cut -f 1,2
asdf	
$ echo -e "asdf\t\tqwer tyui" | cut -w -f 1,2
asdf	qwer
$ echo -e "asdf\t\tqwer tyui" | cut -w -f 1,2,3
asdf	qwer	tyui
```

获取ls命令输出的各个字段： <br/>

```text
$ ls -al
total 24
drwxr-xr-x   8 chengxia  staff   256 10 26 11:05 .
drwxr-xr-x  21 chengxia  staff   672 10 27 10:17 ..
-rw-r--r--   1 chengxia  staff  2048 10 27 08:14 a.txt
-rw-r--r--   1 chengxia  staff  4608 10 27 08:15 all.tar
drwxr-xr-x   5 chengxia  staff   160 10 27 08:20 all0
drwxr-xr-x   5 chengxia  staff   160 10 26 11:13 all1
-rw-r--r--   1 chengxia  staff     0 10 25 11:13 b.txt
-rw-r--r--   1 chengxia  staff     0 10 27 11:13 c.txt
$ ls -al | cut -w -f 1,3,5,7,9,10
total
drwxr-xr-x	chengxia	256	26	.
drwxr-xr-x	chengxia	672	27	..
-rw-r--r--	chengxia	2048	27	a.txt
-rw-r--r--	chengxia	4608	27	all.tar
drwxr-xr-x	chengxia	160	27	all0
drwxr-xr-x	chengxia	160	26	all1
-rw-r--r--	chengxia	0	25	b.txt
-rw-r--r--	chengxia	0	27	c.txt
```

不过，处理ls命令的输出，还是结合awk命令使用比较好[^fn:20]。如下： <br/>

```text
$ ls -al
total 24
drwxr-xr-x   8 chengxia  staff   256 10 26 11:05 .
drwxr-xr-x  21 chengxia  staff   672 10 27 10:17 ..
-rw-r--r--   1 chengxia  staff  2048 10 27 08:14 a.txt
-rw-r--r--   1 chengxia  staff  4608 10 27 08:15 all.tar
drwxr-xr-x   5 chengxia  staff   160 10 27 08:20 all0
drwxr-xr-x   5 chengxia  staff   160 10 26 11:13 all1
-rw-r--r--   1 chengxia  staff     0 10 25 11:13 b.txt
-rw-r--r--   1 chengxia  staff     0 10 27 11:13 c.txt
$ ls -al | awk '{print $1,$3}'
total 
drwxr-xr-x chengxia
drwxr-xr-x chengxia
-rw-r--r-- chengxia
-rw-r--r-- chengxia
drwxr-xr-x chengxia
drwxr-xr-x chengxia
-rw-r--r-- chengxia
-rw-r--r-- chengxia
$ ls -al | awk '{print $1$3}'
total
drwxr-xr-xchengxia
drwxr-xr-xchengxia
-rw-r--r--chengxia
-rw-r--r--chengxia
drwxr-xr-xchengxia
drwxr-xr-xchengxia
-rw-r--r--chengxia
-rw-r--r--chengxia
$ ls -al | awk '{print "权限: "$1", 用户: "$3}'
权限: total, 用户: 
权限: drwxr-xr-x, 用户: chengxia
权限: drwxr-xr-x, 用户: chengxia
权限: -rw-r--r--, 用户: chengxia
权限: -rw-r--r--, 用户: chengxia
权限: drwxr-xr-x, 用户: chengxia
权限: drwxr-xr-x, 用户: chengxia
权限: -rw-r--r--, 用户: chengxia
权限: -rw-r--r--, 用户: chengxia
```

这里awk命令的用法可以参考对应的[章节](#awk_command_introduction)。 <br/>


### read {#read}


#### 选项 {#选项}

参考[^fn:21]： <br/>

```text
read [-a aname] [-d delim] [-n nchars]
    [-N nchars] [-p prompt] [-t timeout] [-u fd] [name …]

-a arr
读取数据保存到数组arr中，arr数组已存在则先清空

-d delim
指定读取数据时的分隔符，不指定时默认为换行符。当指定为空字符串时，则默认为\0。当指定为文件中不存在的符号，则直接读整个文件

-n X
每次读取X个字符，但如果读满X字符前遇到了分隔符，则也停止本次读取

-N X
每次读取X个字符，即使遇到了分隔符也不停止

-p prompt
交互式提示用户在终端输入的信息，默认不输出换行符

-s
用户在终端中的输入不会显示出来。在提示用户输入密码时应设置该选项

-r
使得读取到的反斜线不进行转义，而是作为一个普通反斜线字符 

-t timeout
超时时间，如果在超时时间内还未读满数据，则读取失败，可指定为小数时间

-u fd
从文件描述符中读取数据
```


#### 场景 {#场景}

默认，可以直接将用户输入读入一个变量： <br/>

```text
# read asdf
this is value of asdf
# echo $asdf          
this is value of asdf
#
```


### cp {#cp}

一般来说，cp命令会覆盖掉原有同名文件，通过指定-n选项可以，不覆盖： <br/>

```text
# 创建一个空文件
$ touch test.txt
# 在asdf目录创建一个有内容的test.txt文件
$ echo 'asdf' > asdf/test.txt
$ cat asdf/test.txt
asdf

# 默认将test.txt复制到asdf目录，会覆盖
$ cp test.txt asdf/
# 有内容的文件已经被空文件盖掉了
$ cat asdf/test.txt

# 在asdf目录创建一个有内容的test.txt文件
$ echo 'asdf' > asdf/test.txt
# cp时，指定-n选项，这样，不会覆盖原文件
$ cp -n test.txt asdf/
$ cat asdf/test.txt
asdf
$
```


### cat {#cat}

cat命令如果后面跟一个文件名，可以查看文件内容。如果没有跟任何内容，会从标准输入读取文件，并输出。如下： <br/>

```text
$ cat a.txt 
asdf
$ cat
This is a input line of user.
This is a input line of user.
This is another input line of user.
This is another input line of user.
^C
$ echo 'foo bar' | cat
foo bar
$
```


### touch {#touch}


#### 选项 {#选项}

-   -m, Change the modification time of the file.  The access time of the file is not changed unless the -a flag is also specified. <br/>
    改变文件的修改时间。 <br/>
-   -a, Change the access time of the file.  The modification time of the file is not changed unless the -m flag is also specified. <br/>
    改变文件的上次访问时间。 <br/>
-   -t, Change the access and modification times to the specified time instead of the current time of day. <br/>
    The argument is of the form “[[CC]YY]MMDDhhmm[.SS]” where each pair of letters represents the following: <br/>
    
    -   CC      The first two digits of the year (the century). <br/>
    -   YY      The second two digits of the year.  If “YY” is specified, but “CC” is not, a value for “YY” between 69 and 99 results in a “CC” value of 19.  Otherwise, a “CC” value of 20 is used. <br/>
    -   MM      The month of the year, from 01 to 12. <br/>
    -   DD      the day of the month, from 01 to 31. <br/>
    -   hh      The hour of the day, from 00 to 23. <br/>
    -   mm      The minute of the hour, from 00 to 59. <br/>
    -   SS      The second of the minute, from 00 to 60. <br/>
    
    If the “CC” and “YY” letter pairs are not specified, the values default to the current year.  If the “SS” letter pair is not specified, the value defaults to 0. <br/>
    指定修改的具体时间，默认是当前时间。 <br/>


#### 场景 {#场景}

改变文件的修改时间为指定时间(macOS测过)： <br/>

```text
$ ls -alrt -D "%Y-%m-%d %H:%M:%S" a.txt 
-rw-r--r--  1 chengxia  staff  2048 2023-10-27 08:14:55 a.txt
$ touch -m -t 202310291111.23 a.txt 
$ ls -alrt -D "%Y-%m-%d %H:%M:%S" a.txt 
-rw-r--r--  1 chengxia  staff  2048 2023-10-29 11:11:23 a.txt
$
```


### uniq {#uniq}


#### 场景 {#场景}

对于排好序的文本行统计重复次数： <br/>

```bash
$ echo "apple
apple
pear
apple
pear
cherry" | sort 
apple
apple
apple
cherry
pear
pear
$ echo "apple
apple
pear
apple
pear
cherry" | sort | uniq -c
   3 apple
   1 cherry
   2 pear
$
```

如果不排序，结果是不符合预期的： <br/>

```bash
$ echo "apple
apple
pear
apple
pear
cherry" | uniq -c
   2 apple
   1 pear
   1 apple
   1 pear
   1 cherry
$
```


### awk {#awk_command_introduction}


#### 场景 {#场景}


##### 输出行中的指定字段 {#输出行中的指定字段}

$0代表整行，$n代表第n个字段，分隔符默认是1个或多个连续的空白字符。 <br/>

```bash
$ echo "a line 1
b line 2
c line 3
d line 4" | awk '{print $1,$3}'
a 1
b 2
c 3
d 4
$ echo "a line 1
b line 2
c line 3
d line 4" | awk '{print "field 1:"$1", field 2:"$2,"field 3:"$3", whole line: "$0}'
field 1:a, field 2:line field 3:1, whole line: a line 1
field 1:b, field 2:line field 3:2, whole line: b line 2
field 1:c, field 2:line field 3:3, whole line: c line 3
field 1:d, field 2:line field 3:4, whole line: d line 4
$
```


##### 输出文件中的指定行 {#输出文件中的指定行}


###### 行号筛选 {#行号筛选}

参考[^fn:22] <br/>

-   筛选指定的行号 <br/>
    ```text
    $ cat a.txt 
    a a,b,d,f
    b alsdjf,apple,kdjf
    c 163.2.201.1
    d 163.2.201.1
    
    # 打印第1到3行
    $ awk 'NR==1,NR==3{print}' a.txt 
    a a,b,d,f
    b alsdjf,apple,kdjf
    c 163.2.201.1
    
    # 打印第2行及其之后的行
    $ awk 'NR>=2{print}' a.txt 
    b alsdjf,apple,kdjf
    c 163.2.201.1
    d 163.2.201.1
    
    # 打印奇数行
    $ awk '(NR%2)==1{print}' a.txt 
    a a,b,d,f
    c 163.2.201.1
    # 打印偶数行
    $ awk '(NR%2)==0{print}' a.txt 
    b alsdjf,apple,kdjf
    d 163.2.201.1
    $
    ```
-   行号关系运算 <br/>
    ```bash
    $ echo "a line 1
    b line 2
    c line 3
    d line 4" | awk 'NR==1||NR==3{print}'
    a line 1
    c line 3
    $ echo "a line 1
    b line 2
    c line 3
    d line 4" | awk 'NR%2==1&&NR==3{print}'
    c line 3
    $  echo "a line 1
    b line 2
    c line 3
    d line 4" | awk '(NR%2==1)&&(NR==3){print}'
    c line 3
    $
    ```


###### 根据内容筛选 {#根据内容筛选}

-   打印包含指定内容的行 <br/>
    ```bash
    $ echo "cat is lovely.
    b line 2
    c line 3
    cat does not like dog" | awk '/cat/{print}'
    cat is lovely.
    cat does not like dog
    $
    ```
-   打印包含正则命中的行 <br/>
    正则或的例子： <br/>
    ```bash
    $ echo "cat is lovely.
    b line 02
    c line 03
    cat does not like dog" | awk '/line (02|03)$/{print}'
    b line 02
    c line 03
    $
    ```
    打印数字结尾的行： <br/>
    ```bash
    $ echo "cat is lovely.
    b line 2
    c line 3
    cat does not like dog" | awk '/[0-9]+$/{print}'
    b line 2
    c line 3
    $
    ```
    打印非数字结尾的行： <br/>
    ```bash
    $ echo "cat is lovely.
    b line 2
    c line 3
    cat does not like dog" | awk '! /[0-9]+$/{print}'
    cat is lovely.
    cat does not like dog
    $
    ```
    注意，这里在macOS和Ubuntu上叹号后面必须要带空格(在有些其它系统上不带空格也可以正常运行)，不然执行会报错如下： <br/>
    ```bash
    $ echo "cat is lovely.
    b line 2
    c line 3
    cat does not like dog" | awk '!/[0-9]+$/{print}'
    -bash: !/[0-9]+$/{print}: event not found
    > 
    $
    ```
    具体，我没有找到原因。不过加空格肯定是好使的。 <br/>
    
    正则表达式多个条件组合： <br/>
    ```bash
    $ echo "cat is lovely.
    b line 2
    c line 3
    cat does not like dog" | awk '! /[0-9]+$/ && ! /not/{print}'
    cat is lovely.
    $ echo "cat is lovely.
    b line 2
    c line 3
    cat does not like dog" | awk '(! /[0-9]+$/) && (! /not/){print}'
    cat is lovely.
    $
    ```
-   打印包含从命中正则1到命中正则2之间的行[^fn:23] <br/>
    如下是一个例子，可以看出，都不包含正则命中的行： <br/>
    ```bash
    $ echo "cat is lovely.
    b line 02
    b line 02
    asdfa
    xxxyy
    asdfa
    b line 03
    c line 03
    cat does not like dog" | awk '/line (02)$/{flag=1;next}/line (03)$/{flag=0}flag'
    asdfa
    xxxyy
    asdfa
    $ echo "cat is lovely.
    b line 02
    b line 02
    asd fa
    xx xyy
    as dfa
    b line 03
    c line 03
    cat does not like dog" | awk '/line (02)$/{flag=1;next}/line (03)$/{flag=0}flag {print $1,$2,$0}'
    asd fa asd fa
    xx xyy xx xyy
    as dfa as dfa
    $ 
    ```


##### 输出指定行的指定字段 {#输出指定行的指定字段}

输出不包含cat文本行的第一个和第三个字段： <br/>

```bash
$ echo "cat is lovely.
b line 2
c line 3
cat does not like dog" | awk '! /cat/{print $1,$3}'
b 2
c 3
$ echo "cat is lovely.
b line 2
c line 3
cat does not like dog" | awk '! /cat/{print "field 1,"$1"; field 3,"$3}'
field 1,b; field 3,2
field 1,c; field 3,3
$
```


#### 奇怪问题 {#奇怪问题}

在测试时，如果打印了最后一个字段或者整行，再后面如果再拼接其他字符串，其他字符串会出现在行首，并把前面的内容盖掉。经查，这个文件时windows上生成的行分隔符是\r\n。这样，行的最后，会有个多余的\r，导致后面的内容在前面输出，覆盖掉前面内容。如下： <br/>

```bash
$ echo -e "line 1\r\nline 2\r\nline 3\r" | awk '{print $0",xx"}'
,xxe 1
,xxe 2
,xxe 3
$ 
```

解决这个问题的方式，是把最后的\r去掉，如下： <br/>

```bash
$ echo -e "line 1\r\nline 2\r\nline 3\r" | awk '{gsub(/[\r]+$/,"",$0);print $0",xx"}'
line 1,xx
line 2,xx
line 3,xx
$
```


### comm {#comm}


#### 常用选项 {#常用选项}

用法示例： <br/>

```text
comm [-123i] file1 file2
```

-   -1, Suppress printing of column 1, lines only in file1. <br/>
    去掉只在file1中有的行。 <br/>
-   -2, Suppress printing of column 2, lines only in file2. <br/>
    去掉只在file2中有的行。 <br/>
-   -3, Suppress printing of column 3, lines common to both. <br/>
    去掉两个文件中都有的行。 <br/>
-   -i, Case insensitive comparison of lines. <br/>
    在比对时，忽略大小写。 <br/>

实际上comm在不加任何选项时，会把比较的两个中的行处理成三列，第一列是只在file1中有的行，第二列是只在file2中有的行，第三列是两个文件中都有的行。从这个角度，就比较容易理解man page里面的说明了。 <br/>
参考[^fn:24] <br/>


#### 场景 {#场景}

比较两个目录中的文件 <br/>

```text
# 不加任何参数时，输出第一列是文件1特有的，第二列是文件2特有，第三列是共有
$ comm <(ls tar_test | sort) <(ls hhh_test | sort)
		a.txt
all.tar
all0
all1
b.txt
		c.txt
	d.txt
# 输出中去掉第一列(文件1特有的)
$ comm -1 <(ls tar_test | sort) <(ls hhh_test | sort)
	a.txt
	c.txt
d.txt
# 输出中去掉第二列(文件2特有的)
$ comm -2 <(ls tar_test | sort) <(ls hhh_test | sort)
	a.txt
all.tar
all0
all1
b.txt
	c.txt
# 输出中去掉第三列(两个文件共有的)
$ comm -3 <(ls tar_test | sort) <(ls hhh_test | sort)
all.tar
all0
all1
b.txt
	d.txt
# 输出中去掉两个文件特有的，只输出共有的。
$ comm -1 -2 <(ls tar_test | sort) <(ls hhh_test | sort)
a.txt
c.txt
# 输出文件1中有，但文件2中没有的。
$ comm -2 -3 <(ls tar_test | sort) <(ls hhh_test | sort)
all.tar
all0
all1
b.txt
# 输出文件2中有，但文件1中没有的
$ comm -1 -3 <(ls tar_test | sort) <(ls hhh_test | sort)
d.txt
```

参考[^fn:25] <br/>


### echo {#echo}


#### 常用选项 {#常用选项}

-   -n, 不换行输出 <br/>
    缺省echo会在输出内容最后追加换行，加了-n之后，就原样输出内容，不会再在最后加换行。 <br/>
-   -e, enable interpretation of backslash escapes <br/>
    输出转义字符，常用的转义字符有\r、\n等，如下[^fn:26]： <br/>
    -   \a：ALERT / BELL (从系统喇叭送出铃声) <br/>
    -   \b：BACKSPACE ，也就是向左删除键 <br/>
    -   \c：取消行末之换行符号 <br/>
    -   \E：ESCAPE，跳脱键 <br/>
    -   \f：FORMFEED，换页字符 <br/>
    -   \n：NEWLINE，换行字符 <br/>
    -   \r：RETURN，回车键 <br/>
    -   \t：TAB，表格跳位键 <br/>
    -   \v：VERTICAL TAB，垂直表格跳位键 <br/>
    -   \n：ASCII 八进位编码(以 x 开首为十六进制) <br/>
    -   \\\\：反斜线本身 <br/>
-   -E, disable interpretation of backslash escapes (default) <br/>
    禁用字符转义(默认就是这样)。 <br/>


#### 场景 {#场景}

-   输出转义字符 <br/>
    ```text
    $ echo -e "line 1\r\nline 2\r\nline 3"
    line 1
    line 2
    line 3
    $
    ```
-   不带换行的输出 <br/>

<!--listend-->

```text
$ echo -n "this is a output line."
this is a output line. $ 
```


### tee {#tee}


#### 常用选项 {#常用选项}

-   -a, Append the output to the files rather than overwriting them. <br/>
    追加到文件最后，不清空原有文件内容。 <br/>


#### 场景 {#场景}

将标准输出和标准错误输出，同时输出到文件，并显示在控制台。 <br/>

```text
$ ls a.txt xxx.txt
ls: xxx.txt: No such file or directory
a.txt
$ ls a.txt xxx.txt 2>&1 | tee -a log.log
ls: xxx.txt: No such file or directory
a.txt
$ cat log.log 
ls: xxx.txt: No such file or directory
a.txt
$ 
```


### grep {#grep}


#### 常用选项 {#常用选项}

-   -a, --text, Treat all files as ASCII text.  Normally grep will simply print “Binary file ... matches” if files contain binary characters.  Use of this option forces grep to output lines matching the specified pattern. <br/>
    强制将所有文件视为ascii文本文件。缺省不带这个选项的话，只会打印出命中二进制文件，不会打印具体内容。设置之后，会打印具体的匹配内容。 <br/>
-   -R, -r, --recursivei, Recursively search subdirectories listed.  (i.e., force grep to behave as rgrep). <br/>
    递归搜索子目录。 <br/>


#### 场景 {#场景}

-   查找命中某个正则表达式的行 <br/>
    ```bash
    $ echo "cat is lovely.
    b line 02
    b line 02
    asd fa
    xx xyy
    as dfa
    b line 03
    c line 03
    cat does not like dog" | grep 'line \(02\|03\)$'
    b line 02
    b line 02
    b line 03
    c line 03
    $ 
    ```
-   查找目录和子目录中包含某个内容的文件 <br/>
    ```text
    $ grep -r 'asdf' .
    Binary file ./all1/a.txt matches
    ./a.txt:asdf
    ./a.txt:asdf sssdeeee
    $ grep -ar 'asdf' .
    ./all1/a.txt:b.txt000644 000765 000024 00000000000 14516224303 012757 0ustar00chengxiastaff000000 000000 c.txt000644 000765 000024 00000000000 14516224307 012764 0ustar00chengxiastaff000000 000000 asdf
    ./a.txt:asdf
    ./a.txt:asdf sssdeeee
    $
    ```
    如果带-a选项，会打印二进制文件命中的具体行。 <br/>


## 典型场景 {#典型场景}


### 获取bash的进程的pid {#获取bash的进程的pid}

```text
$ echo $BASHPID

$ echo $$
731
$ ps
  PID TTY           TIME CMD
  731 ttys000    0:00.25 -bash
```

在macos上BASHPID这个变量取不到，只能用$$。 <br/>


### 获取bash的版本 {#获取bash的版本}

```text
$ echo $BASH_VERSION
3.2.57(1)-release
$ echo $BASH_VERSINFO
3
$
```

参考[^fn:27] <br/>


### 读取内容时忽略第一行 {#读取内容时忽略第一行}

比如这里需要读取ls命令的输出： <br/>

```text
$ ls -lt hhh_test/
total 0
-rw-r--r--  1 chengxia  staff  0 10 27 10:18 c.txt
-rw-r--r--  1 chengxia  staff  0 10 27 10:17 a.txt
$
```

假如需要从中解析出文件名，要忽略第一行，然就会在前面多一个空行： <br/>

```text
$ ls -lt hhh_test/ | awk '{print $9}'

c.txt
a.txt
$ ls -lt hhh_test/ | (read foobar; awk '{print $9}')
c.txt
a.txt
$
```

原理上，其实非常简单。这里通过管道的作用，将ls的输出给到了管道右侧命令的文件描述符0(stdin)，右侧命令通过括号在一个subshell中执行。在同一个子shell中，对于一个文件描述符，如果一个命令已经读取了一行，下一个命令只能从下一行开始读取。这样，就实现了忽略第一行的效果。 <br/>
参考[^fn:28]。 <br/>


### 同时输出多行 {#同时输出多行}

-   使用echo <br/>
    ```bash
    $ echo "a line 1
    > b line 2
    > c line 3
    > d line 4"
    a line 1
    b line 2
    c line 3
    d line 4
    $
    ```
-   使用 `here file` <br/>
    ```bash
    $ cat <<_end_
    > a line 1
    > b line 2
    > c line 3
    > d line 4
    > _end_
    a line 1
    b line 2
    c line 3
    d line 4
    $
    ```
    其中， `_end_` 可以是任何内容，只要上下一样就可以。但是中间不能出现 `_end_` 开头的行，否则提前结束[^fn:29]。 <br/>

[^fn:1]: [shell prompt和Carriage Return的关系](http://bbs.chinaunix.net/forum.php?mod=viewthread&tid=218853&page=2#pid1467910)  <br/>
[^fn:2]: [Linux Shell 13问，单引号和双引号的区别](http://bbs.chinaunix.net/forum.php?mod=viewthread&tid=218853&page=4#pid1511745) <br/>
[^fn:3]: [Linux Shell基础 多个命令中的分号(;)、与(&amp;&amp;) 、 或(||)](https://www.cnblogs.com/lizhouwei/p/9991635.html) <br/>
[^fn:4]: [Illustrated Redirection Tutorial](https://web.archive.org/web/20230315225157/https://wiki.bash-hackers.org/howto/redirection_tutorial)  <br/>
[^fn:5]: [3.6.9 Moving File Descriptors](https://www.gnu.org/software/bash/manual/html_node/Redirections.html) <br/>
[^fn:6]: [shell if 判断，字符正则匹配](https://www.itxm.cn/post/ajbjje1a1.html) <br/>
[^fn:7]: [Regex](https://en.wikipedia.org/wiki/Regular_expression#POSIX)  <br/>
[^fn:8]: [RegEx with \d doesn’t work in if-else statement](https://askubuntu.com/questions/1143710/regex-with-d-doesn-t-work-in-if-else-statement-with#:~:text=d%20and%20w%20don%27t%20work%20in%20POSIX%20regular,%5B%3Adigit%3A%5D%5D%2B-%2B%20%5D%5D%3Bthen%20echo%20%22Pre%22%20else%20echo%20%22Release%22%20fi) <br/>
[^fn:9]: [shell统计循环次数的方法](https://blog.csdn.net/tjcwt2011/article/details/128498972)  <br/>
[^fn:10]: [hostname man page](https://www.man7.org/linux/man-pages/man1/hostname.1.html) <br/>
[^fn:11]: [How do I get the current Unix time in milliseconds in Bash?](https://serverfault.com/questions/151109/how-do-i-get-the-current-unix-time-in-milliseconds-in-bash) <br/>
[^fn:12]: [彻底搞明白find命令mtime含义和用法](https://blog.csdn.net/db_murphy/article/details/107053545)  <br/>
[^fn:13]: [How to display modified date time with 'find' command?](https://stackoverflow.com/questions/20893022/how-to-display-modified-date-time-with-find-command) <br/>
[^fn:14]: [man page of find](https://man7.org/linux/man-pages/man1/find.1.html)  <br/>
[^fn:15]: [find lacks the option -printf, now what?](https://stackoverflow.com/questions/752818/find-lacks-the-option-printf-now-what) <br/>
[^fn:16]: [Executing Multiple Commands in Find -exec](https://www.baeldung.com/linux/find-exec-multiple-commands)  <br/>
[^fn:17]: [Linux tar 命令](https://www.runoob.com/linux/linux-comm-tar.html)  <br/>
[^fn:18]: [GNU tar - update tar file, overwriting the original file in command line](https://askubuntu.com/questions/1384589/gnu-tar-update-tar-file-overwriting-the-original-file-in-command-linewhich-i)  <br/>
[^fn:19]: [linux 命令：echo 详解](https://blog.csdn.net/yspg_217/article/details/122187643)  <br/>
[^fn:20]: [Cutting the column including size](https://stackoverflow.com/questions/16374616/cutting-the-column-including-size) <br/>
[^fn:21]: [Bash read 命令读数据](https://www.junmajinlong.com/shell/script_course/shell_read/) <br/>
[^fn:22]: [Unix文本处理工具之awk](https://blog.csdn.net/xia7139/article/details/49806421) <br/>
[^fn:23]: [How to select lines between two marker patterns which may occur multiple times with awk/sed](https://stackoverflow.com/questions/17988756/how-to-select-lines-between-two-marker-patterns-which-may-occur-multiple-times-w) <br/>
[^fn:24]: [Linux常用命令——comm命令](https://blog.csdn.net/weixin_43251547/article/details/128597850)  <br/>
[^fn:25]: [Find common files between two folders](https://stackoverflow.com/questions/38827243/find-common-files-between-two-folders)  <br/>
[^fn:26]: [別人 echo、你也 echo ，是問 echo 知多少？](http://bbs.chinaunix.net/forum.php?mod=viewthread&tid=218853&page=3#pid1482452)  <br/>
[^fn:27]: [How to get the Bash version number](https://stackoverflow.com/questions/9450604/how-to-get-the-bash-version-number) <br/>
[^fn:28]: [remove first line in bash](https://superuser.com/questions/284258/remove-first-line-in-bash)  <br/>
[^fn:29]: [shell同时输出多行信息](https://blog.51cto.com/u_15127527/3388614)  <br/>