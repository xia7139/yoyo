+++
title = "Docker入门介绍"
author = ["Cheng Xia"]
date = 2023-10-15
draft = false
+++

<div class="ox-hugo-toc toc">

<div class="heading">Table of Contents</div>

- [Docker是什么](#docker是什么)
- [介绍](#介绍)
    - [历史](#历史)
    - [Docker和传统虚拟化的比较](#docker和传统虚拟化的比较)
        - [更高效的利用系统资源](#更高效的利用系统资源)
        - [更快速的启动时间](#更快速的启动时间)
        - [一致的运行环境](#一致的运行环境)
        - [持续交付和部署](#持续交付和部署)
        - [更轻松的迁移](#更轻松的迁移)
        - [更轻松的维护和扩展](#更轻松的维护和扩展)
        - [对比传统虚拟机总结](#对比传统虚拟机总结)
    - [Docker的用途](#docker的用途)
- [基本概念](#基本概念)
    - [镜像（Image）](#镜像-image)
    - [容器（Container）](#容器-container)
    - [仓库（Repository）](#仓库-repository)
- [安装和配置](#安装和配置)
    - [下载安装](#下载安装)
    - [修改源](#修改源)
    - [检查和确认](#检查和确认)
- [简单使用](#简单使用)
    - [运行nginx](#运行nginx)
    - [运行hello world](#运行hello-world)

</div>
<!--endtoc-->

本文主要是"Docker 入门教程"[^fn:1]和"Docker, 从入门到实践"[^fn:2]的整理和学习笔记。 <br/>


## Docker是什么 {#docker是什么}

由于虚拟机存在这些缺点，Linux 发展出了另一种虚拟化技术：Linux 容器（Linux Containers，缩写为 LXC）。 <br/>
Linux 容器不是模拟一个完整的操作系统，而是对进程进行隔离。或者说，在正常进程的外面套了一个保护层。对于容器里面的进程来说，它接触到的各种资源都是虚拟的，从而实现与底层系统的隔离。 <br/>
Docker 属于 Linux 容器的一种封装，提供简单易用的容器使用接口。它是目前最流行的 Linux 容器解决方案。 <br/>
Docker 将应用程序与该程序的依赖，打包在一个文件里面。运行这个文件，就会生成一个虚拟容器。程序在这个虚拟容器里运行，就好像在真实的物理机上运行一样。有了 Docker，就不用担心环境问题。 <br/>
总体来说，Docker 的接口相当简单，用户可以方便地创建和使用容器，把自己的应用放入容器。容器还可以进行版本管理、复制、分享、修改，就像管理普通的代码一样。 <br/>


## 介绍 {#介绍}

参考：[^fn:3] <br/>


### 历史 {#历史}

Docker 最初是 dotCloud 公司创始人 Solomon Hykes 在法国期间发起的一个公司内部项目，它是基于 dotCloud 公司多年云服务技术的一次革新，并于 2013 年 3 月以 Apache 2.0 授权协议开源，主要项目代码在 GitHub 上进行维护。Docker 项目后来还加入了 Linux 基金会，并成立推动 开放容器联盟（OCI）。 <br/>
Docker 自开源后受到广泛的关注和讨论，至今其 GitHub 项目 已经超过 5 万 7 千个星标和一万多个 fork。甚至由于 Docker 项目的火爆，在 2013 年底，dotCloud 公司决定改名为 Docker。Docker 最初是在 Ubuntu 12.04 上开发实现的；Red Hat 则从 RHEL 6.5 开始对 Docker 进行支持；Google 也在其 PaaS 产品中广泛应用 Docker。 <br/>
Docker 使用 Google 公司推出的 Go 语言 进行开发实现，基于 Linux 内核的 cgroup，namespace，以及 OverlayFS 类的 Union FS 等技术，对进程进行封装隔离，属于 操作系统层面的虚拟化技术。由于隔离的进程独立于宿主和其它的隔离的进程，因此也称其为容器。最初实现是基于 LXC，从 0.7 版本以后开始去除 LXC，转而使用自行开发的 libcontainer，从 1.11 版本开始，则进一步演进为使用 runC 和 containerd。 <br/>


### Docker和传统虚拟化的比较 {#docker和传统虚拟化的比较}

传统虚拟化： <br/>
![](/ox-hugo/01_Traditional_Virtualization.png) <br/>
Docker： <br/>
![](/ox-hugo/02_DockerIllustration.png) <br/>
传统虚拟机技术是虚拟出一套硬件后，在其上运行一个完整操作系统，在该系统上再运行所需应用进程；而容器内的应用进程直接运行于宿主的内核，容器内没有自己的内核，而且也没有进行硬件虚拟。因此容器要比传统虚拟机更为轻便。 <br/>


#### 更高效的利用系统资源 {#更高效的利用系统资源}

由于容器不需要进行硬件虚拟以及运行完整操作系统等额外开销，Docker 对系统资源的利用率更高。无论是应用执行速度、内存损耗或者文件存储速度，都要比传统虚拟机技术更高效。因此，相比虚拟机技术，一个相同配置的主机，往往可以运行更多数量的应用。 <br/>


#### 更快速的启动时间 {#更快速的启动时间}

传统的虚拟机技术启动应用服务往往需要数分钟，而 Docker 容器应用，由于直接运行于宿主内核，无需启动完整的操作系统，因此可以做到秒级、甚至毫秒级的启动时间。大大的节约了开发、测试、部署的时间。 <br/>


#### 一致的运行环境 {#一致的运行环境}

开发过程中一个常见的问题是环境一致性问题。由于开发环境、测试环境、生产环境不一致，导致有些 bug 并未在开发过程中被发现。而 Docker 的镜像提供了除内核外完整的运行时环境，确保了应用运行环境一致性，从而不会再出现 「这段代码在我机器上没问题啊」 这类问题。 <br/>


#### 持续交付和部署 {#持续交付和部署}

对开发和运维（DevOps）人员来说，最希望的就是一次创建或配置，可以在任意地方正常运行。 <br/>
使用 Docker 可以通过定制应用镜像来实现持续集成、持续交付、部署。开发人员可以通过 Dockerfile 来进行镜像构建，并结合 持续集成(Continuous Integration) 系统进行集成测试，而运维人员则可以直接在生产环境中快速部署该镜像，甚至结合 持续部署(Continuous Delivery/Deployment) 系统进行自动部署。 <br/>
而且使用 Dockerfile 使镜像构建透明化，不仅仅开发团队可以理解应用运行环境，也方便运维团队理解应用运行所需条件，帮助更好的生产环境中部署该镜像。 <br/>


#### 更轻松的迁移 {#更轻松的迁移}

由于 Docker 确保了执行环境的一致性，使得应用的迁移更加容易。Docker 可以在很多平台上运行，无论是物理机、虚拟机、公有云、私有云，甚至是笔记本，其运行结果是一致的。因此用户可以很轻易的将在一个平台上运行的应用，迁移到另一个平台上，而不用担心运行环境的变化导致应用无法正常运行的情况。 <br/>


#### 更轻松的维护和扩展 {#更轻松的维护和扩展}

Docker 使用的分层存储以及镜像的技术，使得应用重复部分的复用更为容易，也使得应用的维护更新更加简单，基于基础镜像进一步扩展镜像也变得非常简单。此外，Docker 团队同各个开源项目团队一起维护了一大批高质量的 官方镜像，既可以直接在生产环境使用，又可以作为基础进一步定制，大大的降低了应用服务的镜像制作成本。 <br/>


#### 对比传统虚拟机总结 {#对比传统虚拟机总结}

{{< figure src="/ox-hugo/03_DockerVsTraditionalVisualization.png" >}} <br/>


### Docker的用途 {#docker的用途}

Docker 的主要用途，目前有三大类。 <br/>

-   提供一次性的环境。比如，本地测试他人的软件、持续集成的时候提供单元测试和构建的环境。 <br/>
-   提供弹性的云服务。因为 Docker 容器可以随开随关，很适合动态扩容和缩容。 <br/>
-   组建微服务架构。通过多个容器，一台机器可以跑多个服务，因此在本机就可以模拟出微服务架构。 <br/>


## 基本概念 {#基本概念}


### 镜像（Image） {#镜像-image}

操作系统分为 内核 和 用户空间。对于 Linux 而言，内核启动后，会挂载 root 文件系统为其提供用户空间支持。而 Docker 镜像（Image），就相当于是一个 root 文件系统。比如官方镜像 ubuntu:18.04 就包含了完整的一套 Ubuntu 18.04 最小系统的 root 文件系统。 <br/>
Docker 镜像 是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。镜像 不包含 任何动态数据，其内容在构建之后也不会被改变。 <br/>
简单地说，Docker镜像就是一个Linux的文件系统(Root FileSystem)，这个文件系统里面包含运行在Linux内核的程序，以及相应的数据。镜像可以用来创建Docker容器，一个镜像可以创建多个容器。 <br/>


### 容器（Container） {#容器-container}

镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的 类 和 实例 一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。 <br/>
容器的实质是进程，但与直接在宿主执行的进程不同，容器进程运行于属于自己的独立的 命名空间。因此容器可以拥有自己的 root 文件系统、自己的网络配置、自己的进程空间，甚至自己的用户 ID 空间。容器内的进程是运行在一个隔离的环境里，使用起来，就好像是在一个独立于宿主的系统下操作一样。这种特性使得容器封装的应用比直接在宿主运行更加安全。也因为这种隔离的特性，很多人初学 Docker 时常常会混淆容器和虚拟机。 <br/>
前面讲过镜像使用的是分层存储，容器也是如此。每一个容器运行时，是以镜像为基础层，在其上创建一个当前容器的存储层，我们可以称这个为容器运行时读写而准备的存储层为 容器存储层。 <br/>
容器存储层的生存周期和容器一样，容器消亡时，容器存储层也随之消亡。因此，任何保存于容器存储层的信息都会随容器删除而丢失。 <br/>
按照 Docker 最佳实践的要求，容器不应该向其存储层内写入任何数据，容器存储层要保持无状态化。所有的文件写入操作，都应该使用 数据卷（Volume）、或者 绑定宿主目录，在这些位置的读写会跳过容器存储层，直接对宿主（或网络存储）发生读写，其性能和稳定性更高。 <br/>
数据卷的生存周期独立于容器，容器消亡，数据卷不会消亡。因此，使用数据卷后，容器删除或者重新运行之后，数据却不会丢失。 <br/>


### 仓库（Repository） {#仓库-repository}

镜像构建完成后，可以很容易的在当前宿主机上运行，但是，如果需要在其它服务器上使用这个镜像，我们就需要一个集中的存储、分发镜像的服务，Docker Registry 就是这样的服务。 <br/>
一个 Docker Registry 中可以包含多个 仓库（Repository）；每个仓库可以包含多个 标签（Tag）；每个标签对应一个镜像。 <br/>
通常，一个仓库会包含同一个软件不同版本的镜像，而标签就常用于对应该软件的各个版本。我们可以通过 &lt;仓库名&gt;:&lt;标签&gt; 的格式来指定具体是这个软件哪个版本的镜像。如果不给出标签，将以 latest 作为默认标签。 <br/>
以 Ubuntu 镜像 为例，ubuntu 是仓库的名字，其内包含有不同的版本标签，如，16.04, 18.04。我们可以通过 ubuntu:16.04，或者 ubuntu:18.04 来具体指定所需哪个版本的镜像。如果忽略了标签，比如 ubuntu，那将视为 ubuntu:latest。 <br/>
仓库名经常以 两段式路径 形式出现，比如 jwilder/nginx-proxy，前者往往意味着 Docker Registry 多用户环境下的用户名，后者则往往是对应的软件名。但这并非绝对，取决于所使用的具体 Docker Registry 的软件或服务。 <br/>


## 安装和配置 {#安装和配置}


### 下载安装 {#下载安装}

这里系统系统版本系macOS Monterey 12.6.3。 <br/>
从官网下载安装包后，可以用一般macOS软件安装的方式(双击之后，将应用图标拖动到application)进行安装。 <br/>
启动后，效果如下： <br/>
![](/ox-hugo/04_DockerStartup.png) <br/>
同时，右上角菜单栏看到多了一个鲸鱼图标，这个图标表明了 Docker 的运行状态。 <br/>


### 修改源 {#修改源}

对于macOS，在任务栏点击 Docker Desktop 应用图标 -&gt; Settings...，在左侧导航菜单选择 Docker Engine，在右侧像下边一样编辑 json 文件。修改完成之后，点击 Apply &amp; restart 按钮，Docker 就会重启并应用配置的镜像地址了。 <br/>
这里添加两个国内的源： <br/>

-   "<https://hub-mirror.c.163.com>" <br/>
-   "<https://mirror.baidubce.com>" <br/>

添加完成之后，效果如下： <br/>
![](/ox-hugo/05_DockerCompose.png) <br/>


### 检查和确认 {#检查和确认}

Docker启动之后，执行 `docker --version` 命令和 `docker info` 命令，可以检查docker是否正常如下： <br/>

```text
$ docker --version
Docker version 24.0.6, build ed223bc
$
```

执行 `docker info` 命令后，可以看到输出中有如下内容： <br/>

```text
Registry Mirrors:
 https://hub-mirror.c.163.com/
 https://mirror.baidubce.com/
Live Restore Enabled: false
```

说明修改的源已经生效。 <br/>


## 简单使用 {#简单使用}


### 运行nginx {#运行nginx}

用如下命令，启动一个名为webserver的nginx镜像容器： <br/>

```text
$ docker run -d -p 8080:80 --name webserver nginx
ccbd4db9158e2efd4cd6e37976f73ad0b3fbc0aee5e014e7bfcfc93e655e6bc0
```

这里-p参数的意思是，将本地的8080端口，映射到容器内的80端口。(注：第一次执行该命令时，需要先从镜像仓库拉取镜像，在命令行输出中可以看到相关日志) <br/>
启动成功后，可以在docker软件中，看到这个容器，如下： <br/>
![](/ox-hugo/05_DockerNginxStart_0.png) <br/>
这时，访问 `http://localhost:8080` ，就可以看到nginx启动成功的页面。如下： <br/>
![](/ox-hugo/05_DockerNginxStart.png) <br/>
执行如下的stop和rm命令，可以停止和删除镜像容器。 <br/>

```text
$ docker stop webserver
webserver
$ docker rm webserver
webserver
```

命令执行之后，docker软件中，就可以看到之前的webserver容器已经没有了。如下： <br/>
![](/ox-hugo/05_DockerNginxStopRm.png) <br/>


### 运行hello world {#运行hello-world}

如下： <br/>

```text
$ docker image pull library/hello-world
Using default tag: latest
latest: Pulling from library/hello-world
719385e32844: Pull complete 
Digest: sha256:88ec0acaa3ec199d3b7eaf73588f4518c25f9d34f58ce9a0df68429c5af48e8d
Status: Downloaded newer image for hello-world:latest
docker.io/library/hello-world:latest

$ docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

$
```

这里，是先执行了拉取镜像的操作，然后再运行容器。和第一个不一样的是，这个运行输出提示以后，hello world就会停止运行，容器自动终止。而前一个例子中，容器不会自动终止，因为提供的是服务。 <br/>
容器已经停止运行，删除的命令如下： <br/>

```text
$ docker image ls
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
nginx         latest    bc649bab30d1   3 days ago     187MB
hello-world   latest    9c7a54a9a43c   5 months ago   13.3kB
# 列出所有容器
$ docker container ls
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
# 列出所有容器，包括已经停止运行的容器
$ docker container ls --all
CONTAINER ID   IMAGE         COMMAND    CREATED         STATUS                     PORTS     NAMES
f2456f25b356   hello-world   "/hello"   7 minutes ago   Exited (0) 7 minutes ago             optimistic_kare
8a75c44a1d6b   hello-world   "/hello"   8 minutes ago   Exited (0) 8 minutes ago             infallible_saha
# 删除容器
$ docker container rm f2456f25b356
f2456f25b356
$ docker container rm 8a75c44a1d6b
8a75c44a1d6b
# 列出所有容器，包括已经停止运行的容器
$ docker container ls --all
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
$
```

参考[^fn:1] <br/>

[^fn:1]: [Docker 入门教程](http://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html) <br/>
[^fn:2]: [Docker, 从入门到实践](https://yeasy.gitbook.io/docker_practice/)  <br/>
[^fn:3]: [什么是 Docker](https://yeasy.gitbook.io/docker_practice/introduction/what)  <br/>