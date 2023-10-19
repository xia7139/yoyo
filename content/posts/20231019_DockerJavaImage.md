+++
title = "Docker打包java程序"
author = ["Cheng Xia"]
draft = false
+++

<div class="ox-hugo-toc toc">

<div class="heading">Table of Contents</div>

- [基于OpenJDK的镜像进行打包](#基于openjdk的镜像进行打包)
- [分阶段打包](#分阶段打包)
    - [分阶段打包说明](#分阶段打包说明)
    - [分阶段打包示例说明](#分阶段打包示例说明)

</div>
<!--endtoc-->

本文主要介绍如何把Java程序打包到Docker中，内容主要参考[^fn:1]。 <br/>


## 基于OpenJDK的镜像进行打包 {#基于openjdk的镜像进行打包}

在一个空的目录中，创建要打包的java文件，写入测试用的java代码，见Hello.java： <br/>

```java
public class Hello{
    public static void main(String[] args){
	String os = System.getProperty("os.name");
	String osArch = System.getProperty("os.arch");
	String osVersion = System.getProperty("os.version");

	System.out.println(os);
	System.out.println(osArch);
	System.out.println(osVersion);
    }
}
```

然后，在这个目录中新建一个文本文件名为"Dockerfile"，内容如下： <br/>

```text
## 设置基础镜像
FROM openjdk:8

## 设置进入容器时的工作目录
WORKDIR /root/app
## 将本地目录复制进容器目录中
COPY Hello.java /root/app

## 镜像制作时执行的命令
RUN javac Hello.java

## 容器启动时执行的命令
ENTRYPOINT java Hello
```

效果如下： <br/>

```text
$ ls
Dockerfile	Hello.java
$ cat Hello.java 
public class Hello{
    public static void main(String[] args){
	String os = System.getProperty("os.name");
	String osArch = System.getProperty("os.arch");
	String osVersion = System.getProperty("os.version");

	System.out.println(os);
	System.out.println(osArch);
	System.out.println(osVersion);
    }
}
$ cat Dockerfile 
## 设置基础镜像
FROM openjdk:8

## 设置进入容器时的工作目录
WORKDIR /root/app
## 将本地目录复制进容器目录中
COPY Hello.java /root/app

## 镜像制作时执行的命令
RUN javac Hello.java

## 容器启动时执行的命令
ENTRYPOINT java Hello
$
```

接下来，执行打包操作： <br/>

```text
# 查看当前已有的镜像列表
$ docker images
REPOSITORY       TAG       IMAGE ID       CREATED        SIZE
nginx            latest    bc649bab30d1   7 days ago     187MB
neosmemo/memos   latest    4f4d8f305b1f   13 days ago    69.2MB
hello-world      latest    9c7a54a9a43c   5 months ago   13.3kB

# 执行打包操作，默认读取当前目录的Dockerfile文件进行打包
$ docker build .
[+] Building 2.4s (9/9) FINISHED                                                             docker:desktop-linux
 => [internal] load .dockerignore                                                                            0.0s
 => => transferring context: 2B                                                                              0.0s
 => [internal] load build definition from Dockerfile                                                         0.0s
 => => transferring dockerfile: 317B                                                                         0.0s
 => [internal] load metadata for docker.io/library/openjdk:8                                                 1.3s
 => [1/4] FROM docker.io/library/openjdk:8@sha256:86e863cc57215cfb181bd319736d0baf625fe8f150577f9eb58bd937f  0.0s
 => [internal] load build context                                                                            0.0s
 => => transferring context: 430B                                                                            0.0s
 => CACHED [2/4] WORKDIR /root/app                                                                           0.0s
 => [3/4] COPY Hello.java /root/app                                                                          0.0s
 => [4/4] RUN javac Hello.java                                                                               0.9s
 => exporting to image                                                                                       0.1s
 => => exporting layers                                                                                      0.1s
 => => writing image sha256:2ca7fc1fc1aaac4fa65c8223f7d6ce39888d6090cf5d070c3db94f0d8f1b1c03                 0.0s

What's Next?
  View a summary of image vulnerabilities and recommendations → docker scout quickview

# 查看当前的镜像列表，可以看到有一个刚打好的镜像
$ docker images
REPOSITORY       TAG       IMAGE ID       CREATED         SIZE
<none>           <none>    2ca7fc1fc1aa   6 seconds ago   526MB
nginx            latest    bc649bab30d1   7 days ago      187MB
neosmemo/memos   latest    4f4d8f305b1f   13 days ago     69.2MB
hello-world      latest    9c7a54a9a43c   5 months ago    13.3kB

# 运行这个镜像
$ docker run 2ca7
Hello World!
Linux
amd64
6.4.16-linuxkit
$
```

前面可以看到，打包的镜像没有指定Repository和Tag，通过如下命令可以指定： <br/>

```text
$ docker tag 2ca7 docker/hello:1.0
$ docker images
REPOSITORY       TAG       IMAGE ID       CREATED          SIZE
docker/hello     1.0       2ca7fc1fc1aa   10 minutes ago   526MB
nginx            latest    bc649bab30d1   7 days ago       187MB
neosmemo/memos   latest    4f4d8f305b1f   13 days ago      69.2MB
hello-world      latest    9c7a54a9a43c   5 months ago     13.3kB
$ 
```

可以看到ID以2ca7开头的镜像已经有了Repository和Tag。其实，也可以在打包的时候，直接通过参数指定这两个字段，如下： <br/>

```text
docker build . -t docker/hello:1.0
```

注意，这里采用的基础镜像是alpine。说明如下[^fn:2]： <br/>
Alpine 的意思是“高山的”，比如 Alpine plants高山植物，Alpine skiing高山滑雪、the alpine resort阿尔卑斯山胜地。alpine系统特点： <br/>

-   小巧：基于Musl libc和busybox，和busybox一样小巧，最小的Docker镜像只有5MB； <br/>
-   安全：面向安全的轻量发行版； <br/>
-   简单：提供APK包管理工具，软件的搜索、安装、删除、升级都非常方便。 <br/>
-   适合容器使用：由于小巧、功能完备，非常适合作为容器的基础镜像。 <br/>


## 分阶段打包 {#分阶段打包}

前面可以看到，打包出来的镜像比较大。这是因为打包的基础镜像是OpenJDK，其中包含了Java编译和运行的全部套件。实际上，我们这个镜像只是为了运行java程序，没必要把编译相关的内容也一并打进镜像包里面。 <br/>

如下介绍分阶段打包的方式，可以基于一个基础镜像编译，然后将编译的结果打入另一个基础镜像。 <br/>


### 分阶段打包说明 {#分阶段打包说明}

参考[^fn:3] <br/>
从docker17.05版本开始，新增了Dockerfile多阶段(multistage)构建,dockerfile中允许使用多个FROM指令。 <br/>
多个 FROM 指令并不是为了生成多根的层关系，最后生成的镜像仍以最后一条 FROM 为准，之前的 FROM 会被抛弃，那么之前的FROM 又有什么意义呢？ <br/>
每一条 FROM 指令都是一个构建阶段，多条 FROM 就是多阶段构建，虽然最后生成的镜像只能是最后一个阶段的结果，但是，能够将前置阶段中的文件拷贝到后边的阶段中，这就是多阶段构建的最大意义。 <br/>
该特性最大的使用场景是可以使编译环境和发布环境分离。 <br/>


### 分阶段打包示例说明 {#分阶段打包示例说明}

在一个空目录中，建立如下的目录结构： <br/>

```text
$ tree .
.
├── Dockerfile
├── pom.xml
└── src
    ├── main
    │   ├── java
    │   │   └── Main.java
    │   └── resources
    └── test
	└── java

6 directories, 3 files
$
```

pom.xml的内容如下： <br/>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
	 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.test</groupId>
    <artifactId>SimpleMaven</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
	<maven.compiler.source>8</maven.compiler.source>
	<maven.compiler.target>8</maven.compiler.target>
	<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>
    <build>
	<pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
	    <plugins>
		<plugin>
		    <artifactId>maven-jar-plugin</artifactId>
		    <version>3.0.2</version>
		    <configuration>
			<archive>
			    <!-- 指定mainClass,不指定可能导致jar包运行不成功 -->
			    <manifest>
				<addClasspath>true</addClasspath>
				<useUniqueVersions>false</useUniqueVersions>
				<classpathPrefix>lib/</classpathPrefix>
				<mainClass>Main</mainClass>
			    </manifest>
			</archive>
		    </configuration>
		</plugin>
	    </plugins>
	</pluginManagement>
    </build>
</project>
```

Main.java的内容如下： <br/>

```java
public class Main {
    public static void main(String[] args) {
	System.out.println("Hello World!");
    }
}
```

执行打包，如下： <br/>

```text
$ docker build . -t docker/hello2:v1.0
[+] Building 660.1s (14/14) FINISHED                                                         docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                         0.1s
 => => transferring dockerfile: 560B                                                                         0.0s
 => [internal] load .dockerignore                                                                            0.1s
 => => transferring context: 2B                                                                              0.0s
 => [internal] load metadata for docker.io/library/maven:amazoncorretto                                      2.7s
 => [internal] load metadata for docker.io/library/openjdk:8-jre-alpine                                      3.4s
 => [stage-1 1/3] FROM docker.io/library/openjdk:8-jre-alpine@sha256:f362b165b870ef129cbe730f29065ff37399c0  0.0s
 => [maven_build 1/5] FROM docker.io/library/maven:amazoncorretto@sha256:dfa1e28cdc8b3049aef83f352e57030c02  0.0s
 => [internal] load build context                                                                            0.0s
 => => transferring context: 568B                                                                            0.0s
 => CACHED [maven_build 2/5] WORKDIR /build/                                                                 0.0s
 => CACHED [maven_build 3/5] COPY pom.xml /build/                                                            0.0s
 => [maven_build 4/5] COPY src /build/src/                                                                   0.0s
 => [maven_build 5/5] RUN mvn package                                                                      656.3s
 => CACHED [stage-1 2/3] WORKDIR /app                                                                        0.0s
 => [stage-1 3/3] COPY --from=MAVEN_BUILD /build/target/SimpleMaven-1.0-SNAPSHOT.jar /app/                   0.0s
 => exporting to image                                                                                       0.0s 
 => => exporting layers                                                                                      0.0s 
 => => writing image sha256:78d529265c283f54b377027c3c246f98017dd3d4025d84e21bc9d6368eab37cf                 0.0s 
 => => naming to docker.io/docker/hello2:v1.0                                                                0.0s 

What's Next?
  View a summary of image vulnerabilities and recommendations → docker scout quickview
$ docker images
REPOSITORY       TAG       IMAGE ID       CREATED          SIZE
docker/hello2    v1.0      78d529265c28   12 seconds ago   84.9MB
docker/hello     1.0       2ca7fc1fc1aa   47 minutes ago   526MB
nginx            latest    bc649bab30d1   7 days ago       187MB
neosmemo/memos   latest    4f4d8f305b1f   13 days ago      69.2MB
hello-world      latest    9c7a54a9a43c   5 months ago     13.3kB
$ 
```

可以看到有一个新打包好的镜像，这次的体积小了很多。运行这个镜像可以正常看到输出，如下： <br/>

```text
$ docker run docker/hello2:v1.0
Hello World!
$
```

[^fn:1]: [Docker极简入门：使用Docker运行Java程序](https://www.cnblogs.com/AD-milk/p/15579530.html)  <br/>
[^fn:2]: [docker使用alpine镜像](https://blog.csdn.net/ichen820/article/details/117879520)  <br/>
[^fn:3]: [Dockerfile多个from的使用及多个build-arg的使用示例](https://blog.csdn.net/quyingzhe0217/article/details/129319294)  <br/>