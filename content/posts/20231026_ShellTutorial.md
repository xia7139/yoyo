+++
title = "Shell教程"
author = ["Cheng Xia"]
date = 2023-10-26
draft = false
+++

<div class="ox-hugo-toc toc">

<div class="heading">Table of Contents</div>

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
    - [tar](#tar)
        - [常用选项说明](#常用选项说明)
        - [打包](#打包)
            - [简单将同一目录下的多个文件打成一个tar](#简单将同一目录下的多个文件打成一个tar)
            - [查看tar包中的文件列表](#查看tar包中的文件列表)
            - [往存量tar中添加文件](#往存量tar中添加文件)
        - [解包](#解包)
    - [cp](#cp)

</div>
<!--endtoc-->

本文主要介绍shell用法，基本是基于bash。 <br/>


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


### tar {#tar}

tar命令的使用 <br/>
tar(英文全拼：tape archive)命令用于备份文件。tar是用来建立，还原备份文件的工具程序，它可以加入，解开备份文件内的文件。[^fn:5] <br/>


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

注意，这里tar不支持在打包的时候，覆盖包中已有的同名文件。原因可能是因为，这个命令诞生于磁带打包场景，磁带这种存储介质，只适合追加写，更新前面的内容(随记写)效率不佳。参考[^fn:6] <br/>


#### 解包 {#解包}


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

[^fn:1]: [shell if 判断，字符正则匹配](https://www.itxm.cn/post/ajbjje1a1.html) <br/>
[^fn:2]: [Regex](https://en.wikipedia.org/wiki/Regular_expression#POSIX)  <br/>
[^fn:3]: [RegEx with \d doesn’t work in if-else statement with [[](https://askubuntu.com/questions/1143710/regex-with-d-doesn-t-work-in-if-else-statement-with#:~:text=d%20and%20w%20don%27t%20work%20in%20POSIX%20regular,%5B%3Adigit%3A%5D%5D%2B-%2B%20%5D%5D%3Bthen%20echo%20%22Pre%22%20else%20echo%20%22Release%22%20fi) <br/>
[^fn:4]: [shell统计循环次数的方法](https://blog.csdn.net/tjcwt2011/article/details/128498972)  <br/>
[^fn:5]: [Linux tar 命令](https://www.runoob.com/linux/linux-comm-tar.html)  <br/>
[^fn:6]: [GNU tar - update tar file, overwriting the original file in command line](https://askubuntu.com/questions/1384589/gnu-tar-update-tar-file-overwriting-the-original-file-in-command-linewhich-i)  <br/>