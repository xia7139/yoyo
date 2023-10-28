+++
title = "Shell语法说明"
author = ["Cheng Xia"]
date = 2023-10-26
draft = false
+++

<div class="ox-hugo-toc toc">

<div class="heading">Table of Contents</div>

- [字符串操作](#字符串操作)
    - [按照长度截取](#按照长度截取)
    - [按照内容截取](#按照内容截取)
- [条件判断](#条件判断)
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
    - [date](#date)
    - [find](#find)
        - [按照文件名查找](#按照文件名查找)
        - [打印时间](#打印时间)
    - [tar](#tar)
        - [常用选项说明](#常用选项说明)
        - [打包](#打包)
            - [简单将同一目录下的多个文件打成一个tar](#简单将同一目录下的多个文件打成一个tar)
            - [查看tar包中的文件列表](#查看tar包中的文件列表)
            - [往存量tar中添加文件](#往存量tar中添加文件)
        - [解包](#解包)
    - [awk](#awk_command_introduction)
    - [cut](#cut)
        - [常用选项](#常用选项)
        - [典型场景](#典型场景)
    - [cp](#cp)
- [典型场景](#典型场景)
    - [查找两个目录中的同名文件](#查找两个目录中的同名文件)

</div>
<!--endtoc-->

本文主要介绍shell用法，基本是基于bash。 <br/>


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


## 条件判断 {#条件判断}


### 基本语法 {#基本语法}

if语句的基本语法[^fn:1]如下： <br/>

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

这里的正则是posix规范的[^fn:2]，而非一般的perl或者javascript[^fn:3]，如下： <br/>
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

参考[^fn:4] <br/>

```bash
i=1
while [ $i -lt 10 ]
do
echo $i;
i=$(($i+1));
done
```


### for {#for}

参考[^fn:4] <br/>

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

参考：[^fn:5] <br/>

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
    指定显示change time。 <br/>
-   -u, Use time of last access, instead of time of last modification of the file for sorting (-t) or long printing (-l). <br/>
    指定显示上次访问时间。 <br/>
-   -s, Display the number of blocks used in the file system by each file. <br/>
    以块为单位列出文件占用的大小。 <br/>


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


### find {#find}


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

参考[^fn:6] <br/>

```bash
find . -maxdepth 1 -type f -printf "%p %TY-%Tm-%Td %TH:%TM:%TS %Tz\n"
```

这里面%p表示文件名，%T表示是修改时间，可以换成%C表示拿到状态改变的时间，换成%A拿到上次访问的时间[^fn:7]。 <br/>
不过，macos中的find命令，并不支持-printf选项[^fn:8]。 <br/>


### tar {#tar}

tar命令的使用 <br/>
tar(英文全拼：tape archive)命令用于备份文件。tar是用来建立，还原备份文件的工具程序，它可以加入，解开备份文件内的文件。[^fn:9] <br/>


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

注意，这里tar不支持在打包的时候，覆盖包中已有的同名文件。原因可能是因为，这个命令诞生于磁带打包场景，磁带这种存储介质，只适合追加写，更新前面的内容(随记写)效率不佳。参考[^fn:10] <br/>


#### 解包 {#解包}


### awk {#awk_command_introduction}


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

这里echo命令的-e选项是为了启用反斜杠转义[^fn:11]。 <br/>

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

不过，处理ls命令的输出，还是结合awk命令使用比较好[^fn:12]。如下： <br/>

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


## 典型场景 {#典型场景}


### 查找两个目录中的同名文件 {#查找两个目录中的同名文件}

```text
$ ls tar_test/
a.txt	all.tar	all0	all1	b.txt	c.txt
$ ls hhh_test/
a.txt	c.txt
$ comm -1 -2 <(ls tar_test | sort) <(ls hhh_test | sort)
a.txt
c.txt
$
```

参考[^fn:13] <br/>

[^fn:1]: [shell if 判断，字符正则匹配](https://www.itxm.cn/post/ajbjje1a1.html) <br/>
[^fn:2]: [Regex](https://en.wikipedia.org/wiki/Regular_expression#POSIX)  <br/>
[^fn:3]: [RegEx with \d doesn’t work in if-else statement](https://askubuntu.com/questions/1143710/regex-with-d-doesn-t-work-in-if-else-statement-with#:~:text=d%20and%20w%20don%27t%20work%20in%20POSIX%20regular,%5B%3Adigit%3A%5D%5D%2B-%2B%20%5D%5D%3Bthen%20echo%20%22Pre%22%20else%20echo%20%22Release%22%20fi) <br/>
[^fn:4]: [shell统计循环次数的方法](https://blog.csdn.net/tjcwt2011/article/details/128498972)  <br/>
[^fn:5]: [hostname man page](https://www.man7.org/linux/man-pages/man1/hostname.1.html) <br/>
[^fn:6]: [How to display modified date time with 'find' command?](https://stackoverflow.com/questions/20893022/how-to-display-modified-date-time-with-find-command) <br/>
[^fn:7]: [man page of find](https://man7.org/linux/man-pages/man1/find.1.html)  <br/>
[^fn:8]: [find lacks the option -printf, now what?](https://stackoverflow.com/questions/752818/find-lacks-the-option-printf-now-what) <br/>
[^fn:9]: [Linux tar 命令](https://www.runoob.com/linux/linux-comm-tar.html)  <br/>
[^fn:10]: [GNU tar - update tar file, overwriting the original file in command line](https://askubuntu.com/questions/1384589/gnu-tar-update-tar-file-overwriting-the-original-file-in-command-linewhich-i)  <br/>
[^fn:11]: [linux 命令：echo 详解](https://blog.csdn.net/yspg_217/article/details/122187643)  <br/>
[^fn:12]: [Cutting the column including size](https://stackoverflow.com/questions/16374616/cutting-the-column-including-size) <br/>
[^fn:13]: [Find common files between two folders](https://stackoverflow.com/questions/38827243/find-common-files-between-two-folders)  <br/>