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
- [通配符](#通配符)
- [行分隔符](#行分隔符)
- [多条命令组合](#多条命令组合)
- [将命令的输出赋值给变量](#将命令的输出赋值给变量)
- [重定向和文件描述符](#重定向和文件描述符)
- [算术运算](#算术运算)
    - [使用expr](#使用expr)
    - [使用 `$(( ))`](#使用)
    - [使用 `$[ ]`](#使用)
    - [使用let命令](#使用let命令)
- [shell中的变量](#shell中的变量)
- [字符串操作](#字符串操作)
    - [按照长度截取](#按照长度截取)
    - [按照内容截取](#按照内容截取)
    - [字符串比对](#字符串比对)
- [shell中的占位空语句](#shell中的占位空语句)
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
    - [判断目录是否为空](#判断目录是否为空)
- [nohup](#nohup)
- [sleep](#sleep)
- [循环](#循环)
    - [while](#while)
        - [典型场景](#典型场景)
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
        - [选项](#选项)
            - [`-exec` 选项](#exec-选项)
            - [`-print` 选项](#print-选项)
            - [`-prune` 选项](#prune-选项)
        - [查找修改时间在15天以内的文件](#查找修改时间在15天以内的文件)
        - [按照文件名查找](#按照文件名查找)
        - [打印时间](#打印时间)
        - [查找某个时间区间内的文件](#查找某个时间区间内的文件)
        - [根据文件大小查找文件](#根据文件大小查找文件)
        - [find命令对查找出的文件执行操作](#find命令对查找出的文件执行操作)
            - [执行单个多个操作](#执行单个多个操作)
            - [执行多个多个操作](#执行多个多个操作)
        - [find命令按照字母序查找，查找文件名小于某个字符串的文件](#find命令按照字母序查找-查找文件名小于某个字符串的文件)
        - [find命令删除符合条件的文件](#find命令删除符合条件的文件)
    - [tar](#tar)
        - [常用选项说明](#常用选项说明)
        - [打包](#打包)
            - [整个目录的打包和解包](#整个目录的打包和解包)
                - [打包](#打包)
                - [解包](#解包)
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
        - [选项](#选项)
        - [场景](#场景)
            - [输出行中的指定字段](#输出行中的指定字段)
            - [输出文件中的指定行](#输出文件中的指定行)
                - [行号筛选](#行号筛选)
                - [根据内容筛选](#根据内容筛选)
                - [筛选两个字段相同的行](#筛选两个字段相同的行)
                - [按照字段预处理文本](#按照字段预处理文本)
            - [输出指定行的指定字段](#输出指定行的指定字段)
            - [指定字段分隔符](#指定字段分隔符)
            - [输出文件中的匹配行之前的行](#输出文件中的匹配行之前的行)
        - [使用记录](#使用记录)
            - [换行符导致的奇怪现象](#换行符导致的奇怪现象)
            - [过滤得到指定线程的全部日志](#过滤得到指定线程的全部日志)
            - [过滤指定报错对应线程的全部日志](#过滤指定报错对应线程的全部日志)
            - [分隔字段并去掉其中的部分](#分隔字段并去掉其中的部分)
    - [comm](#comm)
        - [常用选项](#常用选项)
        - [场景](#场景)
    - [echo](#echo)
        - [常用选项](#常用选项)
        - [场景](#场景)
    - [tee](#tee)
        - [常用选项](#常用选项)
        - [场景](#场景)
    - [stat](#stat)
        - [选项](#选项)
        - [场景](#场景)
    - [ps](#ps)
    - [mount](#mount)
    - [Ctrl+Z、fg和jobs命令](#ctrl-plus-z-fg和jobs命令)
    - [lsof](#lsof)
    - [grep](#grep)
        - [常用选项](#常用选项)
        - [场景](#场景)
    - [locale](#locale)
        - [机制说明](#机制说明)
        - [相关命令](#相关命令)
    - [crontab](#crontab)
    - [file](#file)
        - [常用选项](#常用选项)
        - [场景](#场景)
            - [查看文件的编码类型](#查看文件的编码类型)
    - [xxd](#xxd)
    - [test](#test)
        - [说明](#说明)
        - [场景](#场景)
    - [gzip和gunzip](#gzip和gunzip)
    - [curl](#curl)
        - [常用选项](#常用选项)
        - [典型场景](#典型场景)
    - [eval](#eval)
    - [ssh](#ssh)
- [典型场景](#典型场景)
    - [获取bash的进程的pid](#获取bash的进程的pid)
    - [获取bash的版本](#获取bash的版本)
    - [读取内容时忽略第一行](#读取内容时忽略第一行)
    - [同时输出多行](#同时输出多行)
    - [遍历目录中的全部文件](#遍历目录中的全部文件)
    - [递归打印目录结构的脚本](#递归打印目录结构的脚本)

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


## 通配符 {#通配符}

参考[^fn:3] <br/>

-   `?` (question mark)问号 <br/>
    匹配任意一个字符。 <br/>
-   `*` (asterisk)星花 <br/>
    匹配任意零个或者多个字符。 <br/>
-   `[]` (square brackets)方括号 <br/>
    []中可以指定范围，然后匹配其中一个字符。a[m,n]z匹配amz、anz，a[r-t]z匹配arz、asz、atz。a[!m,n]z表示匹配除了amz和anz之外的其他。 <br/>
-   `{}` (curly brackets)大括号 <br/>
    大括号里面可以放多项，支持通配符。{**.foo,**.bar}；{1..5}可以表示1 2 3 4 5。 <br/>
    这个大括号的神奇的地方在于支持展开，示例如下： <br/>
    ```text
    $ ls *.bar *.foo
    a.bar	a.foo
    $ ls {*.bar,*.foo}
    a.bar	a.foo
    $ echo {1..4}
    1 2 3 4
    $ echo {a..f}
    a b c d e f
    $ echo {*.bar,*.foo}
    a.bar a.foo
    $ ls a{.bar,.foo}
    a.bar	a.foo
    $
    ```

到这里，可以看出，通配符不支持 `[0-9]{10}` 十个数字这种类似正则的语法。 <br/>


## 行分隔符 {#行分隔符}

shell语句中，分号和换行都可以作为行分隔符，因此，每条语句的末尾不用额外加分号作为结束。 <br/>


## 多条命令组合 {#多条命令组合}

参考[^fn:4] <br/>

| 多命令执行符 | 格 式                | 作 用                                    |
|--------|--------------------|----------------------------------------|
| ;            | 命令1 ; 命令2        | 多条命令顺序执行，命令之间没有任何逻辑关系 |
| &amp;&amp;   | 命令1 &amp;&amp; 命令2 | 命令1正确执行( `$?=0` )，命令2才执行；否则，命令2不会执行 |
| &vert;&vert; | 命令1 &vert;&vert; 命令2 | 如果命令1执行不正确( `$?≠0` )，则命令2才会执行；否则，命令2不会执行 |


## 将命令的输出赋值给变量 {#将命令的输出赋值给变量}

两种方式，如下： <br/>

```text
$ date_str1=$(date "+%Y-%m-%d")
$ date_str2=`date "+%Y-%m-%d"`
$ echo $date_str1
2024-01-16
$ echo $date_str2
2024-01-16
```


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

参考[^fn:5]。 <br/>
链接[^fn:6]中解释了 `1<&3-` 。 <br/>


## 算术运算 {#算术运算}

参考[^fn:7] <br/>


### 使用expr {#使用expr}

注意，运算符和数字之间必须要有空格，而且乘号需要用斜线转义。如下： <br/>

```text
$ result_multi=`expr 6 \* 3`
$ result_add=`expr 6 + 3`
$ result_minus=`expr 6 - 3`
$ result_multi=`expr 6 \* 3`
$ result_divide=`expr 6 / 3`
$ echo $result_add $result_minus $result_multi $result_divide
9 3 18 2
$ echo `expr 6 % 3`
0
```


### 使用 `$(( ))` {#使用}

如下： <br/>

```text
$ echo $((6+3))
9
$ echo $((6-3))
3
$ echo $((6*3))
18
$ echo $((6/3))
2
$ echo $((6%3))
0
$ echo $((6**3))
216
$ m=6
$ n=3
$ echo `expr m + n`
expr: not a decimal number: 'm'

$ echo `expr $m + $n`
9
$ m=6
$ n=3
$ echo $((m+n))
9
$ echo $((m+1))
7
$ 
```


### 使用 `$[ ]` {#使用}

如下： <br/>

```text
$ echo $[6+3]
9
$ echo $[6-3]
3
$ echo $[6*3]
18
$ echo $[6/3]
2
$ echo $[6%3]
0
$ echo $[6**3]
216
$ m=6
$ n=3
$ echo $[m+n]
9
$ echo $[m+1]
7
$ 
```


### 使用let命令 {#使用let命令}

如下： <br/>

```text
$ let a=6+3
$ echo $a
9
$ let a=6-3
$ echo $a
3
$ let a=6*3
$ echo $a
18
$ let a=6/3
$ echo $a
2
$ let a=6%3
$ echo $a
0
$ m=6
$ n=3
$ let a=m+n
$ echo $a
9
$ let a=m+1
$ echo $a
7
```


## shell中的变量 {#shell中的变量}

参考[^fn:8]。 <br/>
shell中有三种变量： <br/>

-   局部变量(local variable)，只在函数体范围内生效。 <br/>
-   全局变量(global variable)，在当前shell进程中生效。 <br/>
-   环境变量(environment variable)，在当前shell及其子进程中生效。 <br/>

关于这三种变量，需要注意： <br/>

-   未加特别说明，直接直接定义的变量，都是全局变量。 <br/>
    即使这个变量是定义在函数内部，只要没加特别的关键字说明，也是全局变量。 <br/>
-   定义局部变量，需要在函数内部定义，前面添加关键字local <br/>
    没有local关键字，即使在函数体内部，也是全局变量。 <br/>
-   如果要将一个全局变量，处理为环境变量，可以使用export关键字，也可以在定义时，前面添加export关键字，将变量直接定义成环境变量。 <br/>
    `a=5; export a` 和 `export a=5` 效果上最终都是将a设置为了环境变量。环境变量只能从父shell传递到子shell，不能从子shell传递到父shell。 <br/>

关于父shell，如果在一个shell脚本中，通过sh命令启动了另外一个脚本，这样就相当于启动了一个子shell。可以控制脚本在执行时，用当前shell环境，而不启用子shell环境，只需要用 `source ./script.sh` 或者是 `. ./script.sh` 即可。在一个场景中，一个shell脚本拆分了到了好多文件，然后通过一个shell串起来，这样，父shell脚本在最后串子shell脚本的时候，可以通过source命令调用。这样，可以复用当前shell的全局变量，而不用将全局变量都逐一export。 <br/>


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


### 字符串比对 {#字符串比对}

有三种方式： <br/>

-   test命令 <br/>
    ```text
    str1=asdf
    str2=qwer
    if test "$str1" = "$str2"; then
        echo "字符串相等"
    elif test "$str1" \< "$str2"; then
        echo "str1小于str2"
    else
        echo "str1大于str2"
    fi
    ```
-   方括号 <br/>
    ```text
    str1=asdf
    str2=qwer
    if [ "$str1" = "$str2" ]; then
        echo "字符串相等"
    elif [ "$str1" \< "$str2" ]; then
        echo "str1小于str2"
    else
        echo "str1大于str2"
    fi
    ```
    注意这里的大于号和小于号，需要反斜线转义。 <br/>
-   双方括号 <br/>
    ```text
    str1=asdf
    str2=qwer
    if [[ "$str1" = "$str2" ]]; then
        echo "字符串相等"
    elif [[ "$str1" < "$str2" ]]; then
        echo "str1小于str2"
    else
        echo "str1大于str2"
    fi
    ```
    方括号比较强大，也支持正则匹配，见[字符串判断](#字符串判断)。 <br/>


## shell中的占位空语句 {#shell中的占位空语句}

shell中可以通过一个英文冒号来充当占位空语句。如下： <br/>

```text
$ if [ $a -ne $b ]; then
> echo "a not equal b"
> else
> :
> fi
a not equal b
$
```


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

if语句的基本语法[^fn:9]如下： <br/>

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

这里的正则是posix规范的[^fn:10]，而非一般的perl或者javascript[^fn:11]，如下： <br/>
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


### 判断目录是否为空 {#判断目录是否为空}

判断目录是否为空，可以用如下语法片段： <br/>

```bash
dir="/path/to/directory"
if [ -z "$(ls -A $dir)" ]; then
    echo "目录为空"
else
    echo "目录非空"
fi
```

需要注意的是，如果dir变量的值中，有 `~` 符号，就不能在外面加引号，单引号和双引号都不行。原因在于，引号会阻止 `~` 符号的展开。如下： <br/>

```text
$ ls '~/temp/empty_dir/'
ls: ~/temp/empty_dir/: No such file or directory
$ ls "~/temp/empty_dir/"
ls: ~/temp/empty_dir/: No such file or directory
$ ls ~/temp/empty_dir/
a.txt
$
```


## nohup {#nohup}

在后台执行。 <br/>
通过ssh工具连接服务器执行命令时，偶尔的网络抖动会导致客户端和服务器的连接断开，进而导致脚本的执行中断。通过nohup可以解决这个问题，执行后，任务在后台运行，即使断开和客户端的连接，任务的执行也不会受影响[^fn:12]。如下： <br/>

```bash
$ cat test.sh 
echo "Exec Start"
i=1
while [ $i -lt 60 ]
do
	# sleep 2 seconds
	sleep 2
	echo $i
	i=$(($i+1))
done
echo "Exec End"
# 在后台运行脚本，并将标准输出和标准错误重定向到文件nohup.out。执行之后，输出的这个数字是任务进程的pid
$ nohup sh test.sh >> nohup.out 2>&1 &
[1] 47878
# 可以通过脚本找到这个后台执行任务的pid
$ ps -a | grep test.sh
47878 ttys001    0:00.01 sh test.sh
47885 ttys001    0:00.00 grep test.sh
# 如果想把这个后台任务干掉，可以直接kill
$ kill -9 47878
[1]+  Killed: 9               nohup sh test.sh >> nohup.out 2>&1
$ cat nohup.out 
Exec Start
1
2
3
4
5
6
7
8
$
```


## sleep {#sleep}

可以让shell脚本执行暂停一段时间[^fn:13]。格式如下： <br/>

```text
sleep NUMBER[SUFFIX]
```

NUMBER表示需要停顿的时间，SUFFIX表示时间单位，可以有以下几种： <br/>

-   s：秒 <br/>
-   m：分 <br/>
-   h：小时 <br/>
-   d：天 <br/>

脚本停顿2秒： <br/>

```bash
sleep 2s
```

脚本停顿5分钟： <br/>

```bash
sleep 5m
```

macos上不是前面这种格式，不支持指定单位，只支持暂停默认的秒数。如下，停顿20秒： <br/>

```bash
sleep 20
```


## 循环 {#循环}


### while {#while}

参考[^fn:14] <br/>

```bash
i=1
while [ $i -lt 10 ]
do
echo $i;
i=$(($i+1));
done
```


#### 典型场景 {#典型场景}

-   循环读取命令的输出，逐行处理。 <br/>
    可以使用如下命令： <br/>
    ```bash
    ls *.log | while read -r filename;
    do
        echo $filename
    done
    ```
    效果如下： <br/>
    ```text
    ls *.log
    sample.log	sample2.log
    $ ls *.log | while read -r filename;
    > do
    >     echo $filename
    > done
    sample.log
    sample2.log
    $ 
    ```


### for {#for}

参考[^fn:14] <br/>

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

参考：[^fn:15] <br/>

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
-   -A, Include directory entries whose names begin with a dot (‘.’) except for . and ...  Automatically set for the super-user unless -I is specified. <br/>
    显示.开头的文件，但是不显示.和..这两个目录。 <br/>


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

获取当前时间(年份只要后两位，用 `%y` )： <br/>

```text
$ date "+%Y.%m.%d"
2024.01.16
$ date "+%y.%m.%d"
24.01.16
$
```

也可以通过%N来获取到纳秒(毫秒的千分之一)，但是macOS上不支持[^fn:16]。 <br/>
获取一天以前的时间。 <br/>
Ubuntu下： <br/>

```text
$ date "+%Y%m%d_%H%M%S"
20231130_235814
$ date -d "1 day ago" "+%Y%m%d_%H%M%S"
20231129_235818
$
```

macOS下： <br/>

```text
$ date  "+%Y%m%d_%H%M%S"
20231201_000049
$ date -v -1d "+%Y%m%d_%H%M%S"
20231130_000055
$ date -v +1d "+%Y%m%d_%H%M%S"
20231202_000058
$
```


### find {#find}


#### 选项 {#选项}


##### `-exec` 选项 {#exec-选项}

macos终端man page描述： <br/>

```text
-exec utility [argument ...] ;
    True if the program named utility returns a zero value as its exit status.  Optional arguments may be passed to the utility.  The expression must be terminated by a semicolon (“;”).  If you invoke find from a shell you may need to quote the semicolon if the shell would otherwise treat it as a control operator.  If the string “{}” appears anywhere in the utility name or the arguments it is replaced by the pathname of the current file.  Utility will be executed from the directory from which find was executed.  Utility and arguments are not subject to the further expansion of shell patterns and constructs.

-exec utility [argument ...] {} +
    Same as -exec, except that “{}” is replaced with as many pathnames as possible for each invocation of utility.  This behaviour is similar to that of xargs(1).  The primary always returns true; if at least one invocation of utility returns a non-zero exit status, find will return a non-zero exit status.
```

exec选项后面的命令执行成功后才会返回true。 <br/>


##### `-print` 选项 {#print-选项}

macos终端man page描述： <br/>

```text
-print  This primary always evaluates to true.  It prints the pathname of the current file to standard output.  If none of -exec, -ls, -print, -print0, or -ok is specified, the given expression shall be effectively replaced by ( given expression ) -print.
```

也就是说，这个 `-print` 选项是默认的缺省选项，当没有指定exec、ls、print、print0、ok等任何一个时，就会用 `( given expression ) -print.` 替换。 <br/>


##### `-prune` 选项 {#prune-选项}

macos终端man page描述： <br/>

```text
-prune  This primary always evaluates to true.  It causes find to not descend into the current file.  Note, the -prune primary has no effect if the -d option was specified.
```

参考材料如下： <br/>

-   `-prune` 是一个动作(action)，而不是一个条件判断(test)。[^fn:17] <br/>
-   说明了在find执行时，如何屏蔽某个指定目录。[^fn:18] <br/>
-   中文博客里面讲得清楚又简单的材料[^fn:19] <br/>
-   介绍了 `-print` 选项只对前面的表达式有效。[^fn:20] <br/>
    这里说的action只对前面的筛选表达式有效，是指前面or分隔的那些表达式段，并非只指紧邻 `-print` 的那一个筛选条件。如果要改变这个action的作用范围，需要在前面条件筛选表达式中加括号控制。 <br/>

这个选项的含义，就是将前面匹配条件命中的结果，在最终的结果集中删除。注意这个prune一直都是返回true(exec选项也类似，不过是后面的命令执行成功后才会返回true)。 <br/>
一般来说，需要按照如下格式使用prune参数：find后面先跟查找的目录，然后指定多个排除的目录，如果有多个需要加括号。最后，通过-o选项连接，后面跟文件的其他筛选条件，再跟要执行的动作。 <br/>

```text
$ tree .
.
├── a.txt
├── b.txt
├── exclude
│   ├── m.txt
│   └── n.txt
└── inner
    ├── x.txt
    └── y.txt

2 directories, 6 files

# 最一般的排除目录的写法
$ find . -path ./exclude -prune -o -type f  -exec echo {} \;
./inner/x.txt
./inner/y.txt
./b.txt
./a.txt

# 通过指定-print选项，达到和前面一样的效果
$ find . -path ./exclude -prune -o -type f -print
./inner/x.txt
./inner/y.txt
./b.txt
./a.txt

# 如果不指定任何action，默认相当于find . \( -path ./exclude -prune -o -type f \) -print
$ find . -path ./exclude -prune -o -type f 
./inner/x.txt
./inner/y.txt
./exclude
./b.txt
./a.txt
$ find . \( -path ./exclude -prune -o -type f \) -print
./inner/x.txt
./inner/y.txt
./exclude
./b.txt
./a.txt

# 同时排除两个目录
$ find . \( -path ./exclude -o -path ./inner \) -prune -o -type f  -print
./b.txt
./a.txt
$ find . -type d \( -path ./exclude -o -path ./inner \) -prune -o -type f  -print
./b.txt
./a.txt

# -o后面跟多个筛选条件
$ find . -type d \( -path ./exclude -o -path ./inner \) -prune -o -type f -name "a.*" -print
./a.txt
$ find . -type d \( -path ./exclude -o -path ./inner \) -prune -o -type f ! -name "a.*" -print
./b.txt
$ 
```

这么看，find后面这些内容，真的是表达式，表达式默认用 `-a` 连接。向 `-prune` 和 `-print` 这样的action，也可以算是表达式中的运算符。这些所有的运算中 `-o` 的优先级别最低，前面为真时，也会发生截断，后面的表达式不执行。在使用中，就是通过这种包含 `-a` 、 `-o` 、括号、action等这些内容的表达式来实现我们的查找需求。 <br/>
明白了这个道理后，不难理解下面的命令： <br/>

```text
$ tree .
.
├── a.txt
├── b.txt
├── exclude
│   ├── m.txt
│   └── n.txt
└── inner
    ├── x.txt
    └── y.txt

2 directories, 6 files
$ find . -type d \( -path ./exclude -o -path ./inner \) -prune -o -type f -print \( -name "a.txt" -exec echo "this is {}." \; -o -name "b.txt" -exec echo "that is {}." \; \)
./b.txt
that is ./b.txt.
./a.txt
this is ./a.txt.
$
```


#### 查找修改时间在15天以内的文件 {#查找修改时间在15天以内的文件}

```text
find . -maxdepth 1 -type f -mtime -15
```

mtime说明[^fn:21]： <br/>
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

参考[^fn:22] <br/>

```bash
find . -maxdepth 1 -type f -printf "%p %TY-%Tm-%Td %TH:%TM:%TS %Tz\n"
```

这里面%p表示文件名，%T表示是修改时间，可以换成%C表示拿到状态改变的时间，换成%A拿到上次访问的时间[^fn:23]。 <br/>
不过，macos中的find命令，并不支持-printf选项[^fn:24]。 <br/>


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


#### 根据文件大小查找文件 {#根据文件大小查找文件}

find命令的 `-size` 选项用于指定文件大小作为查找条件[^fn:25]，支持的单位有： <br/>

-   b - 512字节的块（如果没有使用后缀，这是默认的）。 <br/>
-   c - 字节 <br/>
-   w - 两个字节的字 <br/>
-   k - 千字节 <br/>
-   M - 百万字节 <br/>
-   G - 千兆字节 <br/>

在指定的大小前添加加号，表示查找大于指定大小的文件；添加减号，表示查找小于指定大小的文件；减号和加号都没有，表示查找登于指定大小的文件。 <br/>

-   搜索当前工作目录中文件大小为6MB的所有文件： `find . -size 6M` 。 <br/>
-   搜索所有大于2千兆字节的文件： `find . -size +2G` 。 <br/>
-   搜索所有大小 "小于" 10KB的文件： `find . -size -10k` 。 <br/>
-   使用find命令来搜索大于10MB但小于20MB的文件： `find . -size +10M -size -20M` 。 <br/>
-   使用find命令来搜索空文件： `find . -type f -size 0b` 或 `find . -type f -empty` 。 <br/>


#### find命令对查找出的文件执行操作 {#find命令对查找出的文件执行操作}


##### 执行单个多个操作 {#执行单个多个操作}

这里 `{}` 代表find查找出的一条结果文件名，如下： <br/>

```text
$ ls *.txt
Math.txt	Mathv.txt	a.txt
$ find . -maxdepth 1 -type f -name "*.txt" -exec echo "filename:"{} \;
filename:./Mathv.txt
filename:./a.txt
filename:./Math.txt
$
```


##### 执行多个多个操作 {#执行多个多个操作}

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

参考[^fn:26] <br/>


#### find命令按照字母序查找，查找文件名小于某个字符串的文件 {#find命令按照字母序查找-查找文件名小于某个字符串的文件}

```text
$ find . -name "*.txt" -exec echo {} \;
./inner/x.txt
./inner/y.txt
./exclude/m.txt
./exclude/n.txt
./b.txt
./a.txt
$ find . -name "*.txt" -exec test "{}" \< "./e.txt" \; -print
./b.txt
./a.txt
$ find . -name "*.txt" -exec test "{}" \> "./e.txt" \; -print
./inner/x.txt
./inner/y.txt
./exclude/m.txt
./exclude/n.txt
$ 
```


#### find命令删除符合条件的文件 {#find命令删除符合条件的文件}

通过fing命令后跟 `-exec rm -rf {} \;` 或 find命令后跟 `-delete` 参数，都可以删除符合条件的文件。但是，后者由于是find内部的删除调用，效率上要快很多[^fn:27]。如下。 <br/>
首先准备如下造文件脚本： <br/>

```bash
$ cat test.sh 
echo "Exec Start"
i=1
while [ $i -lt 60 ]
do
	touch "a$i.txt"
	i=$(($i+1))
done
echo "Exec End"
$
```

执行用时比较： <br/>

```bash
$ ls
test.sh
$ sh test.sh 
Exec Start
Exec End
$ ls
a1.txt	a12.txt	a15.txt	a18.txt	a20.txt	a23.txt	a26.txt	a29.txt	a31.txt	a34.txt	a37.txt	a4.txt	a42.txt	a45.txt	a48.txt	a50.txt	a53.txt	a56.txt	a59.txt	a8.txt
a10.txt	a13.txt	a16.txt	a19.txt	a21.txt	a24.txt	a27.txt	a3.txt	a32.txt	a35.txt	a38.txt	a40.txt	a43.txt	a46.txt	a49.txt	a51.txt	a54.txt	a57.txt	a6.txt	a9.txt
a11.txt	a14.txt	a17.txt	a2.txt	a22.txt	a25.txt	a28.txt	a30.txt	a33.txt	a36.txt	a39.txt	a41.txt	a44.txt	a47.txt	a5.txt	a52.txt	a55.txt	a58.txt	a7.txt	test.sh
$ time find . -type f -name "a*.txt" -exec rm -rf {} \;

real	0m0.213s
user	0m0.052s
sys	0m0.128s
$ ls
test.sh
$ sh test.sh 
Exec Start
Exec End
$ ls
a1.txt	a12.txt	a15.txt	a18.txt	a20.txt	a23.txt	a26.txt	a29.txt	a31.txt	a34.txt	a37.txt	a4.txt	a42.txt	a45.txt	a48.txt	a50.txt	a53.txt	a56.txt	a59.txt	a8.txt
a10.txt	a13.txt	a16.txt	a19.txt	a21.txt	a24.txt	a27.txt	a3.txt	a32.txt	a35.txt	a38.txt	a40.txt	a43.txt	a46.txt	a49.txt	a51.txt	a54.txt	a57.txt	a6.txt	a9.txt
a11.txt	a14.txt	a17.txt	a2.txt	a22.txt	a25.txt	a28.txt	a30.txt	a33.txt	a36.txt	a39.txt	a41.txt	a44.txt	a47.txt	a5.txt	a52.txt	a55.txt	a58.txt	a7.txt	test.sh
$ time find . -type f -name "a*.txt" -delete

real	0m0.007s
user	0m0.001s
sys	0m0.006s
$ ls
test.sh
$
```


### tar {#tar}

tar命令的使用 <br/>
tar(英文全拼：tape archive)命令用于备份文件。tar是用来建立，还原备份文件的工具程序，它可以加入，解开备份文件内的文件。[^fn:28] <br/>


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


##### 整个目录的打包和解包 {#整个目录的打包和解包}


###### 打包 {#打包}

```text
# 将目录file_dir中的内容打包到file_dir.tar
tar -cvf file_dir.tar file_dir
# 将目录file_dir中的内容打包并压缩到file_dir.tar
tar -czvf file_dir.tar file_dir
```


###### 解包 {#解包}

这种带目录的解包，只需在要解压到的目的目录下，执行一般的解压操作，就会将tar中的目录解压到当前目录下。 <br/>

```text
# 解压到当前目录，覆盖已有的文件
tar -xvf file_dir.tar
# 解压到当前目录，不覆盖已有的文件
tar -xkvf file_dir.tar
```

如果执行解压命令不是在解压到的目的目录下，可以通过 `-C` 参数指定解压路径： <br/>

```text
# 将 /path/to/tar/file_dir.tar 压缩包中的内容，解压到目录 /path/extract/dest/
tar -xvf /path/to/tar/file_dir.tar -C /path/extract/dest/
```


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

注意，这里tar不支持在打包的时候，覆盖包中已有的同名文件。原因可能是因为，这个命令诞生于磁带打包场景，磁带这种存储介质，只适合追加写，更新前面的内容(随记写)效率不佳。参考[^fn:29] <br/>


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

这里echo命令的-e选项是为了启用反斜杠转义[^fn:30]。 <br/>

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

不过，处理ls命令的输出，还是结合awk命令使用比较好[^fn:31]。如下： <br/>

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

参考[^fn:32]： <br/>

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


#### 选项 {#选项}

常用格式如下： <br/>

```text
awk [ -F fs ] [ -v var=value ] [ 'prog' | -f progfile ] [ file ...  ]
```

选项 `-F fs` ，指定行分隔符。可以是一个正则表达式。 <br/>


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

参考[^fn:33] <br/>

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
-   打印包含从命中正则1到命中正则2之间的行[^fn:34] <br/>
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


###### 筛选两个字段相同的行 {#筛选两个字段相同的行}

语法支持直接写if条件，根据不同字段比对的结果确定是否输出。如下： <br/>

```text
$ echo "a line 1
b line 2
c line c
d line d" | awk '{if($1==$3)print $0,$1,$2,$3}'
c line c c line c
d line d d line d

$ echo "a line 1
b line 2
c line c
d line d" | awk '{if($1!=$3)print $0,$1,$2,$3}'
a line 1 a line 1
b line 2 b line 2

$ echo "a line 1
b line 2
c line c
d line d" | awk '{if($1==$3&&$1=="d")print}'
d line d

$ echo "a line 1
b line 2
c line c
d line d" | awk '{if($1==$3||$1=="a")print $0,$1,$2,$3}'
a line 1 a line 1
c line c c line c
d line d d line d
$
```


###### 按照字段预处理文本 {#按照字段预处理文本}

如下： <br/>

```text
$ echo "20231111 apple_2*3
> 20231112 pear_5*3
> 20231112 apple_2*3
> 20231113 pear_1*3
> 20231113 pear_2*3
> 20231112 pear_5*3
> 20231113 banana_3*3" | awk '{if($2 ~/pear/){print $1,"apple"}else if($2 ~/pear/){print $1,"pear"}else{print $1,"other"}}'
20231111 other
20231112 apple
20231112 other
20231113 apple
20231113 apple
20231112 apple
20231113 other
$ 
```

这样，根据第二个字段是否匹配正则，将文本行归到了几类。这样预处理之后，可以进行sort、count的进一步统计。如下： <br/>

```text
$ echo "20231111 apple_2*3
20231112 pear_5*3
20231112 apple_2*3
20231113 pear_1*3
20231113 pear_2*3
20231112 pear_5*3
20231113 banana_3*3" | awk '{if($2 ~/pear/){print $1,"apple"}else if($2 ~/pear/){print $1,"pear"}else{print $1,"other"}}' | sort | uniq -c
   1 20231111 other
   2 20231112 apple
   1 20231112 other
   2 20231113 apple
   1 20231113 other
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


##### 指定字段分隔符 {#指定字段分隔符}

通过-F选项可以指定字段分隔符，字段分隔符可以是一个正则表达式。应该是POSIX规范的。如下： <br/>

```text
$ echo "2023-11-10 11:03 a 1
2024-12-12 12:06 b 2
1990-08-09 02:09 c 3
2020-09-09 03:06 d 4" | awk -F "-" '{print "f0: "$0"f1: "$1"f2: "$2"f3: "$3"f4: "$4}'
f0: 2023-11-10 11:03 a 1f1: 2023f2: 11f3: 10 11:03 a 1f4: 
f0: 2024-12-12 12:06 b 2f1: 2024f2: 12f3: 12 12:06 b 2f4: 
f0: 1990-08-09 02:09 c 3f1: 1990f2: 08f3: 09 02:09 c 3f4: 
f0: 2020-09-09 03:06 d 4f1: 2020f2: 09f3: 09 03:06 d 4f4: 

$ echo "2023-11-10 11:03 a 1
2024-12-12 12:06 b 2
1990-08-09 02:09 c 3
2020-09-09 03:06 d 4" | awk -F ":" '{print "f0: "$0"f1: "$1"f2: "$2"f3: "$3"f4: "$4}'
f0: 2023-11-10 11:03 a 1f1: 2023-11-10 11f2: 03 a 1f3: f4: 
f0: 2024-12-12 12:06 b 2f1: 2024-12-12 12f2: 06 b 2f3: f4: 
f0: 1990-08-09 02:09 c 3f1: 1990-08-09 02f2: 09 c 3f3: f4: 
f0: 2020-09-09 03:06 d 4f1: 2020-09-09 03f2: 06 d 4f3: f4: 

# 指定正则表达式作为分隔符，或
$ echo "2023-11-10 11:03 a 1
2024-12-12 12:06 b 2
1990-08-09 02:09 c 3
2020-09-09 03:06 d 4" | awk -F ":|-" '{print "f0: "$0"f1: "$1"f2: "$2"f3: "$3"f4: "$4}'
f0: 2023-11-10 11:03 a 1f1: 2023f2: 11f3: 10 11f4: 03 a 1
f0: 2024-12-12 12:06 b 2f1: 2024f2: 12f3: 12 12f4: 06 b 2
f0: 1990-08-09 02:09 c 3f1: 1990f2: 08f3: 09 02f4: 09 c 3
f0: 2020-09-09 03:06 d 4f1: 2020f2: 09f3: 09 03f4: 06 d 4

# 指定正则表达式作为分隔符，字符范围
$ echo "2023-11-10 11:03 a 1
2024-12-12 12:06 b 2
1990-08-09 02:09 c 3
2020-09-09 03:06 d 4" | awk -F "[:-]" '{print "f0: "$0"f1: "$1"f2: "$2"f3: "$3"f4: "$4}'
f0: 2023-11-10 11:03 a 1f1: 2023f2: 11f3: 10 11f4: 03 a 1
f0: 2024-12-12 12:06 b 2f1: 2024f2: 12f3: 12 12f4: 06 b 2
f0: 1990-08-09 02:09 c 3f1: 1990f2: 08f3: 09 02f4: 09 c 3
f0: 2020-09-09 03:06 d 4f1: 2020f2: 09f3: 09 03f4: 06 d 4

# 指定正则表达式作为分隔符，一个或者多个数字作为分隔符，这样，行开始就是一个分隔符，那么s1为空
$ echo "2023-11-10 11:03 a 1
2024-12-12 12:06 b 2
1990-08-09 02:09 c 3
2020-09-09 03:06 d 4" | awk -F "[0-9]+" '{print "f0: "$0"f1: "$1"f2: "$2"f3: "$3"f4: "$4"f5: "$5"f6: "$6"f7: "$7}'
f0: 2023-11-10 11:03 a 1f1: f2: -f3: -f4:  f5: :f6:  a f7: 
f0: 2024-12-12 12:06 b 2f1: f2: -f3: -f4:  f5: :f6:  b f7: 
f0: 1990-08-09 02:09 c 3f1: f2: -f3: -f4:  f5: :f6:  c f7: 
f0: 2020-09-09 03:06 d 4f1: f2: -f3: -f4:  f5: :f6:  d f7: 

# 指定正则表达式作为分隔符，一个或者多个数字作为分隔符，这里的正则是POSIX规范
$ echo "2023-11-10 11:03 a 1
2024-12-12 12:06 b 2
1990-08-09 02:09 c 3
2020-09-09 03:06 d 4" | awk -F "[[:digit:]]+" '{print "f0: "$0"f1: "$1"f2: "$2"f3: "$3"f4: "$4"f5: "$5"f6: "$6"f7: "$7}'
f0: 2023-11-10 11:03 a 1f1: f2: -f3: -f4:  f5: :f6:  a f7: 
f0: 2024-12-12 12:06 b 2f1: f2: -f3: -f4:  f5: :f6:  b f7: 
f0: 1990-08-09 02:09 c 3f1: f2: -f3: -f4:  f5: :f6:  c f7: 
f0: 2020-09-09 03:06 d 4f1: f2: -f3: -f4:  f5: :f6:  d f7: 
$
```


##### 输出文件中的匹配行之前的行 {#输出文件中的匹配行之前的行}

参考[^fn:35] <br/>
思路就是将每一行的内容都给到一个变量，匹配行命中时，检测下前面一行是否是不匹配，如果是就打印前面一行和当前匹配行。如下： <br/>

```text
$ echo "time 12:00
launch
time 08:00
breakfast
time 20:00
dinner" | awk '/breakfast/{if(line && line !~ /breakfast/) print line; print}{line=$0}'
-bash: !~: event not found
> 

```

这个报错和shell的设置有关系，需要修改设置之后，可以正常执行[^fn:36]。解决方式就是在命令之前执行下 `set +H` 即可，如下： <br/>

```text
$ set +H
$ echo "time 12:00
> launch
> time 08:00
> breakfast
> time 20:00
> dinner" | awk '/breakfast/{if(line && line !~ /breakfast/) print line; print}{line=$0}'
time 08:00
breakfast
$ set -H
$ 
```

如果不想修改这个设置，也可以用如下的方式： <br/>

```text
$ echo "time 12:00
> launch
> time 08:00
> breakfast
> time 20:00
> dinner" | awk '/breakfast/{if(line && ! (line ~ /breakfast/)) print line; print}{line=$0}'
time 08:00
breakfast
$
```

相当于用了一个逻辑非操作，在叹号后面加了一个空格就避免了这个问题。 <br/>


#### 使用记录 {#使用记录}


##### 换行符导致的奇怪现象 {#换行符导致的奇怪现象}

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


##### 过滤得到指定线程的全部日志 {#过滤得到指定线程的全部日志}

日志文件样例如下： <br/>

```text
$ cat sample2.log 
2023-12-11 15:09:33,987 thread323 INFO com.test.Test Time is 15:09
2023-12-11 15:09:33,988 thread323 INFO com.test.Test:
thread is going on...
haha
thread is going on...
2023-12-11 15:09:35,97 thread322 DEBUG com.test.Test Time is 15:09
2023-12-11 15:09:36,97 thread322 DEBUG com.test.Test Time is 15:09
2023-12-11 15:10:33,987 thread323 INFO com.test.Test Time is 15:10
2023-12-11 15:10:35,97 thread322 INFO com.test.Test Time is 15:10
asdf
this is 322
2023-12-11 15:12:35,97 thread325 INFO com.test.Test Time is 15:12
2023-12-11 15:15:35,97 thread323 INFO com.test.Test :
error occur in the class
com.test.Test : line 3
Nullpointer Exception
-end-

2023-12-11 16:09:33,987 thread325 INFO com.test.Test Time is 16:09
2023-12-11 16:09:35,97 thread322 DEBUG com.test.Test Time is 16:09
2023-12-11 16:10:33,987 thread323 INFO com.test.Test Time is 16:10
2023-12-11 16:10:35,97 thread322 INFO com.test.Test Time is 16:10
2023-12-11 16:12:35,97 thread325 INFO com.test.Test Time is 16:12
2023-12-11 16:15:35,97 thread325 INFO com.test.Test :
vital exception occur in the class
com.test.Test : line 5 
Nullpointer Exception
-end-

2023-12-11 17:09:33,987 thread323 INFO com.test.Test Time is 17:09
2023-12-11 17:09:35,97 thread322 DEBUG com.test.Test Time is 17:09
qwqeououeo
asdfalj
2023-12-11 17:09:36,97 thread322 DEBUG com.test.Test Time is 17:09
2023-12-11 17:10:33,987 thread323 INFO com.test.Test Time is 17:10
2023-12-11 17:10:33,878 thread323 INFO com.test.Test:
thread is going on...
haha
thread is going on...
2023-12-11 17:10:35,97 thread322 INFO com.test.Test Time is 17:10
2023-12-11 17:12:35,97 thread325 INFO com.test.Test Time is 17:12
2023-12-11 17:15:35,97 thread323 INFO com.test.Test :
error occur in the class
com.test.Test : line 7 
Nullpointer Exception
-end-

2023-12-11 18:09:33,987 thread325 INFO com.test.Test Time is 18:09
2023-12-11 18:09:35,97 thread322 DEBUG com.test.Test Time is 18:09
2023-12-11 18:10:33,987 thread323 INFO com.test.Test Time is 18:10
2023-12-11 18:10:35,97 thread322 INFO com.test.Test Time is 18:10
2023-12-11 18:12:35,97 thread325 INFO com.test.Test Time is 18:12
2023-12-11 18:15:35,97 thread325 INFO com.test.Test :
vital exception occur in the class
com.test.Test : line 9 
Nullpointer Exception
-end-

$ 
```

如果想得到某个线程的全部日志，可以采用如下脚本，filter_log_threadid.awk： <br/>

```text
BEGIN{
	# 标识当前是否是目标日志的一部分
	found_flag = 0
	# 从命令行参数中拿到线程标识
	threadid_pattern = ARGV[1];
	# 拿到之后，将其置空，避免被当成文件名
	ARGV[1] = "";
}
{
	# 如果当前行不是一条独立的log entry，就判断它是否输入目标threadid的输出
	if($0 !~ / thread[0-9]+ /)
	{
		if(found_flag == 1){
			print $0
		}
	}else{
		found_flag = 0
	}
	# 报错标识，找到报错标识匹配的行
	if($0 ~ threadid_pattern)
	{
		print $0
		found_flag = 1
	}
}
```

运行效果如下： <br/>

```text
$ awk -f filter_log_threadid.awk thread322 sample2.log 
2023-12-11 15:09:35,97 thread322 DEBUG com.test.Test Time is 15:09
2023-12-11 15:09:36,97 thread322 DEBUG com.test.Test Time is 15:09
2023-12-11 15:10:35,97 thread322 INFO com.test.Test Time is 15:10
asdf
this is 322
2023-12-11 16:09:35,97 thread322 DEBUG com.test.Test Time is 16:09
2023-12-11 16:10:35,97 thread322 INFO com.test.Test Time is 16:10
2023-12-11 17:09:35,97 thread322 DEBUG com.test.Test Time is 17:09
qwqeououeo
asdfalj
2023-12-11 17:09:36,97 thread322 DEBUG com.test.Test Time is 17:09
2023-12-11 17:10:35,97 thread322 INFO com.test.Test Time is 17:10
2023-12-11 18:09:35,97 thread322 DEBUG com.test.Test Time is 18:09
2023-12-11 18:10:35,97 thread322 INFO com.test.Test Time is 18:10
$ awk -f filter_log_threadid.awk thread325 sample2.log 
2023-12-11 15:12:35,97 thread325 INFO com.test.Test Time is 15:12
2023-12-11 16:09:33,987 thread325 INFO com.test.Test Time is 16:09
2023-12-11 16:12:35,97 thread325 INFO com.test.Test Time is 16:12
2023-12-11 16:15:35,97 thread325 INFO com.test.Test :
vital exception occur in the class
com.test.Test : line 5 
Nullpointer Exception
-end-

2023-12-11 17:12:35,97 thread325 INFO com.test.Test Time is 17:12
2023-12-11 18:09:33,987 thread325 INFO com.test.Test Time is 18:09
2023-12-11 18:12:35,97 thread325 INFO com.test.Test Time is 18:12
2023-12-11 18:15:35,97 thread325 INFO com.test.Test :
vital exception occur in the class
com.test.Test : line 9 
Nullpointer Exception
-end-

$
```

从这个例子中，可以看到awk脚本的语法，还是很有特色的，可以通过简单的逻辑控制，过滤出想要的内容，非常方便。 <br/>


##### 过滤指定报错对应线程的全部日志 {#过滤指定报错对应线程的全部日志}

假设有如下日志文件： <br/>

```text
$ cat sample.log 
2023-12-11 15:09:33,987 thread323 INFO com.test.Test Time is 15:09
2023-12-11 15:09:33,988 thread323 INFO com.test.Test:
thread is going on...
haha
thread is going on...
2023-12-11 15:09:35,97 thread322 DEBUG com.test.Test Time is 15:09
2023-12-11 15:10:33,987 thread323 INFO com.test.Test Time is 15:10
2023-12-11 15:10:35,97 thread322 INFO com.test.Test Time is 15:10
2023-12-11 15:12:35,97 thread325 INFO com.test.Test Time is 15:12
2023-12-11 15:15:35,97 thread323 INFO com.test.Test :
error occur in the class
com.test.Test : line 3
Nullpointer Exception
-end-

2023-12-11 16:09:33,987 thread325 INFO com.test.Test Time is 16:09
2023-12-11 16:09:35,97 thread322 DEBUG com.test.Test Time is 16:09
2023-12-11 16:10:33,987 thread323 INFO com.test.Test Time is 16:10
2023-12-11 16:10:35,97 thread322 INFO com.test.Test Time is 16:10
2023-12-11 16:12:35,97 thread325 INFO com.test.Test Time is 16:12
2023-12-11 16:15:35,97 thread325 INFO com.test.Test :
vital exception occur in the class
com.test.Test : line 5 
Nullpointer Exception
-end-

2023-12-11 17:09:33,987 thread323 INFO com.test.Test Time is 17:09
2023-12-11 17:09:35,97 thread322 DEBUG com.test.Test Time is 17:09
2023-12-11 17:10:33,987 thread323 INFO com.test.Test Time is 17:10
2023-12-11 17:10:33,878 thread323 INFO com.test.Test:
thread is going on...
haha
thread is going on...
2023-12-11 17:10:35,97 thread322 INFO com.test.Test Time is 17:10
2023-12-11 17:12:35,97 thread325 INFO com.test.Test Time is 17:12
2023-12-11 17:15:35,97 thread323 INFO com.test.Test :
error occur in the class
com.test.Test : line 7 
Nullpointer Exception
-end-

2023-12-11 18:09:33,987 thread325 INFO com.test.Test Time is 18:09
2023-12-11 18:09:35,97 thread322 DEBUG com.test.Test Time is 18:09
2023-12-11 18:10:33,987 thread323 INFO com.test.Test Time is 18:10
2023-12-11 18:10:35,97 thread322 INFO com.test.Test Time is 18:10
2023-12-11 18:12:35,97 thread325 INFO com.test.Test Time is 18:12
2023-12-11 18:15:35,97 thread325 INFO com.test.Test :
vital exception occur in the class
com.test.Test : line 9 
Nullpointer Exception
-end-

$ 
```

其中，我们想要过滤出 `error occur in the class` 这个报错对应线程的全部日志。可以通过如下awk脚本实现，filter_log.awk： <br/>

```text
BEGIN{
	i = 1
	# 从命令行参数中拿到要识别的报错模式
	error_pattern = ARGV[1];
	# 拿到之后，将其置空，避免被当成文件名
	ARGV[1] = "";
}
{
	lines[i]=$0
	# 报错标识，找到报错标识匹配的行
	if($0 ~ error_pattern)
	{
		line_before = lines[i-1]
		# 拿到报错标识前一行的线程号作为行筛选标识
		match(line_before,/ thread[0-9]+ /);
		line_mark = substr(line_before, RSTART, RLENGTH)
		# print "line_mark: " line_mark
		# 打印在当前报错行之前，所有该线程号的日志，包括多行的情况
		for(j=1; j<i;j++)
			if(match(lines[j],line_mark) !=0) {
				# 这个标识用于控制打印报错段的跨行内容
				found_flag = 1
				# 打印该行输出，包括跨行的内容
				print lines[j]
				while(lines[j+1] !~ / thread[0-9]+ / && j+1 <i){
					print lines[j+1]
					j=j+1
				}
			}
		# 打印当前报错行
		print $0
		i=0
	}
	# 打印当前报错行之后的连续行(报错行到报错结束标识，这里的报错行标识是一个空行)
	if($0 ~ /^$/ && found_flag == 1)
	{
		for(j=1;j<=i;j++)
			print lines[j]
		found_flag=0
	}
	i=i+1
}
```

运行效果如下： <br/>

```text
$ awk -f filter_log.awk 'error occur in the class' sample.log 
2023-12-11 15:09:33,987 thread323 INFO com.test.Test Time is 15:09
2023-12-11 15:09:33,988 thread323 INFO com.test.Test:
thread is going on...
haha
thread is going on...
2023-12-11 15:10:33,987 thread323 INFO com.test.Test Time is 15:10
2023-12-11 15:15:35,97 thread323 INFO com.test.Test :
error occur in the class
com.test.Test : line 3
Nullpointer Exception
-end-

2023-12-11 16:10:33,987 thread323 INFO com.test.Test Time is 16:10
2023-12-11 17:09:33,987 thread323 INFO com.test.Test Time is 17:09
2023-12-11 17:10:33,987 thread323 INFO com.test.Test Time is 17:10
2023-12-11 17:10:33,878 thread323 INFO com.test.Test:
thread is going on...
haha
thread is going on...
2023-12-11 17:15:35,97 thread323 INFO com.test.Test :
error occur in the class
com.test.Test : line 7 
Nullpointer Exception
-end-

$ 
```

换一个报错标识，也可以正常运行： <br/>

```text
$ awk -f filter_log.awk 'vital exception occur in the class' sample.log 
2023-12-11 15:12:35,97 thread325 INFO com.test.Test Time is 15:12
2023-12-11 16:09:33,987 thread325 INFO com.test.Test Time is 16:09
2023-12-11 16:12:35,97 thread325 INFO com.test.Test Time is 16:12
2023-12-11 16:15:35,97 thread325 INFO com.test.Test :
vital exception occur in the class
com.test.Test : line 5 
Nullpointer Exception
-end-

2023-12-11 17:12:35,97 thread325 INFO com.test.Test Time is 17:12
2023-12-11 18:09:33,987 thread325 INFO com.test.Test Time is 18:09
2023-12-11 18:12:35,97 thread325 INFO com.test.Test Time is 18:12
2023-12-11 18:15:35,97 thread325 INFO com.test.Test :
vital exception occur in the class
com.test.Test : line 9 
Nullpointer Exception
-end-

$
```

从这个例子中，可以看到awk写这种脚本的基本结构、以及如何从命令行接收参数等。学会了这些基本的东西，是掌握这个强大工具的开始。 <br/>


##### 分隔字段并去掉其中的部分 {#分隔字段并去掉其中的部分}

有如下测试文本，记录的是一些比赛数据，test_split.txt： <br/>

```text
2023-12-11 sunny 10:00_LakersVSSpurs_89:103|07:00_76sVSRockets_88:76|10:30_HenanVSTianjin_100:33
2023-12-12 windy 11:00_NuggetsVSShanghai_153:103|06:00_XinjiangVSZhejiang_99:101
2023-12-13 cloudy 12:00_GuangdongVSShenzhen_100:93|07:00_LiaoningVSFujian_99:76|09:30_ShandongVSHunan_100:88
```

下面要将这写数据中，上午6点或者7点的比赛去掉，并按照比赛时间排序后，重新打印。 <br/>
程序如下，split.awk： <br/>

```text
{
	split($3, game_list, "|")
	for(i in game_list){
		# printf("%d %s\n", i, game_list[i])
		if(match(game_list[i], /^07:00|^06:00/)){
			# print game_list[i] " match."
			delete game_list[i]
		}
	}

	len = tidy_arr(game_list)
	#print "len " len
	#print "length " length(game_list)
	isort(game_list, len)
	remain = ""
	for(i = 1; i <= length(game_list); i++){
		remain = remain game_list[i] "|"
	}
	print "old: " $1 " " $2 " " $3
	print "new: " $1 " " $2 " " substr(remain, 1, length(remain) - 1)
}
# isort - sort A[1..n] by insertion. only function param can be local, so there is so many param in this funtion.
function isort(A,n,i,j,t){
	for(i = 2; i <= n; i++)
		for(j=i; j > 1 && A[j-1] > A[j]; j--){
			# swap A[j-1] and A[j]
			t = A[j-1]; A[j-1] = A[j]; A[j] = t
		}
}

# tidy_arr - tidy the array into index start by 1 and in a row, returning the length. only function param can be local, so there is so many param in this funtion.
function tidy_arr(A, i , tmpA){
	i = 1
	for(k in A){
		tmpA[i] = A[k]
		delete A[k]
		i = i + 1
	}
	i = 1
	for(k in tmpA){
		A[i] = tmpA[k]
		i = i + 1
	}
	return i - 1
}
```

运行效果如下： <br/>

```text
$ cat test_split.txt 
2023-12-11 sunny 10:00_LakersVSSpurs_89:103|07:00_76sVSRockets_88:76|10:30_HenanVSTianjin_100:33
2023-12-12 windy 11:00_NuggetsVSShanghai_153:103|06:00_XinjiangVSZhejiang_99:101
2023-12-13 cloudy 12:00_GuangdongVSShenzhen_100:93|07:00_LiaoningVSFujian_99:76|09:30_ShandongVSHunan_100:88
$ awk -f split.awk test_split.txt 
old: 2023-12-11 sunny 10:00_LakersVSSpurs_89:103|07:00_76sVSRockets_88:76|10:30_HenanVSTianjin_100:33
new: 2023-12-11 sunny 10:00_LakersVSSpurs_89:103|10:30_HenanVSTianjin_100:33
old: 2023-12-12 windy 11:00_NuggetsVSShanghai_153:103|06:00_XinjiangVSZhejiang_99:101
new: 2023-12-12 windy 11:00_NuggetsVSShanghai_153:103
old: 2023-12-13 cloudy 12:00_GuangdongVSShenzhen_100:93|07:00_LiaoningVSFujian_99:76|09:30_ShandongVSHunan_100:88
new: 2023-12-13 cloudy 09:30_ShandongVSHunan_100:88|12:00_GuangdongVSShenzhen_100:93
$ 
```

搞定，这个里面有数组、排序、数组整理等相关内容，是一个很好的开始。 <br/>


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
参考[^fn:37] <br/>


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

参考[^fn:38] <br/>


### echo {#echo}


#### 常用选项 {#常用选项}

-   -n, 不换行输出 <br/>
    缺省echo会在输出内容最后追加换行，加了-n之后，就原样输出内容，不会再在最后加换行。 <br/>
-   -e, enable interpretation of backslash escapes <br/>
    输出转义字符，常用的转义字符有\r、\n等，如下[^fn:39]： <br/>
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


### stat {#stat}


#### 选项 {#选项}

-   -c  --format=FORMAT use the specified FORMAT instead of the default; output a newline after each use of FORMAT <br/>
    指定输出的格式。 <br/>
    支持的格式如下： <br/>
    -   %a, permission bits in octal (note '#' and '0' printf flags) <br/>
    -   %A, permission bits and file type in human readable form <br/>
    -   %b, number of blocks allocated (see %B) <br/>
    -   %B, the size in bytes of each block reported by %b <br/>
    -   %C, SELinux security context string <br/>
    -   %d, device number in decimal <br/>
    -   %D, device number in hex <br/>
    -   %f, raw mode in hex <br/>
    -   %F, file type <br/>
        文件类型 <br/>
    -   %g, group ID of owner <br/>
    -   %G, group name of owner <br/>
    -   %h, number of hard links <br/>
    -   %i, inode number <br/>
    -   %m, mount point <br/>
    -   %n, file name <br/>
        文件名 <br/>
    -   %N, quoted file name with dereference if symbolic link <br/>
    -   %o, optimal I/O transfer size hint <br/>
    -   %s, total size, in bytes <br/>
        文件大小，字节数 <br/>
    -   %t, major device type in hex, for character/block device special files <br/>
    -   %T, minor device type in hex, for character/block device special files <br/>
    -   %u, user ID of owner <br/>
    -   %U, user name of owner <br/>
    -   %w, time of file birth, human-readable; - if unknown <br/>
    -   %W, time of file birth, seconds since Epoch; 0 if unknown <br/>
    -   %x, time of last access, human-readable <br/>
        atime，最后的访问时间。 <br/>
    -   %X, time of last access, seconds since Epoch <br/>
    -   %y, time of last data modification, human-readable <br/>
        mtime，最后的修改时间 <br/>
    -   %Y, time of last data modification, seconds since Epoch <br/>
    -   %z, time of last status change, human-readable <br/>
        ctime，最后的状态改变时间 <br/>
    -   %Z, time of last status change, seconds since Epoch <br/>


#### 场景 {#场景}

查看文件的mtime、ctime、atime、文件大小(单位字节)、文件类型(目录or普通文件)、文件名： <br/>

```text
$ stat -c "%y,%z,%x,%s,%F,%n" a.txt 
2023-11-13 00:03:44.037876231 +0800,2023-11-13 00:03:44.037876231 +0800,2023-11-13 00:03:46.829929449 +0800,11,regular file,a.txt
$
```

在macOS上，这个命令的支持不太一样，应该用如下的方式： <br/>

```text
$ stat -f "mtime: %Sm, ctime:%Sc, atime:%Sa, %HT, %z, %N" -t "%Y-%m-%d %H:%M:%S" f.txt 
mtime: 2023-11-23 09:35:55, ctime:2023-11-23 09:35:55, atime:2023-11-23 09:35:57, Regular File, 18, f.txt
```

有时候，stat后面跟的文件数过多，如果用\*，会提示超过文件数上限，这里可以结合find命令一起使用。示意如下： <br/>

```text
$ find . -maxdepth 1 -type f
./.bashrc
./.bash_logout
./a.txt
./.profile
./.lesshst
./.sudo_as_admin_successful
./.bash_history

$ find . -maxdepth 1 -type f -exec stat -c "%y,%z,%x,%s,%F%n" {} \;
2023-11-11 15:36:34.484586384 +0800,2023-11-11 15:36:34.484586384 +0800,2023-11-12 17:17:17.893104109 +0800,3771,regular file./.bashrc
2023-11-11 15:36:34.488587287 +0800,2023-11-11 15:36:34.488587287 +0800,2023-11-13 00:17:28.434611338 +0800,220,regular file./.bash_logout
2023-11-13 00:03:44.037876231 +0800,2023-11-13 00:03:44.037876231 +0800,2023-11-13 00:03:46.829929449 +0800,11,regular file./a.txt
2023-11-11 15:36:34.488587287 +0800,2023-11-11 15:36:34.488587287 +0800,2023-11-12 17:17:17.893104109 +0800,807,regular file./.profile
2023-11-22 23:01:35.615586650 +0800,2023-11-22 23:01:35.615586650 +0800,2023-11-22 23:01:35.611586291 +0800,36,regular file./.lesshst
2023-11-12 17:18:42.733220090 +0800,2023-11-12 17:18:42.733220090 +0800,2023-11-12 17:18:42.733220090 +0800,0,regular empty file./.sudo_as_admin_successful
2023-11-13 00:17:28.442611511 +0800,2023-11-13 00:17:28.442611511 +0800,2023-11-13 00:17:28.442611511 +0800,479,regular file./.bash_history

$ find . -maxdepth 1 -type f -exec stat -c "%y,%z,%x,%s,%F%n" {} \; > result.txt
$ cat result.txt 
2023-11-11 15:36:34.484586384 +0800,2023-11-11 15:36:34.484586384 +0800,2023-11-12 17:17:17.893104109 +0800,3771,regular file./.bashrc
2023-11-11 15:36:34.488587287 +0800,2023-11-11 15:36:34.488587287 +0800,2023-11-13 00:17:28.434611338 +0800,220,regular file./.bash_logout
2023-11-22 23:05:49.876614693 +0800,2023-11-22 23:05:49.876614693 +0800,2023-11-22 23:05:49.860619805 +0800,274,regular file./result.txt
2023-11-13 00:03:44.037876231 +0800,2023-11-13 00:03:44.037876231 +0800,2023-11-13 00:03:46.829929449 +0800,11,regular file./a.txt
2023-11-11 15:36:34.488587287 +0800,2023-11-11 15:36:34.488587287 +0800,2023-11-12 17:17:17.893104109 +0800,807,regular file./.profile
2023-11-22 23:01:35.615586650 +0800,2023-11-22 23:01:35.615586650 +0800,2023-11-22 23:01:35.611586291 +0800,36,regular file./.lesshst
2023-11-12 17:18:42.733220090 +0800,2023-11-12 17:18:42.733220090 +0800,2023-11-12 17:18:42.733220090 +0800,0,regular empty file./.sudo_as_admin_successful
2023-11-13 00:17:28.442611511 +0800,2023-11-13 00:17:28.442611511 +0800,2023-11-13 00:17:28.442611511 +0800,479,regular file./.bash_history
$
```


### ps {#ps}

ps命令可以查看当前服务器上运行的进程： <br/>

```text
$ ps -ax
  PID TTY           TIME CMD
    1 ??        21:56.93 /sbin/launchd
   91 ??       121:26.10 /usr/libexec/logd
   92 ??         3:07.51 /usr/libexec/UserEventAgent (System)
   95 ??         0:15.43 /System/Library/PrivateFrameworks/Uninstall.framework/Resources/uninstalld
   96 ??         4:15.22 /System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/FSEvents.framework/Versions/A/Support/fseventsd
   97 ??         0:10.75 /System/Library/PrivateFrameworks/MediaRemote.framework/Support/mediaremoted
   99 ??        23:09.63 /usr/sbin/systemstats --daemon
```

注意，这里，可能有的进程TIME显示是 `0:00.00` 。因为这的TIME指的是cpu时间，有的进程cpu时间使用较小，比如不到4ms，不到0.01s，就会显示为 `0:00.00`&nbsp;[^fn:40]。 <br/>


### mount {#mount}

mount命令不带任何参数执行，可以查看当前挂载情况： <br/>

```text
$ mount
sysfs on /sys type sysfs (rw,nosuid,nodev,noexec,relatime)
proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
udev on /dev type devtmpfs (rw,nosuid,relatime,size=1944272k,nr_inodes=486068,mode=755,inode64)
devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000)
tmpfs on /run type tmpfs (rw,nosuid,nodev,noexec,relatime,size=396444k,mode=755,inode64)
/dev/sda3 on / type ext4 (rw,relatime,errors=remount-ro)
securityfs on /sys/kernel/security type securityfs (rw,nosuid,nodev,noexec,relatime)
tmpfs on /dev/shm type tmpfs (rw,nosuid,nodev,inode64)
tmpfs on /run/lock type tmpfs (rw,nosuid,nodev,noexec,relatime,size=5120k,inode64)
cgroup2 on /sys/fs/cgroup type cgroup2 (rw,nosuid,nodev,noexec,relatime,nsdelegate,memory_recursiveprot)
pstore on /sys/fs/pstore type pstore (rw,nosuid,nodev,noexec,relatime)
bpf on /sys/fs/bpf type bpf (rw,nosuid,nodev,noexec,relatime,mode=700)
systemd-1 on /proc/sys/fs/binfmt_misc type autofs (rw,relatime,fd=29,pgrp=1,timeout=0,minproto=5,maxproto=5,direct,pipe_ino=24624)
hugetlbfs on /dev/hugepages type hugetlbfs (rw,relatime,pagesize=2M)
mqueue on /dev/mqueue type mqueue (rw,nosuid,nodev,noexec,relatime)
debugfs on /sys/kernel/debug type debugfs (rw,nosuid,nodev,noexec,relatime)
tracefs on /sys/kernel/tracing type tracefs (rw,nosuid,nodev,noexec,relatime)
fusectl on /sys/fs/fuse/connections type fusectl (rw,nosuid,nodev,noexec,relatime)
configfs on /sys/kernel/config type configfs (rw,nosuid,nodev,noexec,relatime)
```

可以看到各个卷的挂载选项，可以看到，大部分都是relatime选项。 <br/>


### Ctrl+Z、fg和jobs命令 {#ctrl-plus-z-fg和jobs命令}

Ctrl+Z实际上是将文件放到后台运行。jobs命令，可以查看当前有哪些进程。如果使用 `fg n` 命令，可以将后台运行的第n个任务切换到前台。其中的n可以通过jobs命令查看。 <br/>
这个应该只是限于当前的这个登录会话，如果关掉这个会话，或者在别的登录会话中，都看不到其他的后台任务信息，即使是同一个用户也不行。 <br/>


### lsof {#lsof}

lsof 是 linux 下的一个非常实用的系统级的监控、诊断工具。它是 List Open Files的缩写。使用 lsof，你可以获取任何被打开文件的各种信息，因为 lsof 需要访问核心内存和各种文件，所以必须以 root 用户的身份运行它才能够充分地发挥其功能[^fn:41]。 <br/>
linux中，有时候，删除文件之后，磁盘空间并未释放。这是因为有文件占用，需要查看占用删除文件的进程，然后，关掉或者重启这个进程，磁盘空间就会被释放。执行如下命令，查找占用文件的进程： <br/>

```text
lsof | grep delete
```

当然，这种情况，也可以直接执行如下重定向命令 `echo " " > asdf.txt` ，来解决这个文件占用的问题，而且不用重启。 <br/>


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


### locale {#locale}

参考[^fn:42] <br/>


#### 机制说明 {#机制说明}

locale用来设置和显示程序运行的语言环境，它会根据计算机用户所使用的语言，所在国家或地区及当地的文化传统定义一个软件运行时的语言环境。 <br/>
设置规则如下： <br/>

```text
<语言>_<地区>.<字符集编码><@修正值>
```

示例如下： <br/>

```text
zh_CN.utf8
zh：表示中文
CN：表示大陆地区
Utf8：表示字符集

de_DE.utf-8@euro
de：表示德语
DE：表示德国
Utf-8：表示字符集
euro：表示按照欧洲习惯加以修正
```

locale默认文件系/usr/share/i18n/locales，包含一组总共12个LC开头的变量，不包括LANG和LC_ALL，如下： <br/>

```text
LC_CTYPE：用于字符分类和字符串处理，控制所有字符的处理方式，包括字符编码，字符是单字节还是多字节，如何打印等，非常重要的一个变量。
LC_NUMERIC：用于格式化非货币的数字显示
LC_TIME：用于格式化时间和日期
LC_COLLATE：用于比较和排序
LC_MONETARY：用于格式化货币单位
LC_MESSAGES：用于控制程序输出时所使用的语言，主要是提示信息，错误信息，状态信息，标题，标签，按钮和菜单等
LC_PAPER：默认纸张尺寸大小
LC_NAME：姓名书写方式
LC_ADDRESS：地址书写方式
LC_TELEPHONE：电话号码书写方式
LC_MEASUREMENT：度量衡表达方式
LC_IDENTIFICATION：locale对自身包含信息的概述
```

LANG：LANG的优先级是最低的，它是所有LC_\*变量的默认值，所有以LC_开头变量（LC_ALL除外）中，如果存在没有设置变量值的变量，将会使用LANG的变量值。如果变量有值，则保持不变。 <br/>
LC_ALL：它不是环境变量，它是一个宏，它可通过该变量的设置覆盖所有LC_\*变量，这个变量设置之后，可以废除LC_\*的设置值，使得这些变量的设置值与LC_ALL的值一致，注意LANG变量不受影响。 <br/>
这样可以看出优先级： `LC_ALL > LC_* > LANG` 。 <br/>


#### 相关命令 {#相关命令}

查看当前locale设置 <br/>

```text
locale
```

查看当前系统所有可用locale <br/>

```text
locale -a
```

设置locale： <br/>

-   通过命令行执行命令 <br/>
    ```text
    localectl set-locale LANG=en_US.UTF-8
    ```
    这个我没自己试。 <br/>
-   修改/etc/profile <br/>
    在文件末尾增加： <br/>
    ```text
    export LC_ALL=zh_CN.utf8
    export LANG=zh_CN.utf8
    ```
    source执行下该配置文件即可生效。 <br/>


### crontab {#crontab}

有一次，配置crontab之后，发现会重复执行，检查了下，原来在多个用户下，都有同一个crontab命令配置。这样，每个用户下都会执行，导致重复执行。 <br/>


### file {#file}

这个命令可以用来查看文件的类型和编码，但实际用的时候，发现这个命令不能正确识别gb2312、gbk、gb18030等这些汉字编码。实际上，对于有BOM的文件(比如UTF-16BE、UTF-16LE等)，可以直接识别出他的编码，对于其他的文件，只能猜，可能需要用统计学的理论进行辅助判断。 <br/>


#### 常用选项 {#常用选项}

-   -i, <br/>
    macos上这个选项是指定如果是普通文件，不区分具体类型；Ubuntu上的意思是打印文件的MIME(文件类型和编码)。在macos上如果要查看文件的具体类型和编码，需要指定 `-I` 选项。 <br/>


#### 场景 {#场景}


##### 查看文件的编码类型 {#查看文件的编码类型}

macos： <br/>

```text
$ cat a.txt 
阿斯顿发垃圾
$ file a.txt 
a.txt: Unicode text, UTF-8 text
$ file -i a.txt 
a.txt: regular file
$ file -I a.txt 
a.txt: text/plain; charset=utf-8
$
```

Ubuntu: <br/>

```text
$ cat a.txt 
a燊b
$ file a.txt 
a.txt: Unicode text, UTF-8 text
$ file -i a.txt 
a.txt: text/plain; charset=utf-8
$ file -I a.txt 
file: invalid option -- 'I'
Usage: file [-bcCdEhikLlNnprsSvzZ0] [--apple] [--extension] [--mime-encoding]
	    [--mime-type] [-e <testname>] [-F <separator>]  [-f <namefile>]
	    [-m <magicfiles>] [-P <parameter=value>] [--exclude-quiet]
	    <file> ...
       file -C [-m <magicfiles>]
       file [--help]
$
```


### xxd {#xxd}

用于查看文本文件的16进制格式。 <br/>

```text
$ cat a.txt 
aa阿斯顿发垃圾xx
$ xxd a.txt 
00000000: 6161 e998 bfe6 96af e9a1 bfe5 8f91 e59e  aa..............
00000010: 83e5 9cbe 7878 0a                        ....xx.
$
```

从这里可以看出，文件中的中文都是用"."来取代的，不展示中文内容。 <br/>


### test {#test}


#### 说明 {#说明}

macOS上的man page说明： <br/>

```text
The test utility evaluates the expression and, if it evaluates to true, returns a zero (true) exit status; otherwise it returns 1 (false).  If there is no expression, test also returns 1 (false).
```

test用来检测表达式真假，如果为真返回0，如果为假或者没有表达式返回0。可以通过 `$?` 来查看test命令的返回值。 <br/>
macOS上的man page支持如下表达式： <br/>

```text
The following primaries are used to construct expression:
     -b file       True if file exists and is a block special file.
     -c file       True if file exists and is a character special file.
     -d file       True if file exists and is a directory.
     -e file       True if file exists (regardless of type).
     -f file       True if file exists and is a regular file.
     -g file       True if file exists and its set group ID flag is set.
     -h file       True if file exists and is a symbolic link.  This operator is retained for compatibility with previous versions of this program.  Do not rely on its existence; use -L instead.
     -k file       True if file exists and its sticky bit is set.
     -n string     True if the length of string is nonzero.
     -p file       True if file is a named pipe (FIFO).
     -r file       True if file exists and is readable.
     -s file       True if file exists and has a size greater than zero.
     -t file_descriptor
		   True if the file whose file descriptor number is file_descriptor is open and is associated with a terminal.
     -u file       True if file exists and its set user ID flag is set.
     -w file       True if file exists and is writable.  True indicates only that the write flag is on.  The file is not writable on a read-only file system even if this test indicates true.
     -x file       True if file exists and is executable.  True indicates only that the execute flag is on.  If file is a directory, true indicates that file can be searched.
     -z string     True if the length of string is zero.
     -L file       True if file exists and is a symbolic link.
     -O file       True if file exists and its owner matches the effective user id of this process.
     -G file       True if file exists and its group matches the effective group id of this process.
     -S file       True if file exists and is a socket.
     file1 -nt file2
		   True if file1 exists and is newer than file2.
     file1 -ot file2
		   True if file1 exists and is older than file2.
     file1 -ef file2
		   True if file1 and file2 exist and refer to the same file.
     string        True if string is not the null string.
     s1 = s2       True if the strings s1 and s2 are identical.
     s1 != s2      True if the strings s1 and s2 are not identical.
     s1 < s2       True if string s1 comes before s2 based on the binary value of their characters.
     s1 > s2       True if string s1 comes after s2 based on the binary value of their characters.
     n1 -eq n2     True if the integers n1 and n2 are algebraically equal.
     n1 -ne n2     True if the integers n1 and n2 are not algebraically equal.
     n1 -gt n2     True if the integer n1 is algebraically greater than the integer n2.
     n1 -ge n2     True if the integer n1 is algebraically greater than or equal to the integer n2.
     n1 -lt n2     True if the integer n1 is algebraically less than the integer n2.
     n1 -le n2     True if the integer n1 is algebraically less than or equal to the integer n2.
     If file is a symbolic link, test will fully dereference it and then evaluate the expression against the file referenced, except for the -h and -L primaries.
     These primaries can be combined with the following operators:
     ! expression  True if expression is false.
     expression1 -a expression2
		   True if both expression1 and expression2 are true.
     expression1 -o expression2
		   True if either expression1 or expression2 are true.
     ( expression )
		   True if expression is true.
     The -a operator has higher precedence than the -o operator.
     Some shells may provide a builtin test command which is similar or identical to this utility. 
```


#### 场景 {#场景}

比较两个字符串的大小： <br/>

```text
$ test "asdf" \= "asdf"
$ echo $?
0
$ test "asdf" = "asdf"
$ echo $?
0
$ test "asdf" = "qwer"
$ echo $?
1
$ test "asdf" \> "qwer"
$ echo $?
1
$ test "asdf" \< "qwer"
$ echo $?
0
$
```

注意，如下方式是不行的： <br/>

```text
$ test "asdf > qwer"
$ echo $?
0
$ test "asdf \> qwer"
$ echo $?
0
$
```


### gzip和gunzip {#gzip和gunzip}

压缩和解压缩的使用很简单，如下： <br/>

```text
$ ls a.foo*
a.foo
$ cat a.foo 
asdfa
$ gzip a.foo 
$ ls a.foo*
a.foo.gz
$ gunzip a.foo.gz 
$ ls a.foo*
a.foo
$ cat a.foo 
asdfa
$ 
```

可以看出，无论是解压、还是压缩，都会自动删除原文件。 <br/>


### curl {#curl}


#### 常用选项 {#常用选项}

-   --resolve，--resolve &lt;[+]host:port:addr[,addr]...&gt; <br/>
    macos上的面 <br/>
    ```text
    Provide a custom address for a specific host and port pair. Using this, you can make the curl requests(s) use a specified address and prevent the otherwise normally resolved address to be used. Consider it a sort of /etc/hosts alternative provided on the command line. The port number should be the number used for the specific protocol the host will be used for. It means you need several entries if you want to provide address for the same host but different ports.
    
    By specifying '*' as host you can tell curl to resolve any host and specific port pair to the specified address. Wildcard is resolved last so any --resolve with a specific host and port will be used first.
    
    The provided address set by this option will be used even if -4, --ipv4 or -6, --ipv6 is set to make curl use another IP version.
    
    By prefixing the host with a '+' you can make the entry time out after curl's default timeout (1 minute). Note that this will only make sense for long running parallel transfers with a lot of files. In such cases, if this option is used curl will try to resolve the host as it normally would once the timeout has expired.
    
    Support for providing the IP address within [brackets] was added in 7.57.0.
    Support for providing multiple IP addresses per entry was added in 7.59.0.
    Support for resolving with wildcard was added in 7.64.0.
    Support for the '+' prefix was was added in 7.75.0.
    This option can be used many times to add many host names to resolve.
    Example:
     curl --resolve example.com:443:127.0.0.1 https://example.com
    
    Added in 7.21.3.
    ```


#### 典型场景 {#典型场景}

-   curl带静态dns解析 <br/>
    如下示例： <br/>
    ```text
    $ curl 127.0.0.1
    <html><body><h1>It works!</h1></body></html>
    $ curl 127.0.0.1:80
    <html><body><h1>It works!</h1></body></html>
    $ curl --resolve example.com:80:127.0.0.1 example.com
    <html><body><h1>It works!</h1></body></html>
    $ curl --resolve example.com:80:127.0.0.1 example.com:80
    <html><body><h1>It works!</h1></body></html>
    $ curl --resolve example.com:80:127.0.0.1 --resolve foobar.com:80:127.0.0.1 foobar.com
    <html><body><h1>It works!</h1></body></html>
    $ curl --resolve example.com:80:127.0.0.1 --resolve foobar.com:80:127.0.0.1 example.com
    <html><body><h1>It works!</h1></body></html>
    $ 
    ```
    这里，需要注意下，dns应该是不带端口的。但是，curl指定的时候，必须要指定端口。后面的使用，如果不是默认端口，还应该显式指定端口。 <br/>


### eval {#eval}

通过这个命令，可以执行保存在一个变量中的命令。 <br/>
可以用通过它，实现将一个变量中的变量名替换为变量的值。如下： <br/>

```text
$ cat_name=PaoPao
$ action='hit $cat_name'
$ action_str='hit $cat_name'
$ echo $action_str
hit $cat_name
$ action_res=$(eval "echo $action_str")
$ echo $action_res
hit PaoPao
$ 
```


### ssh {#ssh}

要通过SSH连接到远程服务器并执行命令，可以使用以下命令： <br/>

```text
ssh username@remote_server 'command'
```

其中，username为登录远程服务器的用户名，remote_server为远程服务器的IP地址或主机名。'command'表示需要在远程服务器上执行的命令。 <br/>
如果想要将结果保存到本地文件中，可以使用重定向符号 &gt; 来指定输出文件路径： <br/>

```text
ssh username@remote_server 'command > output.txt'
```

这样会将命令的输出内容写入到远程服务器当前目录下的output.txt文件中。 <br/>
如果要将命令输出内容写入本地服务器当前目录，可以使用命令 <br/>

```text
ssh username@remote_server 'command' > output.txt
```


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

参考[^fn:43] <br/>


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
参考[^fn:44]。 <br/>


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
    其中， `_end_` 可以是任何内容，只要上下一样就可以。但是中间不能出现 `_end_` 开头的行，否则提前结束[^fn:45]。 <br/>


### 遍历目录中的全部文件 {#遍历目录中的全部文件}

如下： <br/>

```text
$ cat file/getFileListOfDirRecursive.sh 
function getFileListOfDir(){
	echo "now dir: $1"
	# 判断目录是否为空
	if [ -z "$(ls -A $1)" ]; then
		# 目录为空，结束该函数执行
		return 0
	fi

	# 如果$1指定的目录为空，其中没有文件，会都到如下循环体if判断中的else分支，无限递归，因此需要加前面的目录为空判断
	for file in $1/*
	do
		if test -f $file
		then
			echo "now file: $file"
		else
			getFileListOfDir $file
		fi
	done
}
# 接收脚本的第一个参数作为指定目录
getFileListOfDir $1
$ sh file/getFileListOfDirRecursive.sh .
now dir: .
now file: ./20231026_ShellTutorial.html
now file: ./20231026_ShellTutorial.html~
now file: ./20231026_ShellTutorial.org
now file: ./20231026_ShellTutorial.org~
now dir: ./adsf
now dir: ./file
now file: ./file/getFileListOfDirRecursive.sh
now file: ./getFileListOfDirRecursive.sh
now dir: ./img
now file: ./img/01_RegexCompare.png
$
```

注意：这个脚本应该可以处理目录和文件名中有空格的情况，因为用的是星花展开做的遍历。 <br/>


### 递归打印目录结构的脚本 {#递归打印目录结构的脚本}

如下： <br/>

```text
$ cat traverseShowDirectoryStructure.sh 
# 接收三个参数：目录、遍历层级、补全前缀
function getFileListOfDir(){
	# $1, now dir
	# $2, traversing hierarchy
	# $3, output prefix
	# 判断目录是否为空
	if [ $2 -gt 0 ]; then
		if [ -z "$(ls -A "$1")" ]; then
			# 目录为空，结束该函数执行
			return 0
		fi

		# 这里只能用星花展开，不能解析ls的输出，不然不支持文件和目录中带空格的情况
		for file in "$1"/* "$1"/.*
		do
			# 忽略.和..
			case "$file" in "$1/."|"$1/..") continue; esac
			if test -f "$file"
			then
				echo "$3""${file##*/}"
			elif test -d "$file"
			then
				echo "$3""${file##*/}/"
				hierarchy=$2
				getFileListOfDir "$file" $((hierarchy - 1)) "$3|------"
			else
				# 走到这儿说明这个目录中，只有.开头的文件
				# 半角冒号用于占位，空语句
				:
			fi
		done
	else
		return 0;
	fi
}

# 接收三个参数：$1目录、$2遍历层级、$3补全前缀
if [ -z "$(ls -A $1)" ]; then
	# 目录为空，直接打印目录名，然后结束函数执行
	echo "$1/"
else
	echo "$1/"
	# 接收脚本的第一个参数作为指定目录
	getFileListOfDir "$1" "$2" "|------"
fi
```

这里需要注意，在实现这个循环遍历时，不能用解析ls输出的方式遍历文件。只能用星花展开的方式，不然不支持文件名和目录名中有空格的情况。 <br/>
运行效果如下： <br/>

```text
$ pwd
/Users/chengxia/Developer/roam/blog/20231026_ShellTutorial/file
$ sh traverseShowDirectoryStructure.sh ~/temp/tar_test 3  ""
/Users/chengxia/temp/tar_test/
|------a.txt
|------all.tar
|------all0/
|------|------a.txt
|------|------b.txt
|------|------c.txt
|------all1/
|------|------a.txt
|------|------b.txt
|------|------c.txt
|------b.txt
|------c.txt
|------log.log
|------tar_test2/
|------|------a.txt
|------|------all.tar
|------|------asdf/
|------|------|------a.txt
|------|------|------b.txt
|------|------|------c.txt
|------|------b.txt
|------|------c.txt
|------|------.a.txt.swp
$
```

[^fn:1]: [shell prompt和Carriage Return的关系](http://bbs.chinaunix.net/forum.php?mod=viewthread&tid=218853&page=2#pid1467910)  <br/>
[^fn:2]: [Linux Shell 13问，单引号和双引号的区别](http://bbs.chinaunix.net/forum.php?mod=viewthread&tid=218853&page=4#pid1511745) <br/>
[^fn:3]: [linux 通配符（wildcard）](https://blog.csdn.net/bdss58/article/details/78568019)  <br/>
[^fn:4]: [Linux Shell基础 多个命令中的分号(;)、与(&amp;&amp;) 、 或(||)](https://www.cnblogs.com/lizhouwei/p/9991635.html) <br/>
[^fn:5]: [Illustrated Redirection Tutorial](https://web.archive.org/web/20230315225157/https://wiki.bash-hackers.org/howto/redirection_tutorial)  <br/>
[^fn:6]: [3.6.9 Moving File Descriptors](https://www.gnu.org/software/bash/manual/html_node/Redirections.html) <br/>
[^fn:7]: [Bash shell 的算术运算有四种方式](https://blog.csdn.net/hansel/article/details/8736775) <br/>
[^fn:8]: [Shell脚本-全局变量、局部变量、环境变量](https://blog.csdn.net/zjc156m/article/details/118582021) <br/>
[^fn:9]: [shell if 判断，字符正则匹配](https://www.itxm.cn/post/ajbjje1a1.html) <br/>
[^fn:10]: [Regex](https://en.wikipedia.org/wiki/Regular_expression#POSIX)  <br/>
[^fn:11]: [RegEx with \d doesn’t work in if-else statement](https://askubuntu.com/questions/1143710/regex-with-d-doesn-t-work-in-if-else-statement-with#:~:text=d%20and%20w%20don%27t%20work%20in%20POSIX%20regular,%5B%3Adigit%3A%5D%5D%2B-%2B%20%5D%5D%3Bthen%20echo%20%22Pre%22%20else%20echo%20%22Release%22%20fi) <br/>
[^fn:12]: [nohup命令_Linux nohup命令详解，终端关闭程序依然可以在执行](https://blog.csdn.net/weixin_39871562/article/details/110891430) <br/>
[^fn:13]: [shell脚本停顿一段时间的命令](https://blog.csdn.net/songpeiying/article/details/134107553)  <br/>
[^fn:14]: [shell统计循环次数的方法](https://blog.csdn.net/tjcwt2011/article/details/128498972)  <br/>
[^fn:15]: [hostname man page](https://www.man7.org/linux/man-pages/man1/hostname.1.html) <br/>
[^fn:16]: [How do I get the current Unix time in milliseconds in Bash?](https://serverfault.com/questions/151109/how-do-i-get-the-current-unix-time-in-milliseconds-in-bash) <br/>
[^fn:17]: [How to use '-prune' option of 'find' in sh?](https://stackoverflow.com/questions/1489277/how-to-use-prune-option-of-find-in-sh)  <br/>
[^fn:18]: [How do I exclude a directory when using \`find\`?](https://stackoverflow.com/questions/4210042/how-do-i-exclude-a-directory-when-using-find/15736463?r=SearchResults&s=1%7C302.8193#15736463) <br/>
[^fn:19]: [find排除一个或多个目录的方法](https://www.jianshu.com/p/428ef40e384d)  <br/>
[^fn:20]: [find命令的prune用法总结](https://blog.csdn.net/weixin_43999327/article/details/118653060) <br/>
[^fn:21]: [彻底搞明白find命令mtime含义和用法](https://blog.csdn.net/db_murphy/article/details/107053545)  <br/>
[^fn:22]: [How to display modified date time with 'find' command?](https://stackoverflow.com/questions/20893022/how-to-display-modified-date-time-with-find-command) <br/>
[^fn:23]: [man page of find](https://man7.org/linux/man-pages/man1/find.1.html)  <br/>
[^fn:24]: [find lacks the option -printf, now what?](https://stackoverflow.com/questions/752818/find-lacks-the-option-printf-now-what) <br/>
[^fn:25]: [find命令根据文件大小来搜索文件](https://zhuanlan.zhihu.com/p/616467047?utm_id=0) <br/>
[^fn:26]: [Executing Multiple Commands in Find -exec](https://www.baeldung.com/linux/find-exec-multiple-commands)  <br/>
[^fn:27]: [find 删除命令rm还是delete更快](https://blog.51cto.com/dbadadong/5196168)  <br/>
[^fn:28]: [Linux tar 命令](https://www.runoob.com/linux/linux-comm-tar.html)  <br/>
[^fn:29]: [GNU tar - update tar file, overwriting the original file in command line](https://askubuntu.com/questions/1384589/gnu-tar-update-tar-file-overwriting-the-original-file-in-command-linewhich-i)  <br/>
[^fn:30]: [linux 命令：echo 详解](https://blog.csdn.net/yspg_217/article/details/122187643)  <br/>
[^fn:31]: [Cutting the column including size](https://stackoverflow.com/questions/16374616/cutting-the-column-including-size) <br/>
[^fn:32]: [Bash read 命令读数据](https://www.junmajinlong.com/shell/script_course/shell_read/) <br/>
[^fn:33]: [Unix文本处理工具之awk](https://blog.csdn.net/xia7139/article/details/49806421) <br/>
[^fn:34]: [How to select lines between two marker patterns which may occur multiple times with awk/sed](https://stackoverflow.com/questions/17988756/how-to-select-lines-between-two-marker-patterns-which-may-occur-multiple-times-w) <br/>
[^fn:35]: [awk print matching line and line before the matched](https://stackoverflow.com/questions/4891383/awk-print-matching-line-and-line-before-the-matched)  <br/>
[^fn:36]: [What is "-bash: !": event not found"](https://serverfault.com/questions/208265/what-is-bash-event-not-found) <br/>
[^fn:37]: [Linux常用命令——comm命令](https://blog.csdn.net/weixin_43251547/article/details/128597850)  <br/>
[^fn:38]: [Find common files between two folders](https://stackoverflow.com/questions/38827243/find-common-files-between-two-folders)  <br/>
[^fn:39]: [別人 echo、你也 echo ，是問 echo 知多少？](http://bbs.chinaunix.net/forum.php?mod=viewthread&tid=218853&page=3#pid1482452)  <br/>
[^fn:40]: [Time field showing Zero in ps command output](https://community.unix.com/t/time-field-showing-zero-in-ps-command-output/352415) <br/>
[^fn:41]: [Linux命令详解（15）lsof命令](https://blog.csdn.net/bigwood99/article/details/126834989)  <br/>
[^fn:42]: [linux下设置locale](https://cloud.tencent.com/developer/article/1671446?from=15425) <br/>
[^fn:43]: [How to get the Bash version number](https://stackoverflow.com/questions/9450604/how-to-get-the-bash-version-number) <br/>
[^fn:44]: [remove first line in bash](https://superuser.com/questions/284258/remove-first-line-in-bash)  <br/>
[^fn:45]: [shell同时输出多行信息](https://blog.51cto.com/u_15127527/3388614)  <br/>