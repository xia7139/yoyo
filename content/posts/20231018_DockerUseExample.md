+++
title = "Docker使用示例"
author = ["Cheng Xia"]
date = 2023-10-18
draft = false
+++

<div class="ox-hugo-toc toc">

<div class="heading">Table of Contents</div>

- [找到镜像](#找到镜像)
- [拉取并运行](#拉取并运行)
- [doker-compose改写启动命令](#doker-compose改写启动命令)

</div>
<!--endtoc-->

本文主要是从一个例子入手，演示docker的方便之处。 <br/>

github上面有好多开源项目，已经有了现成的Docker镜像。这里我们以memos[^fn:1]为例(memos是一个小的笔记系统)进行说明。 <br/>


## 找到镜像 {#找到镜像}

打开memos的主页，如下： <br/>
![](/ox-hugo/01_MemosIndex.png) <br/>
可以看到，有docker镜像的超链接，见上图中的红框。点击该超链接，可以跳转到docker镜像的主页，如下： <br/>
![](/ox-hugo/02_MemosImageIndex1.png) <br/>


## 拉取并运行 {#拉取并运行}

页面底端，可以看到docker操作的命令说明，如下： <br/>
![](/ox-hugo/02_MemosImageIndex2.png) <br/>
首先，复制pull命令， `docker pull neosmemo/memos` ，在命令行(已经安装了docker)执行： <br/>

```text
$ docker pull neosmemo/memos
Using default tag: latest
latest: Pulling from neosmemo/memos
latest: Pulling from neosmemo/memos
96526aa774ef: Pull complete 
3121191514a6: Pull complete 
95389c80abe0: Pull complete 
fee6d326fe80: Pull complete 
fab424b69884: Pull complete 
Digest: sha256:53826afa84e58d8109721eeac78a2c21d9ed4a4110a6e796b1859cdbcb74a42a
Status: Downloaded newer image for neosmemo/memos:latest
docker.io/neosmemo/memos:latest

What's Next?
  View a summary of image vulnerabilities and recommendations → docker scout quickview neosmemo/memos
$ 
```

然后，复制运行的命令，在命令行执行： <br/>

```text
$ ls ~/.memos/
$ docker run -d --name memos -p 5230:5230 -v ~/.memos/:/var/opt/memos neosmemo/memos:latest
892fc5d0dbe8e23596914e12f90fb2dc469215504cfaad1baf82c15fe90370b7
$ ls ~/.memos/
memos_prod.db		memos_prod.db-shm	memos_prod.db-wal
$ 
```

这样，容器就在后台运行了起来(就像页面上说明的一样，如果 `~/.memos/` 目录中没有memos_prod.db文件，会自动创建，系统会启动在<http://localhost:5230>)，在docker管理界面可以看到这个容器： <br/>
![](/ox-hugo/03_MemosContainer.png) <br/>
点击上图中的超链接，就可以打开这个镜像包含的笔记系统： <br/>
![](/ox-hugo/04_MemosLogin.png) <br/>
在前面的这个页面中，可以进行正常的登录和注册，并且可以正常使用这个系统。相关的数据会记录在本地的 `/.memos/` 目录映射到 `/var/opt/memos` 目录中。这样，下次启动镜像时，仍然映射同样的本地目录 `/.memos/` 到 `/var/opt/memos` 目录中，就可以正常访问到之前已经注册的用户以及其中发布的笔记信息。 <br/>

从这里，可以看出，有了docker之后，在搭建这种类似系统时，就不用安装各种依赖、修改各种配置文件了，直接拉取镜像，然后运行就行。 <br/>


## doker-compose改写启动命令 {#doker-compose改写启动命令}

前面的启动命令，有些复杂，这里可以将相关的配置写在docker-compose.yml文件中，然后通过简单的 `docker-compose` 命令启动，如下： <br/>

```text
$ cat .env 
SOFTWARE_VERSION_TAG=latest
$ cat docker-compose.yml 
version: "3.3"
services:
  memos:
    image: "neosmemo/memos:${SOFTWARE_VERSION_TAG}"
    restart: always
    ports:
      - "5230:5230"
    volumes:
      - "./memos/:/var/opt/memos"
$ docker-compose up -d
[+] Building 0.0s (0/0)                                    docker:desktop-linux
[+] Running 2/2
 ✔ Network memos_tmp_default    Created                                    0.1s 
 ✔ Container memos_tmp-memos-1  Started                                    0.1s 
$ 
```

启动之后，从docker管理界面中，可以看到(因为是docker-compose启动的，可以看到有些区别，容器折叠在一个组中)： <br/>
![](/ox-hugo/05_MemosCompose.png) <br/>
点击上图的超链接，效果和前面的启动方式是一样的。 <br/>

[^fn:1]: [memos](https://usememos.com) <br/>