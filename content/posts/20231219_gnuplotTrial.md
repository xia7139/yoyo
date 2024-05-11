+++
title = "gnuplot画散点图简易示例"
author = ["Cheng Xia"]
date = 2023-12-19
draft = false
+++

<div class="ox-hugo-toc toc">

<div class="heading">Table of Contents</div>

- [简介](#简介)
- [安装和启动](#安装和启动)
- [画散点图](#画散点图)
    - [示例](#示例)
    - [简单的样式控制](#简单的样式控制)
    - [将画图命令写入脚本](#将画图命令写入脚本)
- [结合awk命令画图](#结合awk命令画图)

</div>
<!--endtoc-->



## 简介 {#简介}

就像名字所表示的一样，gnuplot是一个画图软件。可以用来绘制2d或3d的数据或者函数图像。尽管名字中有gnu，但是，它和GNU没有关系，授权协议也不是GNU GPL，它的gnu是小写，读作"new plot"[^fn:1]。 <br/>


## 安装和启动 {#安装和启动}

安装很容易，安装完成后，windows上打开桌面，或者在Linux上执行gnuplot命令，都可以得到一个命令执行环境，如下是一个mac系统上的示例： <br/>

```text
$ gnuplot

	G N U P L O T
	Version 5.4 patchlevel 10    last modified 2023-10-20

	Copyright (C) 1986-1993, 1998, 2004, 2007-2023
	Thomas Williams, Colin Kelley and many others

	gnuplot home:     http://www.gnuplot.info
	faq, bugs, etc:   type "help FAQ"
	immediate help:   type "help"  (plot window: hit 'h')

Terminal type is now 'qt'
gnuplot> 
```

类似于python，输入命令之后，就可以画图。如下示例： <br/>
![](/ox-hugo/02_gnuplotSinX.png) <br/>


## 画散点图 {#画散点图}


### 示例 {#示例}

有如下数据： <br/>

```text
$ cat precipitation1.dat 
# 各城市月平均降水量 (mm) #
# 月份 北京 上海 
# ====================== 
1 2.5 38.1
2 5.1 58.4
3 10.2 81.3
4 25.4 101.6
5 27.9 114.3
6 71.1 152.4
7 175.3 129.5
8 182.9 132.1
9 48.3 154.9
10 17.8 61.0
11 5.1 50.8
12 2.5 35.6
$ cat precipitation2.dat 
# 各城市月平均降水量 (mm) #
# 月份 徐州 滨州 
# ====================== 
1.5 5.5 41.1
2.5 8.1 61.4
3.5 13.2 84.3
4.5 28.4 104.6
5.5 30.9 117.3
6.5 74.1 155.4
7.5 178.3 132.5
8.5 185.9 135.1
9.5 51.3 157.9
10.5 20.8 64.0
11.5 8.1 53.8
12.5 5.5 38.6
$ 
```

这时候，在这两个数据文件所在的目录，执行gnuplot启动gnuplot画图，执行如下命令： <br/>

```text
set xlabel "月份"
set ylabel "降水量(毫米)"
set title "月平均降水量"
set xrange [0:12.5]
set xtics 0,1,12
plot "precipitation1.dat" using 1:2 pointtype 4 pointsize 1 linecolor rgb "red" title "北京", "precipitation1.dat" using 1:3 pointtype 5 pointsize 1 linecolor rgb "green" title "上海",  "precipitation2.dat" using 1:2 pointtype 6 pointsize 1 linecolor rgb "blue" title "徐州", "precipitation2.dat" using 1:3 pointtype 7 pointsize 1 linecolor rgb "yellow" title "滨州"
```

效果如下： <br/>
![](/ox-hugo/03_gnuplotPointGraph.png) <br/>
带连线的代码如下： <br/>

```text
set xlabel "月份"
set ylabel "降水量(毫米)"
set title "月平均降水量"
set xrange [0:12.5]
set xtics 0,1,12
plot "precipitation1.dat" using 1:2 with linespoint pointtype 4 pointsize 1 linecolor rgb "red" title "北京", "precipitation1.dat" using 1:3 with linespoint pointtype 5 pointsize 1 linecolor rgb "green" title "上海",  "precipitation2.dat" using 1:2 with linespoint pointtype 6 pointsize 1 linecolor rgb "blue" title "徐州", "precipitation2.dat" using 1:3 with linespoint pointtype 7 pointsize 1 linecolor rgb "yellow" title "滨州"
```

执行后的效果如下： <br/>
![](/ox-hugo/04_gnuplotPointLineGraph.png) <br/>


### 简单的样式控制 {#简单的样式控制}

相关参数如下： <br/>

-   linestyle 连线风格（包括linetype，linewidth等） <br/>
-   linetype 连线种类 <br/>
-   linewidth 连线粗细 <br/>
-   linecolor 连线颜色 <br/>
    这个参数除了控制连线的颜色外，也控制点的颜色。可以接受指定rgb颜色[^fn:2]。 <br/>
-   pointtype 点的种类 <br/>
-   pointsize 点的大小 <br/>

这些参数可以通过指定一个数字来指定，数字对应的样式，可以通过gnuplot中test命令的输出来查看。如下： <br/>
![](/ox-hugo/01_gnuplotTest.png) <br/>


### 将画图命令写入脚本 {#将画图命令写入脚本}

将前面的画图命令写入脚本，然后，执行load命令，也可以画出效果一样的图。如下： <br/>
![](/ox-hugo/05_gnuplotScript.png) <br/>


## 结合awk命令画图 {#结合awk命令画图}

数据文件如下： <br/>

```text
$ cat rain.dat 
# 数据系杜撰
# 北京各区降水量 (mm/100) #
# 月份 海淀 西城 
# ====================== 
23-06-07 14:54 2.5 38.1
23-06-07 14:55 5.1 58.4
23-06-07 14:56 10.2 81.3
23-06-07 14:57 25.4 101.6
23-06-07 14:58 27.9 114.3
23-06-07 14:59 71.1 152.4
23-06-07 15:00 175.3 129.5
23-06-07 15:01 182.9 132.1
23-06-07 15:02 48.3 154.9
23-06-07 15:03 17.8 61.0
23-06-07 15:04 5.1 50.8
23-06-07 15:05 2.5 35.6
$
```

如果想将这个数据用gnuplot画图的话，需要对横坐标进行处理，处理成整数，这样才可以根据刻度进行画图。这个可以借助awk命令来实现。如下： <br/>

```text
set xlabel "时间"
set ylabel "降水量(毫米/100)"
set title "降水量统计"
set xrange [0:12.5]
set xtics 0,1,12
plot \
"< awk -F ' ' 'function time2int(time_str){return substr(time_str,1,2)*60+substr(time_str,4)-(60*14+53)}!/^#/{print time2int($2), $3, $4}' rain.dat " using 1:2 with linespoint pointtype 4 pointsize 1 linecolor "red" title "海淀", \
"< awk -F ' ' 'function time2int(time_str){return substr(time_str,1,2)*60+substr(time_str,4)-(60*14+53)}!/^#/{print time2int($2), $3, $4}' rain.dat " using 1:3 with linespoint pointtype 5 pointsize 1 linecolor "green" title "西城"
```

画图结果如下： <br/>
![](/ox-hugo/240511_gnuplot_awk01.png) <br/>
为了数据看起来更加直观，可以将横坐标再标记为时间字符串，如下： <br/>

```text
set xlabel "时间"
set ylabel "降水量(毫米/100)"
set title "降水量统计"
set xrange [0:12.5]
# 每组刻度由"显示标签 刻度值 主刻度标记(0否1是,0可以省略)"
set xtics ("" 0 1, "14:54" 1 0, "" 2 1, "14:56" 3 0, "" 4 1, "14:58" 5 0, "" 6 1, "15:00" 7 0, "" 8 1, "15:02" 9 0, "" 10 1, "15:04" 11 0, "" 12 0)
plot \
"< awk -F ' ' 'function time2int(time_str){return substr(time_str,1,2)*60+substr(time_str,4)-(60*14+53)}!/^#/{print time2int($2), $3, $4}' rain.dat " using 1:2 with linespoint pointtype 4 pointsize 1 linecolor "red" title "海淀", \
"< awk -F ' ' 'function time2int(time_str){return substr(time_str,1,2)*60+substr(time_str,4)-(60*14+53)}!/^#/{print time2int($2), $3, $4}' rain.dat " using 1:3 with linespoint pointtype 5 pointsize 1 linecolor "green" title "西城"
```

结果如下： <br/>
![](/ox-hugo/240511_gnuplot_awk02.png) <br/>
这样，有了awk命令，就可以方便的从一个数据文件筛选处理不同的数据，画到多条线上，非常强大。 <br/>

[^fn:1]: 使用gnuplot，科学作图_Gnuplot 中文教程，马欢 <br/>
[^fn:2]: [Change the color of the points in labelled plot](https://stackoverflow.com/questions/25202251/change-the-color-of-the-points-in-labelled-plot)  <br/>