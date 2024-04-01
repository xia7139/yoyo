+++
title = "Spring介绍和入门"
author = ["Cheng Xia"]
date = 2024-03-26
draft = false
+++

<div class="ox-hugo-toc toc">

<div class="heading">Table of Contents</div>

- [SpringBoot介绍](#springboot介绍)
    - [Spring和Spring Boot对比](#spring和spring-boot对比)
    - [Spring Boot与Spring MVC](#spring-boot与spring-mvc)
    - [Spring Boot体系结构](#spring-boot体系结构)
- [Spring IoC容器](#spring-ioc容器)
- [Spring中的Bean装配](#spring中的bean装配)
    - [说明](#说明)
    - [xml方式装配bean](#xml方式装配bean)
    - [java方式装配bean](#java方式装配bean)
    - [混合xml和java方式装配bean](#混合xml和java方式装配bean)
    - [自动装配](#自动装配)
- [Spring装配进阶](#spring装配进阶)
    - [说明](#说明)
    - [设置bean的作用域](#设置bean的作用域)
        - [Spring中的Bean作用域说明](#spring中的bean作用域说明)
        - [Singleton和Prototype作用域的设置示例](#singleton和prototype作用域的设置示例)
    - [按条件装配](#按条件装配)
    - [注入外部配置文件配置项的值](#注入外部配置文件配置项的值)
    - [通过属性占位符注入配置文件配置项的值](#通过属性占位符注入配置文件配置项的值)
    - [使SpEL进行装配](#使spel进行装配)
- [Spring切面](#spring切面)
    - [说明](#说明)
    - [AOP术语](#aop术语)
    - [切面简单示例](#切面简单示例)
    - [传入参数的切面](#传入参数的切面)
    - [通过切面引入新功能](#通过切面引入新功能)

</div>
<!--endtoc-->



## SpringBoot介绍 {#springboot介绍}

Spring Boot是用于创建微服务的基于Java的开源框架。它是由Pivotal Team开发的，用于构建独立的和生产就绪的弹簧应用程序。 <br/>
参考[^fn:1]。 <br/>


### Spring和Spring Boot对比 {#spring和spring-boot对比}

Spring: Spring框架是最流行的Java应用程序开发框架。 Spring框架的主要功能是依赖注入或控制反转(IoC)。借助Spring Framework，我们可以开发一个松耦合的应用程序。如果纯粹定义应用程序类型或特征，最好使用。 <br/>
SpringBoot: Spring Boot是Spring Framework的模块。它允许我们构建具有最少配置或零配置的独立应用程序。如果我们要开发一个简单的基于Spring的应用程序或RESTful服务，最好使用它。 <br/>
Spring和Spring Boot之间的主要比较讨论如下： <br/>

| Spring                                    | Spring Boot                                                           |
|-------------------------------------------|-----------------------------------------------------------------------|
| Spring Framework是用于构建应用程序的广泛使用的Java EE框架。 | SpringBoot Framework 被广泛用于开发 REST API 。                       |
| 它旨在简化Java EE开发，从而使开发人员更加高效。 | 它旨在缩短代码长度，并提供开发 Web应用程序的最简单方法。              |
| Spring Framework的主要功能是依赖注入。    | Spring Boot的主要功能是自动配置。它会根据需求自动配置类。             |
| 通过允许我们开发松耦合应用程序，可以使事情变得更简单。 | 它有助于创建配置更少的独立应用。                                      |
| 开发人员编写了大量代码(模板代码)来完成最小的任务。 | 它减少样板代码。                                                      |
| 要测试Spring项目，我们需要显式设置服务器。 | SpringBoot提供了嵌入式服务器，例如 Jetty 和 Tomcat 等。               |
| 它不支持内存数据库。                      | 它提供了多个插件来处理嵌入式和内存中数据库，例如 H2 。                |
| 开发人员在 pom.xml 中为Spring项目手动定义依赖项。 | Spring Boot在pom.xml文件中带有 starter 概念，该概念内部负责根据Spring Boot要求下载依赖项 JAR 。 |


### Spring Boot与Spring MVC {#spring-boot与spring-mvc}

Spring Boot: SpringBoot使得快速引导和开始开发基于Spring的过程变得容易。应用。它避免了很多样板代码。它隐藏了很多复杂的信息，因此开发人员可以快速入门并轻松开发基于Spring的应用程序。 <br/>
Spring MVC: Spring MVC是用于以下方面的Web MVC框架: 构建Web应用程序。它包含许多用于各种功能的配置文件。这是一个面向HTTP的Web应用程序开发框架。 <br/>
Spring Boot和Spring MVC出于不同的目的而存在。下面讨论了Spring Boot和Spring MVC之间的主要比较： <br/>

| SpringBoot                                       | SpringMVC                            |
|--------------------------------------------------|--------------------------------------|
| SpringBoot 是Spring的模块，用于使用合理的默认值打包基于Spring的应用程序。 | SpringMVC 是Spring框架下基于模型视图控制器的Web框架。 |
| 它提供了用于构建 Spring-powered 框架的默认配置。 | 它提供了易于使用功能来构建Web应用程序。 |
| 无需手动构建配置。                               | 它需要手动进行构建配置。             |
| 不需要部署描述符。                               | 必需。                               |
| 它避免了样板代码，并将依赖项包装在一个单元中。   | 它分别指定每个依赖项。               |
| 它减少开发时间并提高生产率。                     | 要花费相同的时间，要花费更多。       |


### Spring Boot体系结构 {#spring-boot体系结构}

Spring Boot体系结构 <br/>
SpringBoot是Spring框架的模块。它用于轻松创建独立的生产级基于Spring的应用程序。它是在核心Spring框架的顶部开发的。 <br/>

SpringBoot遵循一个分层的体系结构，其中每一层都与它的直接下层或上层(层次结构)进行通信。 <br/>

之前了解 SpringBoot Architecture 后，我们必须了解其中的不同层和类。 SpringBoot中有四个层，如下所示： <br/>

{{< figure src="/ox-hugo/240326_Spring_Architecture.png" >}} <br/>

-   展示层 <br/>
    表示层负责处理HTTP请求，将JSON参数转换为对象，并对请求进行身份验证并将其传输到业务层。简而言之，它由视图即前端部分组成。 <br/>
-   业务层 <br/>
    业务层处理所有业务逻辑。它由服务类组成，并使用数据访问层提供的服务。它还执行授权和验证。 <br/>
-   持久层 <br/>
    持久层包含所有存储逻辑，并将业务对象与数据库行进行相互转换。 <br/>
-   数据库层 <br/>
    在数据库层中， CRUD(创建，检索，更新，删除)操作。 <br/>


## Spring IoC容器 {#spring-ioc容器}

本章节参考[^fn:2]。 <br/>
Spring容器是Spring框架的核心。容器将创建对象，把它们连接在一起，配置它们，并管理他们的整个生命周期从创建到销毁。Spring容器使用依赖注入（DI）来管理组成一个应用程序的组件。这些对象被称为Spring Beans。 <br/>
Spring 容器是 Spring 框架的核心。容器将创建对象，把它们连接在一起，配置它们，并管理他们的整个生命周期从创建到销毁。Spring 容器使用依赖注入（DI）来管理组成一个应用程序的组件。这些对象被称为 Spring Beans，我们将在下一章中进行讨论。 <br/>
通过阅读配置元数据提供的指令，容器知道对哪些对象进行实例化，配置和组装。配置元数据可以通过 XML，Java 注释或 Java 代码来表示。下图是 Spring 如何工作的高级视图。 Spring IoC 容器利用 Java 的 POJO 类和配置元数据来生成完全配置和可执行的系统或应用程序。 <br/>

{{< figure src="/ox-hugo/240326_Spring_IOC_Container.jpeg" >}} <br/>

IOC 容器具有依赖注入功能的容器，它可以创建对象，IOC 容器负责实例化、定位、配置应用程序中的对象及建立这些对象间的依赖。通常new一个实例，控制权由程序员控制，而"控制反转"是指new实例工作不由程序员来做而是交给Spring容器来做。 <br/>
Spring 提供了以下两种不同类型的容器： <br/>

-   Spring BeanFactory容器 <br/>
    它是最简单的容器，给 DI 提供了基本的支持，它用 org.springframework.beans.factory.BeanFactory 接口来定义。BeanFactory 或者相关的接口，如 BeanFactoryAware，InitializingBean，DisposableBean，在 Spring 中仍然存在具有大量的与 Spring 整合的第三方框架的反向兼容性的目的。 <br/>
-   Spring ApplicationContext容器 <br/>
    该容器添加了更多的企业特定的功能，例如从一个属性文件中解析文本信息的能力，发布应用程序事件给感兴趣的事件监听器的能力。该容器是由 org.springframework.context.ApplicationContext 接口定义。 <br/>

ApplicationContext 容器包括 BeanFactory 容器的所有功能，所以通常不建议使用BeanFactory。BeanFactory 仍然可以用于轻量级的应用程序，如移动设备或基于 applet 的应用程序，其中它的数据量和速度是显著。 <br/>


## Spring中的Bean装配 {#spring中的bean装配}


### 说明 {#说明}

本章节中代码参考[^fn:3]，项目的完整目录结构如下： <br/>

```text
spring-bean $ tree .
.
├── pom.xml
└── src
    ├── main
    │   ├── java
    │   │   └── com
    │   │       └── lfqy
    │   │           └── trial
    │   │               └── spring
    │   │                   └── bean
    │   │                       ├── autoconfig
    │   │                       │   ├── AutoConfigByJavaMain.java
    │   │                       │   ├── AutoConfigByXmlMain.java
    │   │                       │   ├── ComponentScanPoint.java
    │   │                       │   ├── config
    │   │                       │   │   └── CDPlayerConfig.java
    │   │                       │   ├── impl
    │   │                       │   │   ├── CDPlayer.java
    │   │                       │   │   └── SgtPeppers.java
    │   │                       │   └── intrfc
    │   │                       │       ├── CompactDisc.java
    │   │                       │       └── MediaPlayer.java
    │   │                       ├── javaconfig
    │   │                       │   ├── JavaConfigMain.java
    │   │                       │   ├── config
    │   │                       │   │   └── CDPlayerConfig.java
    │   │                       │   ├── impl
    │   │                       │   │   ├── CDPlayer.java
    │   │                       │   │   └── SgtPeppers.java
    │   │                       │   └── intrfc
    │   │                       │       ├── CompactDisc.java
    │   │                       │       └── MediaPlayer.java
    │   │                       ├── mixedconfig
    │   │                       │   ├── JavaMixed2XmlConfigMain.java
    │   │                       │   ├── XmlMixed2JavaConfigMain.java
    │   │                       │   ├── config
    │   │                       │   │   ├── java2xml
    │   │                       │   │   │   └── CDConfig.java
    │   │                       │   │   └── xml2java
    │   │                       │   │       └── CDPlayerConfig.java
    │   │                       │   ├── impl
    │   │                       │   │   ├── BlankDisc.java
    │   │                       │   │   ├── CDPlayer.java
    │   │                       │   │   └── SgtPeppers.java
    │   │                       │   └── intrfc
    │   │                       │       ├── CompactDisc.java
    │   │                       │       └── MediaPlayer.java
    │   │                       └── xmlconfig
    │   │                           ├── XmlConfigMain.java
    │   │                           ├── impl
    │   │                           │   ├── CDPlayer.java
    │   │                           │   └── SgtPeppers.java
    │   │                           └── intrfc
    │   │                               ├── CompactDisc.java
    │   │                               └── MediaPlayer.java
    │   └── resources
    │       └── META-INF
    │           └── spring
    │               ├── autoconfig
    │               │   └── autoconfig.xml
    │               ├── mixedconfig
    │               │   ├── java2xml
    │               │   │   └── mixedconfig-cdplayer.xml
    │               │   └── xml2java
    │               │       └── mixedconfig-cd.xml
    │               └── xmlconfig
    │                   └── xmlconfig.xml
    └── test
	└── java
	    └── com
		└── lfqy
		    └── trial
			└── spring
			    └── bean
				└── mixedconfig
				    └── Java2XmlConfigTest.java

42 directories, 34 files
ChengdeMac-mini:spring-bean $
```

项目的pom文件，见pom.xml： <br/>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
	 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.lfqy.trial</groupId>
    <artifactId>spring-bean</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
	<maven.compiler.source>8</maven.compiler.source>
	<maven.compiler.target>8</maven.compiler.target>
	<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
<!--        <spring.version>4.0.9.RELEASE</spring.version>-->
	<spring.version>4.1.4.RELEASE</spring.version>
	<aspectj.version>1.7.2</aspectj.version>
    </properties>
    <dependencies>
	<!-- Spring Core -->
	<!-- http://mvnrepository.com/artifact/org.springframework/spring-core -->
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-core</artifactId>
	    <version>${spring.version}</version>
	</dependency>
	<!-- Spring Context -->
	<!-- http://mvnrepository.com/artifact/org.springframework/spring-context -->
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-context</artifactId>
	    <version>${spring.version}</version>
	</dependency>


	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-test</artifactId>
	    <version>${spring.version}</version>
	    <scope>test</scope>
	</dependency>

	<dependency>
	    <groupId>org.aspectj</groupId>
	    <artifactId>aspectjweaver</artifactId>
	    <version>${aspectj.version}</version>
	</dependency>

	<dependency>
	    <groupId>junit</groupId>
	    <artifactId>junit</artifactId>
	    <version>4.13.1</version>
	    <scope>test</scope>
	</dependency>

	<dependency>
	    <groupId>org.mockito</groupId>
	    <artifactId>mockito-core</artifactId>
	    <version>2.23.4</version>
	    <scope>test</scope>
	</dependency>

	<dependency>
	    <groupId>org.apache.commons</groupId>
	    <artifactId>commons-lang3</artifactId>
	    <version>3.1</version>
	</dependency>
    </dependencies>
</project>
```


### xml方式装配bean {#xml方式装配bean}

-   两个接口类 <br/>
    -   Compactdisc类 <br/>
        com.lfqy.trial.spring.bean.xmlconfig.intrfc.CompactDisc： <br/>
        ```java
        package com.lfqy.trial.spring.bean.xmlconfig.intrfc;
        
        public interface CompactDisc {
            void play();
        }
        ```
    -   MediaPlayer类 <br/>
        com.lfqy.trial.spring.bean.xmlconfig.intrfc.MediaPlayer： <br/>
        ```java
        package com.lfqy.trial.spring.bean.xmlconfig.intrfc;
        
        public interface MediaPlayer {
        
        	void play();
        
        }
        ```
-   两个对应的实现类 <br/>
    -   SgtPepper类 <br/>
        com.lfqy.trial.spring.bean.xmlconfig.impl.SgtPeppers： <br/>
        ```java
        package com.lfqy.trial.spring.bean.xmlconfig.impl;
        
        import com.lfqy.trial.spring.bean.xmlconfig.intrfc.CompactDisc;
        
        public class SgtPeppers implements CompactDisc {
        
        	//英国摇滚乐队The Beatles的第八张录音室专辑
        	private String title = "Sgt. Pepper's Lonely Hearts Club Band";
        	private String artist = "The Beatles";
        
        	public void play() {
        		System.out.println("Playing " + title + " by " + artist);
        	}
        
        }
        ```
    -   CDPlayer类 <br/>
        com.lfqy.trial.spring.bean.xmlconfig.impl.CDPlayer： <br/>
        ```java
        package com.lfqy.trial.spring.bean.xmlconfig.impl;
        
        import com.lfqy.trial.spring.bean.xmlconfig.intrfc.CompactDisc;
        import com.lfqy.trial.spring.bean.xmlconfig.intrfc.MediaPlayer;
        
        public class CDPlayer implements MediaPlayer {
        
        	private CompactDisc cd;
        
        
        	// 构造器注入
        	public CDPlayer(CompactDisc cd) {
        		this.cd = cd;
        	}
        
        	public void play() {
        		cd.play();
        	}
        
        }
        ```
-   xml装配的配置文件 <br/>
    见src/main/resources/META-INF/spring/xmlconfig/xmlconfig.xml： <br/>
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:context="http://www.springframework.org/schema/context"
      xmlns:c="http://www.springframework.org/schema/c"
      xmlns:p="http://www.springframework.org/schema/p"
      xmlns:util="http://www.springframework.org/schema/util"
      xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
    		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
    		http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">
    
    	<bean id="compactDisc" class="com.lfqy.trial.spring.bean.xmlconfig.impl.SgtPeppers"/>
    
    	<!-- 构造器注入 -->
    	<bean id="cdPlayer" class="com.lfqy.trial.spring.bean.xmlconfig.impl.CDPlayer">
    		<constructor-arg ref="compactDisc"/>
    	</bean>
    </beans>
    ```
-   运行测试java类 <br/>
    XmlConfigMain类，com.lfqy.trial.spring.bean.xmlconfig.XmlConfigMain： <br/>
    ```java
    package com.lfqy.trial.spring.bean.xmlconfig;
    
    import com.lfqy.trial.spring.bean.xmlconfig.intrfc.MediaPlayer;
    import org.springframework.context.ApplicationContext;
    import org.springframework.context.support.ClassPathXmlApplicationContext;
    
    public class XmlConfigMain {
        public static void main(String []args){
    	ApplicationContext ac = new ClassPathXmlApplicationContext("classpath:META-INF/spring/xmlconfig/xmlconfig.xml");
    	MediaPlayer mp = (MediaPlayer) ac.getBean("cdPlayer");
    	mp.play();
        }
    }
    ```
    运行这个类，输出如下： <br/>
    ```text
    Playing Sgt. Pepper's Lonely Hearts Club Band by The Beatles
    
    Process finished with exit code 0
    ```


### java方式装配bean {#java方式装配bean}

-   两个接口类 <br/>
    -   Compactdisc类 <br/>
        com.lfqy.trial.spring.bean.javaconfig.intrfc.CompactDisc： <br/>
        ```java
        package com.lfqy.trial.spring.bean.javaconfig.intrfc;
        
        public interface CompactDisc {
            void play();
        }
        ```
    -   MediaPlayer类 <br/>
        com.lfqy.trial.spring.bean.javaconfig.intrfc.MediaPlayer： <br/>
        ```java
        package com.lfqy.trial.spring.bean.javaconfig.intrfc;
        
        public interface MediaPlayer {
        
        	void play();
        
        }
        ```
-   两个对应的实现类 <br/>
    -   SgtPepper类 <br/>
        com.lfqy.trial.spring.bean.javaconfig.impl.SgtPeppers： <br/>
        ```java
        package com.lfqy.trial.spring.bean.javaconfig.impl;
        
        import com.lfqy.trial.spring.bean.javaconfig.intrfc.CompactDisc;
        
        public class SgtPeppers implements CompactDisc {
        
        	//英国摇滚乐队The Beatles的第八张录音室专辑
        	private String title = "Sgt. Pepper's Lonely Hearts Club Band";
        	private String artist = "The Beatles";
        
        	public void play() {
        		System.out.println("Playing " + title + " by " + artist);
        	}
        
        }
        ```
    -   CDPlayer类 <br/>
        com.lfqy.trial.spring.bean.javaconfig.impl.CDPlayer： <br/>
        ```java
        package com.lfqy.trial.spring.bean.javaconfig.impl;
        
        import com.lfqy.trial.spring.bean.javaconfig.intrfc.CompactDisc;
        import com.lfqy.trial.spring.bean.javaconfig.intrfc.MediaPlayer;
        
        public class CDPlayer implements MediaPlayer {
        
        	private CompactDisc cd;
        
        	public CDPlayer(CompactDisc cd) {
        		this.cd = cd;
        	}
        
        	public void play() {
        		cd.play();
        	}
        
        }
        ```
-   装配bean的java类 <br/>
    CDPlayerConfig类，com.lfqy.trial.spring.bean.javaconfig.config.CDPlayerConfig： <br/>
    ```java
    package com.lfqy.trial.spring.bean.javaconfig.config;
    
    import com.lfqy.trial.spring.bean.javaconfig.intrfc.CompactDisc;
    import com.lfqy.trial.spring.bean.javaconfig.impl.CDPlayer;
    import com.lfqy.trial.spring.bean.javaconfig.impl.SgtPeppers;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    
    @Configuration
    public class CDPlayerConfig {
    
    	@Bean
    	public CompactDisc compactDisc() {
    		return new SgtPeppers();
    	}
    
    	@Bean
    	public CDPlayer cdPlayer(CompactDisc compactDisc) {
    		return new CDPlayer(compactDisc);
    	}
    
    }
    ```
-   运行测试的java类 <br/>
    JavaConfigMain类，com.lfqy.trial.spring.bean.javaconfig.JavaConfigMain： <br/>
    ```java
    package com.lfqy.trial.spring.bean.javaconfig;
    
    import com.lfqy.trial.spring.bean.javaconfig.config.CDPlayerConfig;
    import com.lfqy.trial.spring.bean.javaconfig.intrfc.MediaPlayer;
    import org.springframework.context.ApplicationContext;
    import org.springframework.context.annotation.AnnotationConfigApplicationContext;
    
    public class JavaConfigMain {
        public static void main(String []args){
    	ApplicationContext ac = new AnnotationConfigApplicationContext(CDPlayerConfig.class);
    	MediaPlayer mp = (MediaPlayer) ac.getBean("cdPlayer");
    	mp.play();
        }
    }
    ```
    运行这个类，输出如下： <br/>
    ```text
    Playing Sgt. Pepper's Lonely Hearts Club Band by The Beatles
    
    Process finished with exit code 0
    ```


### 混合xml和java方式装配bean {#混合xml和java方式装配bean}

-   两个接口类 <br/>
    -   Compactdisc类 <br/>
        com.lfqy.trial.spring.bean.mixedconfig.intrfc.CompactDisc： <br/>
        ```java
        package com.lfqy.trial.spring.bean.mixedconfig.intrfc;
        
        public interface CompactDisc {
            void play();
        }
        ```
    -   MediaPlayer类 <br/>
        com.lfqy.trial.spring.bean.mixedconfig.intrfc.MediaPlayer： <br/>
        ```java
        package com.lfqy.trial.spring.bean.mixedconfig.intrfc;
        
        public interface MediaPlayer {
        
        	void play();
        
        }
        ```
-   几个对应的实现类 <br/>
    -   SgtPepper类 <br/>
        com.lfqy.trial.spring.bean.mixedconfig.impl.SgtPeppers： <br/>
        ```java
        package com.lfqy.trial.spring.bean.mixedconfig.impl;
        
        import com.lfqy.trial.spring.bean.mixedconfig.intrfc.CompactDisc;
        
        public class SgtPeppers implements CompactDisc {
        
        	//英国摇滚乐队The Beatles的第八张录音室专辑
        	private String title = "Sgt. Pepper's Lonely Hearts Club Band";
        	private String artist = "The Beatles";
        
        	public void play() {
        		System.out.println("Playing " + title + " by " + artist);
        	}
        
        }
        ```
    -   CDPlayer类 <br/>
        com.lfqy.trial.spring.bean.mixedconfig.impl.CDPlayer： <br/>
        ```java
        package com.lfqy.trial.spring.bean.mixedconfig.impl;
        
        import com.lfqy.trial.spring.bean.mixedconfig.intrfc.CompactDisc;
        import com.lfqy.trial.spring.bean.mixedconfig.intrfc.MediaPlayer;
        
        public class CDPlayer implements MediaPlayer {
        
        	private CompactDisc cd;
        
        
        	// 构造器注入
        	public CDPlayer(CompactDisc cd) {
        		this.cd = cd;
        	}
        
        	public void play() {
        		cd.play();
        	}
        
        }
        ```
    -   BlankDisc类 <br/>
        com.lfqy.trial.spring.bean.mixedconfig.impl.BlankDisc： <br/>
        ```java
        package com.lfqy.trial.spring.bean.mixedconfig.impl;
        
        import com.lfqy.trial.spring.bean.mixedconfig.intrfc.CompactDisc;
        import java.util.List;
        
        public class BlankDisc implements CompactDisc {
        
        	private String title;
        	private String artist;
        	private List<String> tracks;
        
        	public BlankDisc(String title, String artist, List<String> tracks) {
        		this.title = title;
        		this.artist = artist;
        		this.tracks = tracks;
        	}
        
        	public void play() {
        		System.out.println("Playing " + title + " by " + artist);
        		for (String track : tracks) {
        			System.out.println("-Track: " + track);
        		}
        	}
        
        }
        ```
-   java中混入xml的装配 <br/>
    -   通过xml装配部分bean <br/>
        见src/main/resources/META-INF/spring/mixedconfig/xml2java/mixedconfig-cd.xml： <br/>
        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:c="http://www.springframework.org/schema/c"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        
          <!-- c:_0 构造函数的第一个参数 -->
          <!-- c:_1 构造函数的第二个参数 -->
          <!-- constructor-arg 构造函数的第三个参数 -->
          <bean id="compactDisc"
        	class="com.lfqy.trial.spring.bean.mixedconfig.impl.BlankDisc"
        	c:_0="Now and Then"
        	c:_1="Carpenters">
            <constructor-arg>
              <list>
        	<value>Yesterday Once More</value>
        		<value>The End of the World</value>
              </list>
            </constructor-arg>
          </bean>
        
        </beans>
        ```
    -   在java中引用xml装配并配置其它bean <br/>
        见com.lfqy.trial.spring.bean.mixedconfig.config.xml2java.CDPlayerConfig： <br/>
        ```java
        package com.lfqy.trial.spring.bean.mixedconfig.config.xml2java;
        
        import com.lfqy.trial.spring.bean.mixedconfig.intrfc.CompactDisc;
        import com.lfqy.trial.spring.bean.mixedconfig.impl.CDPlayer;
        import org.springframework.context.annotation.Bean;
        import org.springframework.context.annotation.Configuration;
        import org.springframework.context.annotation.ImportResource;
        
        @Configuration
        @ImportResource("classpath:META-INF/spring/mixedconfig/xml2java/mixedconfig-cd.xml")
        public class CDPlayerConfig {
        
        	@Bean
        	public CDPlayer cdPlayer(CompactDisc compactDisc) {
        		return new CDPlayer(compactDisc);
        	}
        
        }
        ```
    -   测试java中引用xml装配 <br/>
        XmlMixed2JavaConfigMain类，com.lfqy.trial.spring.bean.mixedconfig.XmlMixed2JavaConfigMain： <br/>
        ```java
        package com.lfqy.trial.spring.bean.mixedconfig;
        
        import com.lfqy.trial.spring.bean.mixedconfig.config.xml2java.CDPlayerConfig;
        import com.lfqy.trial.spring.bean.mixedconfig.intrfc.MediaPlayer;
        import org.springframework.context.ApplicationContext;
        import org.springframework.context.annotation.AnnotationConfigApplicationContext;
        
        public class XmlMixed2JavaConfigMain {
            public static void main(String []args){
        	ApplicationContext ac = new AnnotationConfigApplicationContext(CDPlayerConfig.class);
        	MediaPlayer mp = (MediaPlayer) ac.getBean("cdPlayer");
        	mp.play();
            }
        }
        ```
        运行该类，结果如下： <br/>
        ```text
        Playing Now and Then by Carpenters
        -Track: Yesterday Once More
        -Track: The End of the World
        
        Process finished with exit code 0
        ```
-   xml中混入java的装配 <br/>
    -   通过java装配部分bean <br/>
        CDConfig类，com.lfqy.trial.spring.bean.mixedconfig.config.java2xml.CDConfig： <br/>
        ```java
        package com.lfqy.trial.spring.bean.mixedconfig.config.java2xml;
        
        import org.springframework.context.annotation.Bean;
        import org.springframework.context.annotation.Configuration;
        import com.lfqy.trial.spring.bean.mixedconfig.intrfc.CompactDisc;
        import com.lfqy.trial.spring.bean.mixedconfig.impl.SgtPeppers;
        
        @Configuration
        public class CDConfig {
        
        	@Bean
        	public CompactDisc compactDisc() {
        			return new SgtPeppers();
        	}
        }
        ```
    -   在xml中引用java装配 <br/>
        见src/main/resources/META-INF/spring/mixedconfig/java2xml/mixedconfig-cdplayer.xml： <br/>
        ```xml { linenos=true, anchorlinenos=true, lineanchors=org-coderef--cc6246 }
        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
               xmlns:context="http://www.springframework.org/schema/context"
          xmlns:c="http://www.springframework.org/schema/c"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
          http://www.springframework.org/schema/context/spring-context.xsd">
          <!-- 如果不加这一行，通过ClassPathXmlApplicationContext创建context时，会找不到compactDisc这个bean -->
          <context:component-scan base-package="com.lfqy.trial.spring.bean.mixedconfig.config.java2xml"/>
          <!-- 引入Java Config，配置了cd的bean，实际上有了component-scan那一行之后(autoconfig)，这一行的配置就多余了，可以没有 -->
          <bean class="com.lfqy.trial.spring.bean.mixedconfig.config.java2xml.CDConfig" />
        <!--  <bean id="compactDisc" class="com.lfqy.trial.spring.bean.mixedconfig.impl.SgtPeppers"/>-->
            <!-- cd-ref来自Java Config -->
          <bean id="cdPlayer" class="com.lfqy.trial.spring.bean.mixedconfig.impl.CDPlayer"
        	c:cd-ref="compactDisc" />
        
        </beans>
        ```
    -   测试xml中引用java装配 <br/>
        JavaMixed2XmlConfigMain类，com.lfqy.trial.spring.bean.mixedconfig.JavaMixed2XmlConfigMain： <br/>
        ```java
        package com.lfqy.trial.spring.bean.mixedconfig;
        
        import com.lfqy.trial.spring.bean.mixedconfig.intrfc.MediaPlayer;
        import org.springframework.context.ApplicationContext;
        import org.springframework.context.support.ClassPathXmlApplicationContext;
        
        public class JavaMixed2XmlConfigMain {
            public static void main(String []args){
        	ApplicationContext ac = new ClassPathXmlApplicationContext("classpath:META-INF/spring/mixedconfig/java2xml/mixedconfig-cdplayer.xml");
        	MediaPlayer mp = (MediaPlayer) ac.getBean("cdPlayer");
        	mp.play();
            }
        }
        ```
        运行该java类，输出如下： <br/>
        ```text
        Playing Sgt. Pepper's Lonely Hearts Club Band by The Beatles
        
        Process finished with exit code 0
        ```
        注意： <br/>
        这里在[xml装配的配置](#org-coderef--cc6246-11)中，添加了 `<context:component-scan base-package="com.lfqy.trial.spring.bean.mixedconfig.config.java2xml"/>` 。 <br/>
        如果不加这个配置，如下： <br/>
        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
               xmlns:context="http://www.springframework.org/schema/context"
          xmlns:c="http://www.springframework.org/schema/c"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
          http://www.springframework.org/schema/context/spring-context.xsd">
          <!-- 如果不加这一行，通过ClassPathXmlApplicationContext创建context时，会找不到compactDisc这个bean -->
        <!--  <context:component-scan base-package="com.lfqy.trial.spring.bean.mixedconfig.config.java2xml"/>-->
          <!-- 引入Java Config，配置了cd的bean，实际上有了component-scan那一行之后(autoconfig)，这一行的配置就多余了，可以没有 -->
          <bean class="com.lfqy.trial.spring.bean.mixedconfig.config.java2xml.CDConfig" />
        <!--  <bean id="compactDisc" class="com.lfqy.trial.spring.bean.mixedconfig.impl.SgtPeppers"/>-->
            <!-- cd-ref来自Java Config -->
          <bean id="cdPlayer" class="com.lfqy.trial.spring.bean.mixedconfig.impl.CDPlayer"
        	c:cd-ref="compactDisc" />
        
        </beans>
        ```
        运行前面java类就会报错： <br/>
        ```text
        org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'cdPlayer' defined in class path resource [META-INF/spring/mixedconfig/java2xml/mixedconfig-cdplayer.xml]: Cannot resolve reference to bean 'compactDisc' while setting constructor argument; nested exception is org.springframework.beans.factory.NoSuchBeanDefinitionException: No bean named 'compactDisc' is defined
        	at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveReference(BeanDefinitionValueResolver.java:359)
        	at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveValueIfNecessary(BeanDefinitionValueResolver.java:108)
        	at org.springframework.beans.factory.support.ConstructorResolver.resolveConstructorArguments(ConstructorResolver.java:648)
        	at org.springframework.beans.factory.support.ConstructorResolver.autowireConstructor(ConstructorResolver.java:140)
        	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.autowireConstructor(AbstractAutowireCapableBeanFactory.java:1131)
        	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBeanInstance(AbstractAutowireCapableBeanFactory.java:1034)
        	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:504)
        	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:476)
        	at org.springframework.beans.factory.support.AbstractBeanFactory$1.getObject(AbstractBeanFactory.java:303)
        	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:230)
        	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:299)
        	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:194)
        	at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingletons(DefaultListableBeanFactory.java:762)
        	at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:757)
        	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:480)
        	at org.springframework.context.support.ClassPathXmlApplicationContext.<init>(ClassPathXmlApplicationContext.java:139)
        	at org.springframework.context.support.ClassPathXmlApplicationContext.<init>(ClassPathXmlApplicationContext.java:83)
        	at com.lfqy.trial.spring.bean.mixedconfig.JavaMixed2XmlConfigMain.main(JavaMixed2XmlConfigMain.java:9)
        Caused by: org.springframework.beans.factory.NoSuchBeanDefinitionException: No bean named 'compactDisc' is defined
        	at org.springframework.beans.factory.support.DefaultListableBeanFactory.getBeanDefinition(DefaultListableBeanFactory.java:694)
        	at org.springframework.beans.factory.support.AbstractBeanFactory.getMergedLocalBeanDefinition(AbstractBeanFactory.java:1168)
        	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:281)
        	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:194)
        	at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveReference(BeanDefinitionValueResolver.java:351)
        	... 17 more
        ```
        比较奇怪的是，这时候，我通过一个单元测试类运行，见com.lfqy.trial.spring.bean.mixedconfig.Java2XmlConfigTest： <br/>
        ```java
        package com.lfqy.trial.spring.bean.mixedconfig;
        
        import com.lfqy.trial.spring.bean.mixedconfig.intrfc.MediaPlayer;
        import org.junit.Test;
        import org.junit.runner.RunWith;
        import org.springframework.beans.factory.annotation.Autowired;
        import org.springframework.test.context.ContextConfiguration;
        import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
        
        @RunWith(SpringJUnit4ClassRunner.class)
        @ContextConfiguration("classpath:META-INF/spring/mixedconfig/java2xml/mixedconfig-cdplayer.xml")
        public class Java2XmlConfigTest {
        
          @Autowired
          private MediaPlayer player;
        
          @Test
          public void play() {
            player.play();
          }
        
        }
        ```
        这时候运行就正常，输出如下： <br/>
        ```text
        Playing Sgt. Pepper's Lonely Hearts Club Band by The Beatles
        
        Process finished with exit code 0
        ```
        这个问题比较奇怪，解决的方式也比较奇葩。因为如果在[配置中添加了ComponentScan](#org-coderef--cc6246-11)，实际就成了自动装配了。[java装配部分的引用](#org-coderef--cc6246-13)，可以不加(实测不加也可以正常运行)。 <br/>


### 自动装配 {#自动装配}

-   两个接口类 <br/>
    -   Compactdisc类 <br/>
        com.lfqy.trial.spring.bean.autoconfig.intrfc.CompactDisc： <br/>
        ```java
        package com.lfqy.trial.spring.bean.autoconfig.intrfc;
        
        public interface CompactDisc {
            void play();
        }
        ```
    -   MediaPlayer类 <br/>
        com.lfqy.trial.spring.bean.autoconfig.intrfc.MediaPlayer： <br/>
        ```java
        package com.lfqy.trial.spring.bean.autoconfig.intrfc;
        
        public interface MediaPlayer {
        
        	void play();
        
        }
        ```
-   两个对应的实现类 <br/>
    -   SgtPepper类 <br/>
        com.lfqy.trial.spring.bean.autoconfig.impl.SgtPeppers： <br/>
        ```java
        package com.lfqy.trial.spring.bean.autoconfig.impl;
        
        import com.lfqy.trial.spring.bean.autoconfig.intrfc.CompactDisc;
        import org.springframework.stereotype.Component;
        
        @Component
        public class SgtPeppers implements CompactDisc {
        
        	//英国摇滚乐队The Beatles的第八张录音室专辑
        	private String title = "Sgt. Pepper's Lonely Hearts Club Band";
        	private String artist = "The Beatles";
        
        	public void play() {
        		System.out.println("Playing " + title + " by " + artist);
        	}
        
        }
        ```
    -   CDPlayer类 <br/>
        com.lfqy.trial.spring.bean.autoconfig.impl.CDPlayer： <br/>
        ```java
        package com.lfqy.trial.spring.bean.autoconfig.impl;
        
        import com.lfqy.trial.spring.bean.autoconfig.intrfc.CompactDisc;
        import com.lfqy.trial.spring.bean.autoconfig.intrfc.MediaPlayer;
        import org.springframework.beans.factory.annotation.Autowired;
        import org.springframework.stereotype.Component;
        
        @Component(value = "cdPlayer")//Component注解的value参数，标识bean的名字
        public class CDPlayer implements MediaPlayer {
        
        	@Autowired
        	private CompactDisc cd;
        
        	public void play() {
        		cd.play();
        	}
        
        }
        ```
-   xml方式实现自动装配 <br/>
    -   xml自动装配配置 <br/>
        见src/main/resources/META-INF/spring/autoconfig/autoconfig.xml： <br/>
        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:c="http://www.springframework.org/schema/c"
          xmlns:p="http://www.springframework.org/schema/p"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
        
          <context:component-scan base-package="com.lfqy.trial.spring.bean.autoconfig" />
        
        </beans>
        ```
    -   xml自动装配测试 <br/>
        类，com.lfqy.trial.spring.bean.autoconfig.AutoConfigByXmlMain： <br/>
        ```java
        package com.lfqy.trial.spring.bean.autoconfig;
        
        import com.lfqy.trial.spring.bean.autoconfig.intrfc.MediaPlayer;
        import org.springframework.context.ApplicationContext;
        import org.springframework.context.support.ClassPathXmlApplicationContext;
        
        public class AutoConfigByXmlMain {
            public static void main(String []args){
        	ApplicationContext ac = new ClassPathXmlApplicationContext("classpath:META-INF/spring/autoconfig/autoconfig.xml");
        	MediaPlayer mp = (MediaPlayer) ac.getBean("cdPlayer");
        	mp.play();
            }
        }
        ```
        运行该类，结果如下： <br/>
        ```text
        Playing Sgt. Pepper's Lonely Hearts Club Band by The Beatles
        
        Process finished with exit code 0
        ```
-   java方式实现自动装配 <br/>
    -   新增ComponentScan包标记类 <br/>
        通过这个类标记扫描包，该类所在的包和子包都会被扫描。见com.lfqy.trial.spring.bean.autoconfig.ComponentScanPoint： <br/>
        ```java
        package com.lfqy.trial.spring.bean.autoconfig;
        
        /**
        ​ * 标记扫描包，该类所在的包和子包都会被扫描
         */
        
        public interface ComponentScanPoint {
        }
        ```
    -   新增自动扫描java配置类 <br/>
        见com.lfqy.trial.spring.bean.autoconfig.config.CDPlayerConfig： <br/>
        ```java
        package com.lfqy.trial.spring.bean.autoconfig.config;
        
        import com.lfqy.trial.spring.bean.autoconfig.ComponentScanPoint;
        import org.springframework.context.annotation.ComponentScan;
        import org.springframework.context.annotation.Configuration;
        
        
        @Configuration
        //@ComponentScan不指定路径的话，会扫描该类所在包及其子包
        //@ComponentScan(basePackages="com.lfqy.trial.spring.bean.autoconfig")
        @ComponentScan(basePackageClasses=ComponentScanPoint.class)//扫描ComponentScanPoint所在包及其子包；相比上一种，类型安全
        public class CDPlayerConfig { 
        }
        ```
    -   新增自动扫描java配置测试类 <br/>
        AutoConfigByJavaMain类，com.lfqy.trial.spring.bean.autoconfig.AutoConfigByJavaMain： <br/>
        ```java
        package com.lfqy.trial.spring.bean.autoconfig;
        
        import com.lfqy.trial.spring.bean.autoconfig.config.CDPlayerConfig;
        import com.lfqy.trial.spring.bean.autoconfig.intrfc.MediaPlayer;
        import org.springframework.context.ApplicationContext;
        import org.springframework.context.annotation.AnnotationConfigApplicationContext;
        
        public class AutoConfigByJavaMain {
            public static void main(String []args){
        	ApplicationContext ac = new AnnotationConfigApplicationContext(CDPlayerConfig.class);
        	MediaPlayer mp = (MediaPlayer) ac.getBean("cdPlayer");
        	mp.play();
            }
        }
        ```
        运行该java类，结果如下： <br/>
        ```text
        Playing Sgt. Pepper's Lonely Hearts Club Band by The Beatles
        
        Process finished with exit code 0
        ```


## Spring装配进阶 {#spring装配进阶}


### 说明 {#说明}

本章节中代码参考[^fn:3]，项目的完整目录结构如下： <br/>

```text
spring-bean $ tree .
.
├── pom.xml
└── src
    ├── main
    │   ├── java
    │   │   └── com
    │   │       └── lfqy
    │   │           └── trial
    │   │               └── spring
    │   │                   └── bean
    │   │                       ├── advancedassembly
    │   │                       │   ├── injectfromprop
    │   │                       │   │   ├── InjectFromPropConfig.java
    │   │                       │   │   ├── InjectFromPropConfigMain.java
    │   │                       │   │   ├── impl
    │   │                       │   │   │   └── BlankDisc.java
    │   │                       │   │   └── intrfc
    │   │                       │   │       └── CompactDisc.java
    │   │                       │   ├── propplaceholder
    │   │                       │   │   ├── PropPlaceHolderConfig.java
    │   │                       │   │   ├── PropPlaceHolderConfigMain.java
    │   │                       │   │   ├── impl
    │   │                       │   │   │   └── BlankDisc.java
    │   │                       │   │   └── intrfc
    │   │                       │   │       └── CompactDisc.java
    │   │                       │   └── spel
    │   │                       │       ├── SpELConfig.java
    │   │                       │       ├── SpELConfigMain.java
    │   │                       │       ├── impl
    │   │                       │       │   └── BlankDisc.java
    │   │                       │       ├── intrfc
    │   │                       │       │   └── CompactDisc.java
    │   │                       │       └── other
    │   │                       │           ├── DiscSpEL.java
    │   │                       │           └── SystemProp.java
    │   │                       ├── autoconfig
    │   │                       │   ├── AutoConfigByJavaMain.java
    │   │                       │   ├── AutoConfigByXmlMain.java
    │   │                       │   ├── ComponentScanPoint.java
    │   │                       │   ├── config
    │   │                       │   │   └── CDPlayerConfig.java
    │   │                       │   ├── impl
    │   │                       │   │   ├── CDPlayer.java
    │   │                       │   │   └── SgtPeppers.java
    │   │                       │   └── intrfc
    │   │                       │       ├── CompactDisc.java
    │   │                       │       └── MediaPlayer.java
    │   │                       ├── javaconfig
    │   │                       │   ├── JavaConfigMain.java
    │   │                       │   ├── config
    │   │                       │   │   └── CDPlayerConfig.java
    │   │                       │   ├── impl
    │   │                       │   │   ├── CDPlayer.java
    │   │                       │   │   └── SgtPeppers.java
    │   │                       │   └── intrfc
    │   │                       │       ├── CompactDisc.java
    │   │                       │       └── MediaPlayer.java
    │   │                       ├── mixedconfig
    │   │                       │   ├── JavaMixed2XmlConfigMain.java
    │   │                       │   ├── XmlMixed2JavaConfigMain.java
    │   │                       │   ├── config
    │   │                       │   │   ├── java2xml
    │   │                       │   │   │   └── CDConfig.java
    │   │                       │   │   └── xml2java
    │   │                       │   │       └── CDPlayerConfig.java
    │   │                       │   ├── impl
    │   │                       │   │   ├── BlankDisc.java
    │   │                       │   │   ├── CDPlayer.java
    │   │                       │   │   └── SgtPeppers.java
    │   │                       │   └── intrfc
    │   │                       │       ├── CompactDisc.java
    │   │                       │       └── MediaPlayer.java
    │   │                       └── xmlconfig
    │   │                           ├── XmlConfigMain.java
    │   │                           ├── impl
    │   │                           │   ├── CDPlayer.java
    │   │                           │   └── SgtPeppers.java
    │   │                           └── intrfc
    │   │                               ├── CompactDisc.java
    │   │                               └── MediaPlayer.java
    │   └── resources
    │       └── META-INF
    │           └── spring
    │               ├── advancedassembly
    │               │   ├── injectfromprop
    │               │   │   └── app.properties
    │               │   ├── propplaceholder
    │               │   │   └── app.properties
    │               │   └── spel
    │               │       └── app.properties
    │               ├── autoconfig
    │               │   └── autoconfig.xml
    │               ├── mixedconfig
    │               │   ├── java2xml
    │               │   │   └── mixedconfig-cdplayer.xml
    │               │   └── xml2java
    │               │       └── mixedconfig-cd.xml
    │               └── xmlconfig
    │                   └── xmlconfig.xml
    └── test
	└── java
	    └── com
		└── lfqy
		    └── trial
			└── spring
			    └── bean
				└── mixedconfig
				    └── Java2XmlConfigTest.java

56 directories, 51 files
spring-bean $ 
```

项目的pom文件，见pom.xml： <br/>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
	 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.lfqy.trial</groupId>
    <artifactId>spring-bean</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
	<maven.compiler.source>8</maven.compiler.source>
	<maven.compiler.target>8</maven.compiler.target>
	<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	<!-- Spring 4.3版本之前，必须要添加一个PropertySourcesPlaceholderConfigurer类型的bean。4.3之后，就可以直接使用，因为spring会使用默认的DefaultPropertySourceFactory解析。 -->
	<!--        <spring.version>5.1.6.RELEASE</spring.version>-->
	<spring.version>4.1.4.RELEASE</spring.version>
	<aspectj.version>1.7.2</aspectj.version>
    </properties>
    <dependencies>
	<!-- Spring Core -->
	<!-- http://mvnrepository.com/artifact/org.springframework/spring-core -->
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-core</artifactId>
	    <version>${spring.version}</version>
	</dependency>
	<!-- Spring Context -->
	<!-- http://mvnrepository.com/artifact/org.springframework/spring-context -->
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-context</artifactId>
	    <version>${spring.version}</version>
	</dependency>


	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-test</artifactId>
	    <version>${spring.version}</version>
	    <scope>test</scope>
	</dependency>

	<dependency>
	    <groupId>org.aspectj</groupId>
	    <artifactId>aspectjweaver</artifactId>
	    <version>${aspectj.version}</version>
	</dependency>

	<dependency>
	    <groupId>junit</groupId>
	    <artifactId>junit</artifactId>
	    <version>4.13.1</version>
	    <scope>test</scope>
	</dependency>

	<dependency>
	    <groupId>org.mockito</groupId>
	    <artifactId>mockito-core</artifactId>
	    <version>2.23.4</version>
	    <scope>test</scope>
	</dependency>

	<dependency>
	    <groupId>org.apache.commons</groupId>
	    <artifactId>commons-lang3</artifactId>
	    <version>3.1</version>
	</dependency>
    </dependencies>
</project>
```


### 设置bean的作用域 {#设置bean的作用域}


#### Spring中的Bean作用域说明 {#spring中的bean作用域说明}

参考[^fn:5] <br/>
当通过spring容器创建一个Bean实例时，不仅可以完成Bean实例的实例化，还可以为Bean指定特定的作用域。Spring支持如下5种作用域： <br/>

-   singleton <br/>
    在整个Spring IoC容器中，使用singleton定义的Bean将只有一个实例 <br/>
-   prototype <br/>
    每次通过容器的getBean方法获取prototype定义的Bean时，都将产生一个新的Bean实例 <br/>
-   request <br/>
    对于每次HTTP请求，使用request定义的Bean都将产生一个新实例，即每次HTTP请求将会产生不同的Bean实例。只有在Web应用中使用Spring时，该作用域才有效。 <br/>
-   session <br/>
    对于每次HTTP Session，使用session定义的Bean豆浆产生一个新实例。同样只有在Web应用中使用Spring时，该作用域才有效。 <br/>
-   globalsession <br/>
    每个全局的HTTP Session，使用session定义的Bean都将产生一个新实例。典型情况下，仅在使用portlet context的时候有效。同样只有在Web应用中使用Spring时，该作用域才有效。 <br/>

其中，比较常用的是singleton和prototype两种作用域。对于singleton作用域的Bean，每次请求该Bean都将获得相同的实例。容器负责跟踪Bean实例的状态，负责维护Bean实例的生命周期行为；如果一个Bean被设置成prototype作用域，程序每次请求该id的Bean，Spring都会新建一个Bean实例，然后返回给程序。在这种情况下，Spring容器仅仅使用new 关键字创建Bean实例，一旦创建成功，容器不在跟踪实例，也不会维护Bean实例的状态。 <br/>
如果不指定Bean的作用域，Spring默认使用singleton作用域。Java在创建Java实例时，需要进行内存申请；销毁实例时，需要完成垃圾回收，这些工作都会导致系统开销的增加。因此，prototype作用域Bean的创建、销毁代价比较大。而singleton作用域的Bean实例一旦创建成功，可以重复使用。因此，除非必要，否则尽量避免将Bean被设置成prototype作用域。 <br/>


#### Singleton和Prototype作用域的设置示例 {#singleton和prototype作用域的设置示例}

-   用于创建bean的类 <br/>
    -   PrototypeC类 <br/>
        这个类用于创建prototype作用域的bean，见com.lfqy.trial.spring.bean.advancedassembly.scope.PrototypeC： <br/>
        ```java
        package com.lfqy.trial.spring.bean.advancedassembly.scope;
        
        import org.springframework.beans.factory.config.ConfigurableBeanFactory;
        import org.springframework.context.annotation.Scope;
        import org.springframework.stereotype.Component;
        
        @Component
        @Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)
        public class PrototypeC {
        }
        ```
        这个类上加了注解 `@Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)` ，这样这个bean会被创建为 `ConfigurableBeanFactory.SCOPE_PROTOTYPE` 作用域，每次请求bean都会重新创建一个bean实例。 <br/>
    -   Singleton1C类 <br/>
        见com.lfqy.trial.spring.bean.advancedassembly.scope.Singleton1C： <br/>
        ```java
        package com.lfqy.trial.spring.bean.advancedassembly.scope;
        
        import org.springframework.beans.factory.config.ConfigurableBeanFactory;
        import org.springframework.context.annotation.Scope;
        import org.springframework.stereotype.Component;
        
        @Component
        public class Singleton1C {
        }
        ```
        这个类上，没有加scope注解，默认bean的作用域是singleton。 <br/>
    -   Singleton2C类 <br/>
        见com.lfqy.trial.spring.bean.advancedassembly.scope.Singleton2C： <br/>
        ```java
        package com.lfqy.trial.spring.bean.advancedassembly.scope;
        
        import org.springframework.beans.factory.config.ConfigurableBeanFactory;
        import org.springframework.context.annotation.Scope;
        import org.springframework.stereotype.Component;
        
        @Component
        @Scope(ConfigurableBeanFactory.SCOPE_SINGLETON)
        public class Singleton2C {
        }
        ```
        这个类上，显式添加了 `@Scope(ConfigurableBeanFactory.SCOPE_SINGLETON)` 注解，效果上和缺省的不加一样。 <br/>
-   作用域运行测试类 <br/>
    见com.lfqy.trial.spring.bean.advancedassembly.scope.ScopeConfigMain： <br/>
    ```java
    package com.lfqy.trial.spring.bean.advancedassembly.scope;
    
    import org.springframework.context.ApplicationContext;
    import org.springframework.context.annotation.AnnotationConfigApplicationContext;
    import org.springframework.context.annotation.ComponentScan;
    import org.springframework.context.annotation.Configuration;
    
    @Configuration
    @ComponentScan(basePackageClasses= ScopeConfigMain.class)//扫描InjectFromPropertiesConfig所在包及其子包
    public class ScopeConfigMain {
        public static void main(String []args){
    	ApplicationContext ac = new AnnotationConfigApplicationContext(ScopeConfigMain.class);
    	PrototypeC prototypeC1 = (PrototypeC) ac.getBean("prototypeC");
    	PrototypeC prototypeC2 = (PrototypeC) ac.getBean("prototypeC");
    	System.out.println("打印PrototypeC这个类两次获取bean的结果：");
    	System.out.println(prototypeC1);
    	System.out.println(prototypeC2);
    	System.out.println("打印Singleton1C这个类两次获取bean的结果：");
    	Singleton1C singleton1C1 = (Singleton1C) ac.getBean("singleton1C");
    	Singleton1C singleton1C2 = (Singleton1C) ac.getBean("singleton1C");
    	System.out.println(singleton1C1);
    	System.out.println(singleton1C2);
    	System.out.println("打印Singleton2C这个类两次获取bean的结果：");
    	Singleton2C singleton2C1 = (Singleton2C) ac.getBean("singleton2C");
    	Singleton2C singleton2C2 = (Singleton2C) ac.getBean("singleton2C");
    	System.out.println(singleton2C1);
    	System.out.println(singleton2C2);
        }
    }
    ```
    运行结果如下： <br/>
    ```text
    打印PrototypeC这个类两次获取bean的结果：
    com.lfqy.trial.spring.bean.advancedassembly.scope.PrototypeC@7e0e6aa2
    com.lfqy.trial.spring.bean.advancedassembly.scope.PrototypeC@365185bd
    打印Singleton1C这个类两次获取bean的结果：
    com.lfqy.trial.spring.bean.advancedassembly.scope.Singleton1C@18bf3d14
    com.lfqy.trial.spring.bean.advancedassembly.scope.Singleton1C@18bf3d14
    打印Singleton2C这个类两次获取bean的结果：
    com.lfqy.trial.spring.bean.advancedassembly.scope.Singleton2C@4fb64261
    com.lfqy.trial.spring.bean.advancedassembly.scope.Singleton2C@4fb64261
    
    Process finished with exit code 0
    ```
    从这里看出，作用域是prototype的bean每次获取，拿到的都是一个新创建的类实例。而作用域是singleton的bean每次获取拿到的都是同一个类实例。 <br/>


### 按条件装配 {#按条件装配}

spring支持当具备某种条件时，才装配某个bean。如下是一个例子： <br/>

-   准备创建bean的类 <br/>
    见com.lfqy.trial.spring.bean.advancedassembly.conditional.MagicClass： <br/>
    ```java
    package com.lfqy.trial.spring.bean.advancedassembly.conditional;
    
    public class MagicClass {
        public void print(){
    	System.out.println("This is a instance of magic class.");
        }
    }
    ```
-   条件化装配配置类 <br/>
    见com.lfqy.trial.spring.bean.advancedassembly.conditional.ConditionalConfig： <br/>
    ```java { linenos=true, anchorlinenos=true, lineanchors=org-coderef--5bbbf1 }
    package com.lfqy.trial.spring.bean.advancedassembly.conditional;
    
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Conditional;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.context.annotation.PropertySource;
    
    @Configuration
    @PropertySource("classpath:META-INF/spring/advancedassembly/conditional/app.properties")      (addConfigurationFileAnnotation)
    public class ConditionalConfig {
    
    	/**
    ​	* 条件化地创建bean
    	 */
    	@Bean
    	@Conditional(MagicExistsCondition.class)
    	public MagicClass magicBean() {
    		return new MagicClass();
    	}
    }
    ```
-   装配条件实现类 <br/>
    见com.lfqy.trial.spring.bean.advancedassembly.conditional.MagicExistsCondition： <br/>
    ```java { linenos=true, anchorlinenos=true, lineanchors=org-coderef--587650 }
    package com.lfqy.trial.spring.bean.advancedassembly.conditional;
    
    import org.springframework.context.annotation.Condition;
    import org.springframework.context.annotation.ConditionContext;
    import org.springframework.core.env.Environment;
    import org.springframework.core.type.AnnotatedTypeMetadata;
    
    public class MagicExistsCondition implements Condition {
    
    	@Override
    	public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
    		//获取环境
    		Environment env = context.getEnvironment();                                            (getEnvironmentViaContext)
    		//包含环境属性 magic 则返回true
    		return env.containsProperty("magic");
    	}
    }
    ```
-   配置文件 <br/>
    见src/main/resources/META-INF/spring/advancedassembly/conditional/app.properties： <br/>
    ```text { linenos=true, anchorlinenos=true, lineanchors=org-coderef--0e7040 }
    # magic="this is a magic configuration item."
    magic="this is a magic configuration item."
    ```
-   运行测试类 <br/>
    见com.lfqy.trial.spring.bean.advancedassembly.conditional.ConditionalConfigMain： <br/>
    ```java
    package com.lfqy.trial.spring.bean.advancedassembly.conditional;
    
    import org.springframework.context.ApplicationContext;
    import org.springframework.context.annotation.AnnotationConfigApplicationContext;
    
    public class ConditionalConfigMain {
        public static void main(String []args){
    	ApplicationContext ac = new AnnotationConfigApplicationContext(ConditionalConfig.class);
    	MagicClass mc = (MagicClass) ac.getBean("magicBean");
    	mc.print();
        }
    }
    ```
    运行结果如下： <br/>
    ```text
    This is a instance of magic class.
    
    Process finished with exit code 0
    ```
    这时候，如果删除[magic的配置行](#org-coderef--0e7040-2)，只保留前面注释掉的行，会发现，运行报错： <br/>
    ```text
    三月 29, 2024 2:23:58 下午 org.springframework.context.annotation.AnnotationConfigApplicationContext prepareRefresh
    信息: Refreshing org.springframework.context.annotation.AnnotationConfigApplicationContext@685f4c2e: startup date [Fri Mar 29 14:23:58 CST 2024]; root of context hierarchy
    Exception in thread "main" org.springframework.beans.factory.NoSuchBeanDefinitionException: No bean named 'magicBean' is defined
    	 at org.springframework.beans.factory.support.DefaultListableBeanFactory.getBeanDefinition(DefaultListableBeanFactory.java:694)
    	at org.springframework.beans.factory.support.AbstractBeanFactory.getMergedLocalBeanDefinition(AbstractBeanFactory.java:1168)
    	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:281)
    	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:194)
    	at org.springframework.context.support.AbstractApplicationContext.getBean(AbstractApplicationContext.java:956)
    	at com.lfqy.trial.spring.bean.advancedassembly.conditional.ConditionalConfigMain.main(ConditionalConfigMain.java:9)
    
    Process finished with exit code 1
    ```
    说明这个bean确实没有加载。 <br/>
    需要注意，从这个例子可以看出，[添加了PropertySource这个注解](#org-coderef--5bbbf1-9)后，对应配置文件中的配置项，就添加到了Environment类对应的实例bean中。这样，就可以[通过ConditionContext类拿到这个实例](#org-coderef--587650-13)，然后判断其中是否有对应的配置项。也可以搞一个Environment的类变量，然后配置 `@Autowire` 也可以拿到这个实例，效果应该是一样的。 <br/>


### 注入外部配置文件配置项的值 {#注入外部配置文件配置项的值}

-   接口类 <br/>
    com.lfqy.trial.spring.bean.advancedassembly.injectfromprop.intrfc.CompactDisc： <br/>
    ```java
    package com.lfqy.trial.spring.bean.advancedassembly.injectfromprop.intrfc;
    
    public interface CompactDisc {
        void play();
    }
    ```
-   实现类 <br/>
    com.lfqy.trial.spring.bean.advancedassembly.injectfromprop.impl.BlankDisc： <br/>
    ```java
    package com.lfqy.trial.spring.bean.advancedassembly.injectfromprop.impl;
    
    import com.lfqy.trial.spring.bean.advancedassembly.injectfromprop.intrfc.CompactDisc;
    
    import java.util.List;
    
    public class BlankDisc implements CompactDisc {
    
    	private String title;
    	private String artist;
    
    	public BlankDisc(String title, String artist) {
    		this.title = title;
    		this.artist = artist;
    	}
    
    	public void play() {
    		System.out.println("Playing " + title + " by " + artist);
    	}
    
    }
    ```
-   配置文件 <br/>
    src/main/resources/META-INF/spring/advancedassembly/injectfromprop/app.properties： <br/>
    ```text
    disc.title=Sgt. Pepper's Lonely Hearts Club Band...
    disc.artist=The Beatles...
    ```
-   装配配置类 <br/>
    com.lfqy.trial.spring.bean.advancedassembly.injectfromprop.InjectFromPropConfig： <br/>
    ```java
    package com.lfqy.trial.spring.bean.advancedassembly.injectfromprop;
    
    import com.lfqy.trial.spring.bean.advancedassembly.injectfromprop.impl.BlankDisc;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.context.annotation.PropertySource;
    import org.springframework.core.env.Environment;
    
    @Configuration
    @PropertySource("classpath:META-INF/spring/advancedassembly/injectfromprop/app.properties")
    public class InjectFromPropConfig {
        @Autowired
        Environment env;
        @Bean(name="disc1")
        public BlankDisc discx(){
    	return new BlankDisc(
    		env.getProperty("disc.title"),//获取配置文件中的配置项
    		env.getProperty("disc.artist")
    		);
        }
        @Bean(name="disc2")
        public BlankDisc discy(){
    	return new BlankDisc(
    		env.getProperty("disc.title2", "Default Title"),//获取配置文件中的配置项，给定一个缺省的默认值
    		env.getProperty("disc.artist2", "Default Artist")
    		);
        }
    }
    ```
    这个例子中，添加了 `@PropertySource("classpath:META-INF/spring/advancedassembly/injectfromprop/app.properties")` 之后，配置文件的内容应该会被注入到名为env的bean中，这样，后面就可以通过这个bean获取配置项的值。 <br/>
-   运行测试类 <br/>
    com.lfqy.trial.spring.bean.advancedassembly.injectfromprop.InjectFromPropConfigMain： <br/>
    ```java
    package com.lfqy.trial.spring.bean.advancedassembly.injectfromprop;
    
    import com.lfqy.trial.spring.bean.advancedassembly.injectfromprop.intrfc.CompactDisc;
    import org.springframework.context.ApplicationContext;
    import org.springframework.context.annotation.AnnotationConfigApplicationContext;
    
    public class InjectFromPropConfigMain {
        public static void main(String []args){
    	ApplicationContext ac = new AnnotationConfigApplicationContext(InjectFromPropConfig.class);
    	CompactDisc cd1 = (CompactDisc) ac.getBean("disc1");
    	cd1.play();
    	CompactDisc cd2 = (CompactDisc) ac.getBean("disc2");
    	cd2.play();
        }
    }
    ```
    这个类的运行结果如下： <br/>
    ```text
    Playing Sgt. Pepper's Lonely Hearts Club Band... by The Beatles...
    Playing Default Title by Default Artist
    
    Process finished with exit code 0
    ```


### 通过属性占位符注入配置文件配置项的值 {#通过属性占位符注入配置文件配置项的值}

-   接口类 <br/>
    com.lfqy.trial.spring.bean.advancedassembly.propplaceholder.intrfc.CompactDisc： <br/>
    ```java
    package com.lfqy.trial.spring.bean.advancedassembly.propplaceholder.intrfc;
    
    public interface CompactDisc {
        void play();
    }
    ```
-   属性配置文件 <br/>
    src/main/resources/META-INF/spring/advancedassembly/propplaceholder/app.properties： <br/>
    ```text
    disc.title=Sgt. Pepper's Lonely Hearts Club Band...
    disc.artist=The Beatles...
    ```
-   接口实现类 <br/>
    com.lfqy.trial.spring.bean.advancedassembly.propplaceholder.impl.BlankDisc： <br/>
    ```java { linenos=true, anchorlinenos=true, lineanchors=org-coderef--a3434b }
    package com.lfqy.trial.spring.bean.advancedassembly.propplaceholder.impl;
    
    import com.lfqy.trial.spring.bean.advancedassembly.propplaceholder.intrfc.CompactDisc;
    import org.springframework.beans.factory.annotation.Value;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.PropertySource;
    import org.springframework.context.support.PropertySourcesPlaceholderConfigurer;
    import org.springframework.stereotype.Component;
    
    @Component
    @PropertySource("classpath:META-INF/spring/advancedassembly/propplaceholder/app.properties")
    public class BlankDisc implements CompactDisc {
    	@Value("${disc.title}")
    	private String title;
    
    	@Value("${disc.artist}")
    	private String artist;
    	//Spring 4.3版本之前，必须要添加一个PropertySourcesPlaceholderConfigurer类型的bean。
    	//4.3之后，就可以直接使用，因为spring会使用默认的DefaultPropertySourceFactory解析。
    	@Bean
    	public static PropertySourcesPlaceholderConfigurer propertySourcesPlaceholderConfigurer(){
    		return new PropertySourcesPlaceholderConfigurer();
    	}
    
    	public void play() {
    		System.out.println("Playing " + title + " by " + artist);
    	}
    
    }
    ```
    这里需要注意下，对于spring 4.3之前的版本，[这里必须要添加一个PropertySourcesPlaceholderConfigurer类型的bean](#org-coderef--a3434b-21)。4.3之后，就可以直接使用，因为spring会使用默认的DefaultPropertySourceFactory解析[^fn:6]。用 `5.1.6.RELEASE` 版本的spring试了下，确实没有这个bean可以正常运行。 <br/>
-   装配配置类 <br/>
    com.lfqy.trial.spring.bean.advancedassembly.propplaceholder.PropPlaceHolderConfig： <br/>
    ```java
    package com.lfqy.trial.spring.bean.advancedassembly.propplaceholder;
    
    import org.springframework.context.annotation.ComponentScan;
    import org.springframework.context.annotation.Configuration;
    
    @Configuration
    @ComponentScan(basePackageClasses= PropPlaceHolderConfig.class)//扫描InjectFromPropertiesConfig所在包及其子包
    public class PropPlaceHolderConfig {
    }
    ```
    实际上，这个类也就是为了配置下ComponentScan。 <br/>
-   运行测试类 <br/>
    com.lfqy.trial.spring.bean.advancedassembly.propplaceholder.PropPlaceHolderConfigMain： <br/>
    ```java
    package com.lfqy.trial.spring.bean.advancedassembly.propplaceholder;
    
    import com.lfqy.trial.spring.bean.advancedassembly.propplaceholder.intrfc.CompactDisc;
    import org.springframework.context.ApplicationContext;
    import org.springframework.context.annotation.AnnotationConfigApplicationContext;
    
    public class PropPlaceHolderConfigMain {
        public static void main(String []args){
    	ApplicationContext ac = new AnnotationConfigApplicationContext(PropPlaceHolderConfig.class);
    	CompactDisc cd1 = (CompactDisc) ac.getBean("blankDisc");
    	cd1.play();
        }
    }
    ```
    运行结果如下： <br/>
    ```text
    Playing Sgt. Pepper's Lonely Hearts Club Band... by The Beatles...
    
    Process finished with exit code 0
    ```

这个例子中，装配配置类和运行测试类的内容可以都放到运行测试类中，如下com.lfqy.trial.spring.bean.advancedassembly.propplaceholder.PropPlaceHolderConfigMain： <br/>

```java
package com.lfqy.trial.spring.bean.advancedassembly.propplaceholder;

import com.lfqy.trial.spring.bean.advancedassembly.propplaceholder.intrfc.CompactDisc;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan(basePackageClasses= PropPlaceHolderConfigMain.class)//扫描InjectFromPropertiesConfig所在包及其子包
public class PropPlaceHolderConfigMain {
    public static void main(String []args){
	ApplicationContext ac = new AnnotationConfigApplicationContext(PropPlaceHolderConfigMain.class);
	CompactDisc cd1 = (CompactDisc) ac.getBean("blankDisc");
	cd1.play();
    }
}
```

这样，就可以删除 `com.lfqy.trial.spring.bean.advancedassembly.propplaceholder.PropPlaceHolderConfig` ，运行结果一样。 <br/>


### 使SpEL进行装配 {#使spel进行装配}

SpEL，Spring语言表达式(Spring Expression Language)，能够以一种强大和简洁的方式将值装配到bean属性和构造函数参数中，在这个过程中使用到的表达式会在运行时进行计算[^fn:3]。支持如下特性： <br/>

-   使用bean的ID来引用bean <br/>
-   调用方法和访问对象的属性 <br/>
-   对值进行算术、关系和逻辑运算 <br/>
-   正则表达式匹配 <br/>
-   集合操作 <br/>

如下是一个通过SpEL表达式进行装配的例子： <br/>

-   接口类 <br/>
    com.lfqy.trial.spring.bean.advancedassembly.spel.intrfc.CompactDisc： <br/>
    ```java
    package com.lfqy.trial.spring.bean.advancedassembly.spel.intrfc;
    
    public interface CompactDisc {
        void play();
    }
    ```
-   接口实现类 <br/>
    com.lfqy.trial.spring.bean.advancedassembly.spel.impl.BlankDisc： <br/>
    ```java
    package com.lfqy.trial.spring.bean.advancedassembly.spel.impl;
    
    import com.lfqy.trial.spring.bean.advancedassembly.spel.intrfc.CompactDisc;
    
    import java.util.List;
    
    public class BlankDisc implements CompactDisc {
    
    	private String title;
    	private String artist;
    
    	public BlankDisc(String title, String artist) {
    		this.title = title;
    		this.artist = artist;
    	}
    
    	public String getTitle() {
    		return title;
    	}
    
    	public void setTitle(String title) {
    		this.title = title;
    	}
    
    	public String getArtist() {
    		return artist;
    	}
    
    	public void setArtist(String artist) {
    		this.artist = artist;
    	}
    
    	public void play() {
    		System.out.println("Playing " + title + " by " + artist);
    	}
    
    }
    ```
-   其他bean类 <br/>
    这些类用于创建bean时的需要： <br/>
    -   DiscSpEL类 <br/>
        com.lfqy.trial.spring.bean.advancedassembly.spel.other.DiscSpEL： <br/>
        ```java
        package com.lfqy.trial.spring.bean.advancedassembly.spel.other;
        
        public class DiscSpEL {
        
        	private final String title;
        	private final String artist;
        
        	public DiscSpEL(String title, String artist) {
        		this.title = title;
        		this.artist = artist;
        	}
        
        	public String getTitle() {
        		return title;
        	}
        
        	public String getArtist() {
        		return artist;
        	}
        
        	public void show() {
        		System.out.println("Show " + title + " by " + artist);
        	}
        }
        ```
    -   SystemProp类 <br/>
        com.lfqy.trial.spring.bean.advancedassembly.spel.other.SystemProp： <br/>
        ```java
        package com.lfqy.trial.spring.bean.advancedassembly.spel.other;
        
        public class SystemProp {
        
        	private String javaVersion;
        	private String javaHome;
        
        	public SystemProp(String javaVersion, String javaHome) {
        		this.javaVersion = javaVersion;
        		this.javaHome = javaHome;
        	}
        
        	public String getJavaVersion() {
        		return javaVersion;
        	}
        
        	public String getJavaHome() {
        		return javaHome;
        	}
        
        	@Override
        	public String toString() {
        		return "SystemPropertiesBean [javaVersion=" + javaVersion + ", javaHome=" + javaHome + "]";
        	}
        }
        ```
-   SpEL装配配置类 <br/>
    com.lfqy.trial.spring.bean.advancedassembly.spel.SpELConfig： <br/>
    ```java { linenos=true, anchorlinenos=true, lineanchors=org-coderef--dfa889 }
    package com.lfqy.trial.spring.bean.advancedassembly.spel;
    
    import com.lfqy.trial.spring.bean.advancedassembly.spel.impl.BlankDisc;
    import com.lfqy.trial.spring.bean.advancedassembly.spel.other.DiscSpEL;
    import com.lfqy.trial.spring.bean.advancedassembly.spel.other.SystemProp;
    import org.springframework.beans.factory.annotation.Qualifier;
    import org.springframework.beans.factory.annotation.Value;
    import org.springframework.beans.factory.config.PropertiesFactoryBean;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    import java.io.IOException;
    import java.util.Properties;
    
    @Configuration
    public class SpELConfig {
    
    	@Bean(name="blankDisc0")
    	public BlankDisc blankDisc() {
    		return new BlankDisc("Sgt. Pepper's Lonely Hearts Club Band", "The Beatles");
    	}
    
    	/**
    ​	* 使用SpEL引用beanId为blankDisc的bean并调用其方法
    	 */
    	@Bean(name="blankDiscSpEL1")
    	@Qualifier("blankDiscSpEL1")
    	public DiscSpEL blankDiscSpEL1(@Value("#{blankDisc0.getTitle()}")String title, @Value("#{blankDisc0.getArtist()}")String artist) {
    		return new DiscSpEL(title, artist);
    	}
    
    	/**
    ​	* 使用SpEL读取属性文件内容
    	 */
    	@Bean
    	@Qualifier("blankDiscSpEL2")
    	public DiscSpEL blankDiscSpEL2(@Value("#{props['disc.title']}")String title, @Value("#{props['disc.artist']}")String artist) {
    		return new DiscSpEL(title, artist);
    	}
    
    	/**
    ​	* 定义bean并加装一个自定义属性文件
    	 */
    	@Bean(name="prop")
    	public Properties properties() {
    		Properties properties = new Properties();
    		try {
    			properties.load(this.getClass().getClassLoader().getResourceAsStream("META-INF/spring/advancedassembly/spel/app.properties"));
    		} catch (IOException e) {
    			e.printStackTrace();
    		}
    		return properties;
    	}
    
    	/**
    ​	* 定义bean并加装多个自定义属性文件
    	 */
    	@Bean(name="props")
    	public PropertiesFactoryBean propertiesFactoryBean(@Qualifier("prop")Properties properties) {
    		PropertiesFactoryBean factoryBean = new PropertiesFactoryBean();
    		//加载一个属性文件
    		factoryBean.setProperties(properties);
    
    //		/*
    //		 * 用来加装多个属性文件，其中属性名称相同的，后加载的会覆盖之前加载的
    //		 */
    //		Properties properties1 = new Properties();
    //		Properties properties2 = new Properties();
    //		try {
    //			properties1.load(this.getClass().getClassLoader().getResourceAsStream("META-INF/spring/advancedassembly/spel/app1.properties"));
    //			properties2.load(this.getClass().getClassLoader().getResourceAsStream("META-INF/spring/advancedassembly/spel/app2.properties"));
    //		} catch (IOException e) {
    //			e.printStackTrace();
    //		}
    //		factoryBean.setPropertiesArray(properties1, properties2);
    
    		return factoryBean;
    	}
    
    	/**
    ​	* 使用SpEL读取系统属性。systemProperties是预定义的bean的id，等同于System.getProperties()
    	 */
    	@Bean
    	public SystemProp systemPropertiesBean(
    			@Value("#{systemProperties['java.version']}")String javaVersion,
    			@Value("#{systemProperties['java.home']}")String javaHome) {
    		return new SystemProp(javaVersion, javaHome);
    	}
    }
    ```
    这个测试代码简单说明： <br/>
    
    -   首先[创建了一个简单的bean](#org-coderef--dfa889-17)，命名为blankDisc0。 <br/>
    -   然后，通过SpEL表达式读取blankDisc0这个bean的属性，[创建了blankDiscSpEL1这个bean](#org-coderef--dfa889-25)。 <br/>
    -   接下来，通过读取配置文件中的属性，[创建了blankDiscSpEL2这个bean](#org-coderef--dfa889-35)。 <br/>
        在读取配置文件的时候，先[将配置文件的内容加载到prop这个bean](#org-coderef--dfa889-43)中，然后又[将这个bean的内容给到了props](#org-coderef--dfa889-57)这个bean。这里需要注意，props这个bean在入参中加了限定 `@Qualifier("prop")` ，指定将prop这个bean注入参数，否则会报错： <br/>
        ```text
        Exception in thread "main" org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'props' defined in com.lfqy.trial.spring.bean.advancedassembly.spel.SpELConfig: Unsatisfied dependency expressed through constructor argument with index 0 of type [java.util.Properties]: : No qualifying bean of type [java.util.Properties] is defined: expected single matching bean but found 2: prop,systemProperties; nested exception is org.springframework.beans.factory.NoUniqueBeanDefinitionException: No qualifying bean of type [java.util.Properties] is defined: expected single matching bean but found 2: prop,systemProperties
        	at org.springframework.beans.factory.support.ConstructorResolver.createArgumentArray(ConstructorResolver.java:749)
        	at org.springframework.beans.factory.support.ConstructorResolver.instantiateUsingFactoryMethod(ConstructorResolver.java:464)
        	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.instantiateUsingFactoryMethod(AbstractAutowireCapableBeanFactory.java:1111)
        	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBeanInstance(AbstractAutowireCapableBeanFactory.java:1006)
        	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:504)
        	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:476)
        	at org.springframework.beans.factory.support.AbstractBeanFactory$1.getObject(AbstractBeanFactory.java:303)
        	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:230)
        	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:299)
        	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:194)
        	at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingletons(DefaultListableBeanFactory.java:743)
        	at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:757)
        	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:480)
        	at org.springframework.context.annotation.AnnotationConfigApplicationContext.<init>(AnnotationConfigApplicationContext.java:84)
        	at com.lfqy.trial.spring.bean.advancedassembly.spel.SpELConfigMain.main(SpELConfigMain.java:11)
        Caused by: org.springframework.beans.factory.NoUniqueBeanDefinitionException: No qualifying bean of type [java.util.Properties] is defined: expected single matching bean but found 2: prop,systemProperties
        	at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1061)
        	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:949)
        	at org.springframework.beans.factory.support.ConstructorResolver.resolveAutowiredArgument(ConstructorResolver.java:813)
        	at org.springframework.beans.factory.support.ConstructorResolver.createArgumentArray(ConstructorResolver.java:741)
        	... 14 more
        
        Process finished with exit code 1
        ```
        报错的原因比较明显，是因为自动注入发现了二义性。 <br/>
        注意，[注释掉的代码](#org-coderef--dfa889-63)，可以加载多个配置文件，不过这里没有用到多个配置的加载，所以注掉了。看了下，PropertiesFactoryBean这个类的api中，好像只有将多个properties实例组成的数组或者单个的properties实例直接set，没法往数组中添加。 <br/>
    -   最后，通过SpEL表达式[读取系统属性注入到systemPropertiesBean](#org-coderef--dfa889-82)这个bean。 <br/>
-   SpEL装配测试类 <br/>
    com.lfqy.trial.spring.bean.advancedassembly.spel.SpELConfigMain： <br/>
    ```java
    package com.lfqy.trial.spring.bean.advancedassembly.spel;
    
    import com.lfqy.trial.spring.bean.advancedassembly.spel.intrfc.CompactDisc;
    import com.lfqy.trial.spring.bean.advancedassembly.spel.other.DiscSpEL;
    import com.lfqy.trial.spring.bean.advancedassembly.spel.other.SystemProp;
    import org.springframework.context.ApplicationContext;
    import org.springframework.context.annotation.AnnotationConfigApplicationContext;
    
    public class SpELConfigMain {
        public static void main(String []args){
    	ApplicationContext ac = new AnnotationConfigApplicationContext(SpELConfig.class);
    	System.out.println("0、测试java配置显示调用构造方法注入的例子");
    	CompactDisc blankDisc0 = (CompactDisc) ac.getBean("blankDisc0");
    	blankDisc0.play();
    	System.out.println("1、测试用spel表达式读取bean属性注入的例子");
    	DiscSpEL discSpEL1 = (DiscSpEL) ac.getBean("blankDiscSpEL1");
    	discSpEL1.show();
    	System.out.println("2、测试用spel表达式读取配置文件注入的例子");
    	DiscSpEL discSpEL2 = (DiscSpEL) ac.getBean("blankDiscSpEL2");
    	discSpEL2.show();
    	System.out.println("3、测试用spel表达式读取配置文件注入的例子");
    	SystemProp spb = (SystemProp) ac.getBean("systemPropertiesBean");
    	System.out.println(spb);
        }
    }
    ```
    运行结果如下： <br/>
    ```text
    0、测试java配置显示调用构造方法注入的例子
    Playing Sgt. Pepper's Lonely Hearts Club Band... by The Beatles...
    1、测试用spel表达式读取bean属性注入的例子
    Show Sgt. Pepper's Lonely Hearts Club Band... by The Beatles...
    2、测试用spel表达式读取配置文件注入的例子
    Show Sgt. Pepper's Lonely Hearts Club Band... ... by The Beatles... ...
    3、测试用spel表达式读取配置文件注入的例子
    SystemPropertiesBean [javaVersion=1.8.0_181, javaHome=/Library/Java/JavaVirtualMachines/jdk1.8.0_181.jdk/Contents/Home/jre]
    
    Process finished with exit code 0
    ```


## Spring切面 {#spring切面}

切面提供了取代继承和委托的另一种方案，在一个地方定义通用功能，通过声明的方式定义这个功能在何处应用，不需要修改受影响的类。好处有两个： <br/>

-   每个关注点都在一个地方，而不是分散在多处的代码。 <br/>
-   服务模块只需要关注本身的业务逻辑，更加清晰简介，其他非业务相关的内容可以全部放在切面中实现。 <br/>


### 说明 {#说明}

本章节中代码参考[^fn:3]，项目的完整目录结构如下： <br/>

```text
spring-bean $ tree .
.
├── pom.xml
└── src
    ├── main
    │   ├── java
    │   │   └── com
    │   │       └── lfqy
    │   │           └── trial
    │   │               └── spring
    │   │                   ├── aop
    │   │                   │   ├── advice
    │   │                   │   │   ├── PerformanceConfig.java
    │   │                   │   │   ├── PerformanceConfigMain.java
    │   │                   │   │   ├── aspect
    │   │                   │   │   │   └── Audience.java
    │   │                   │   │   ├── impl
    │   │                   │   │   │   └── SingPerformance.java
    │   │                   │   │   └── intrfc
    │   │                   │   │       └── Performance.java
    │   │                   │   ├── paramadvice
    │   │                   │   │   ├── BlankDisc.java
    │   │                   │   │   ├── TrackCounter.java
    │   │                   │   │   ├── TrackCounterConfig.java
    │   │                   │   │   └── TrackCounterConfigMain.java
    │   │                   │   └── proxyadvice
    │   │                   │       ├── ProxyAdviceConfig.java
    │   │                   │       ├── ProxyAdviceConfigMain.java
    │   │                   │       ├── encore
    │   │                   │       │   ├── DefaultEncoreable.java
    │   │                   │       │   ├── Encoreable.java
    │   │                   │       │   └── EncoreableIntroducer.java
    │   │                   │       ├── impl
    │   │                   │       │   └── SingPerformance.java
    │   │                   │       └── intrfc
    │   │                   │           └── Performance.java
    │   │                   └── bean
    │   │                       ├── advancedassembly
    │   │                       │   ├── conditional
    │   │                       │   │   ├── ConditionalConfig.java
    │   │                       │   │   ├── ConditionalConfigMain.java
    │   │                       │   │   ├── MagicClass.java
    │   │                       │   │   └── MagicExistsCondition.java
    │   │                       │   ├── injectfromprop
    │   │                       │   │   ├── InjectFromPropConfig.java
    │   │                       │   │   ├── InjectFromPropConfigMain.java
    │   │                       │   │   ├── impl
    │   │                       │   │   │   └── BlankDisc.java
    │   │                       │   │   └── intrfc
    │   │                       │   │       └── CompactDisc.java
    │   │                       │   ├── propplaceholder
    │   │                       │   │   ├── PropPlaceHolderConfig.java
    │   │                       │   │   ├── PropPlaceHolderConfigMain.java
    │   │                       │   │   ├── impl
    │   │                       │   │   │   └── BlankDisc.java
    │   │                       │   │   └── intrfc
    │   │                       │   │       └── CompactDisc.java
    │   │                       │   ├── scope
    │   │                       │   │   ├── PrototypeC.java
    │   │                       │   │   ├── ScopeConfigMain.java
    │   │                       │   │   ├── Singleton1C.java
    │   │                       │   │   └── Singleton2C.java
    │   │                       │   └── spel
    │   │                       │       ├── SpELConfig.java
    │   │                       │       ├── SpELConfigMain.java
    │   │                       │       ├── impl
    │   │                       │       │   └── BlankDisc.java
    │   │                       │       ├── intrfc
    │   │                       │       │   └── CompactDisc.java
    │   │                       │       └── other
    │   │                       │           ├── DiscSpEL.java
    │   │                       │           └── SystemProp.java
    │   │                       ├── autoconfig
    │   │                       │   ├── AutoConfigByJavaMain.java
    │   │                       │   ├── AutoConfigByXmlMain.java
    │   │                       │   ├── ComponentScanPoint.java
    │   │                       │   ├── config
    │   │                       │   │   └── CDPlayerConfig.java
    │   │                       │   ├── impl
    │   │                       │   │   ├── CDPlayer.java
    │   │                       │   │   └── SgtPeppers.java
    │   │                       │   └── intrfc
    │   │                       │       ├── CompactDisc.java
    │   │                       │       └── MediaPlayer.java
    │   │                       ├── javaconfig
    │   │                       │   ├── JavaConfigMain.java
    │   │                       │   ├── config
    │   │                       │   │   └── CDPlayerConfig.java
    │   │                       │   ├── impl
    │   │                       │   │   ├── CDPlayer.java
    │   │                       │   │   └── SgtPeppers.java
    │   │                       │   └── intrfc
    │   │                       │       ├── CompactDisc.java
    │   │                       │       └── MediaPlayer.java
    │   │                       ├── mixedconfig
    │   │                       │   ├── JavaMixed2XmlConfigMain.java
    │   │                       │   ├── XmlMixed2JavaConfigMain.java
    │   │                       │   ├── config
    │   │                       │   │   ├── java2xml
    │   │                       │   │   │   └── CDConfig.java
    │   │                       │   │   └── xml2java
    │   │                       │   │       └── CDPlayerConfig.java
    │   │                       │   ├── impl
    │   │                       │   │   ├── BlankDisc.java
    │   │                       │   │   ├── CDPlayer.java
    │   │                       │   │   └── SgtPeppers.java
    │   │                       │   └── intrfc
    │   │                       │       ├── CompactDisc.java
    │   │                       │       └── MediaPlayer.java
    │   │                       └── xmlconfig
    │   │                           ├── XmlConfigMain.java
    │   │                           ├── impl
    │   │                           │   ├── CDPlayer.java
    │   │                           │   └── SgtPeppers.java
    │   │                           └── intrfc
    │   │                               ├── CompactDisc.java
    │   │                               └── MediaPlayer.java
    │   └── resources
    │       └── META-INF
    │           └── spring
    │               ├── advancedassembly
    │               │   ├── conditional
    │               │   │   └── app.properties
    │               │   ├── injectfromprop
    │               │   │   └── app.properties
    │               │   ├── propplaceholder
    │               │   │   └── app.properties
    │               │   └── spel
    │               │       └── app.properties
    │               ├── autoconfig
    │               │   └── autoconfig.xml
    │               ├── mixedconfig
    │               │   ├── java2xml
    │               │   │   └── mixedconfig-cdplayer.xml
    │               │   └── xml2java
    │               │       └── mixedconfig-cd.xml
    │               └── xmlconfig
    │                   └── xmlconfig.xml
    └── test
	└── java
	    └── com
		└── lfqy
		    └── trial
			└── spring
			    └── bean
				└── mixedconfig
				    └── Java2XmlConfigTest.java

69 directories, 76 files
spring-bean $
```

项目的pom文件，见pom.xml： <br/>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
	 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.lfqy.trial</groupId>
    <artifactId>spring-bean</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
	<maven.compiler.source>8</maven.compiler.source>
	<maven.compiler.target>8</maven.compiler.target>
	<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	<!-- Spring 4.3版本之前，必须要添加一个PropertySourcesPlaceholderConfigurer类型的bean。4.3之后，就可以直接使用，因为spring会使用默认的DefaultPropertySourceFactory解析。 -->
	<!--        <spring.version>5.1.6.RELEASE</spring.version>-->
	<spring.version>4.1.4.RELEASE</spring.version>
	<aspectj.version>1.7.2</aspectj.version>
    </properties>
    <dependencies>
	<!-- Spring Core -->
	<!-- http://mvnrepository.com/artifact/org.springframework/spring-core -->
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-core</artifactId>
	    <version>${spring.version}</version>
	</dependency>
	<!-- Spring Context -->
	<!-- http://mvnrepository.com/artifact/org.springframework/spring-context -->
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-context</artifactId>
	    <version>${spring.version}</version>
	</dependency>


	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-test</artifactId>
	    <version>${spring.version}</version>
	    <scope>test</scope>
	</dependency>

	<dependency>
	    <groupId>org.aspectj</groupId>
	    <artifactId>aspectjweaver</artifactId>
	    <version>${aspectj.version}</version>
	</dependency>

	<dependency>
	    <groupId>junit</groupId>
	    <artifactId>junit</artifactId>
	    <version>4.13.1</version>
	    <scope>test</scope>
	</dependency>

	<dependency>
	    <groupId>org.mockito</groupId>
	    <artifactId>mockito-core</artifactId>
	    <version>2.23.4</version>
	    <scope>test</scope>
	</dependency>

	<dependency>
	    <groupId>org.apache.commons</groupId>
	    <artifactId>commons-lang3</artifactId>
	    <version>3.1</version>
	</dependency>
    </dependencies>
</project>
```


### AOP术语 {#aop术语}

-   通知(Advice) <br/>
    定义切面是什么以及何时使用。分为如下几类： <br/>
    -   前置通知(Before) <br/>
        在目标方法被调用之前调用通知功能。 <br/>
    -   后置通知(After) <br/>
        在目标方法完成之后调用通知，不关心方法的输出是什么。 <br/>
    -   返回通知(After-returning) <br/>
        在目标方法成功执行之后调用的通知。 <br/>
    -   异常通知(After-throwing) <br/>
        在目标方法抛出异常后调用的通知。 <br/>
    -   环绕通知(Around) <br/>
        通知包裹了被通知的方法，在被通知的方法调用之前和调用之后执行自定义的行为。 <br/>
-   连接点(Join point) <br/>
    应用执行过程中，能够插入切面的点。 <br/>
-   切点(Pointcut) <br/>
    通知所要织入的一个或者多个连接点。 <br/>
-   切面(Aspect) <br/>
    切面是通知和切点的结合。通知和切点定义了切面的全部内容(是什么，在何时、何处完成功能)。 <br/>
-   引入(Introduction) <br/>
    引入允许我们向现有的类添加新方法或者属性。 <br/>
-   织入(Weaving) <br/>
    织入是把切面应用到目标对象并创建新的代理对象的过程。切面在指定的连接点被织入到目标对象中。目标对象的生命周期中，多个阶段可以织入： <br/>
    -   编译期 <br/>
        需要特殊的编译器，比如AspectJ织入编译器。 <br/>
    -   类加载期 <br/>
        特殊的类加载器，在目标类被加载时增强该目标类的字节码。 <br/>
    -   运行期 <br/>
        切面在应用运行的某个时刻被织入，Spring AOP就是以这种方式织入切面的。 <br/>


### 切面简单示例 {#切面简单示例}

-   接口类 <br/>
    见com.lfqy.trial.spring.aop.advice.intrfc.Performance： <br/>
    ```java
    package com.lfqy.trial.spring.aop.advice.intrfc;
    
    public interface Performance {
    	void perform();
    }
    ```
-   接口实现类 <br/>
    见com.lfqy.trial.spring.aop.advice.impl.SingPerformance： <br/>
    ```java
    package com.lfqy.trial.spring.aop.advice.impl;
    
    import com.lfqy.trial.spring.aop.advice.intrfc.Performance;
    import org.springframework.stereotype.Component;
    
     @Component
     public class SingPerformance implements Performance {
    
         @Override
         public void perform() {
    	 System.out.println("Now, performing... @impl SingPerformance");
         }
     }
    ```
-   切面类 <br/>
    ```java
    package com.lfqy.trial.spring.aop.advice.aspect;
    
    import org.aspectj.lang.ProceedingJoinPoint;
    import org.aspectj.lang.annotation.*;
    import org.springframework.stereotype.Component;
    
    @Aspect	//定义切面
    @Component	//同时声明bean
    public class Audience {
    
        //定义切点
        @Pointcut("execution(** com.lfqy.trial.spring.aop.advice.intrfc.Performance.perform(..))")
        public void watch() {}
    
        //前置通知
        @Before("watch()")
        public void silenceCellPhone() {
    	System.out.println("Mobile phone mute. @Audience. Before advice.");
        }
    
        //前置通知
        @Before("watch()")
        public void takeSeats() {
    	System.out.println("Take seat. @Audience. Before advice.");
        }
    
        //后置返回通知
        @AfterReturning("watch()")
        public void applause() {
    	System.out.println("Clap. @Audience. After advice.");
        }
    
        //后置异常通知
        @AfterThrowing("watch()")
        public void demandRefund() {
    	System.out.println("Demanding a refund. @Audience. AfterThrowing Advice.");
        }
    
        //环绕通知，与上面四个通知共用
        @Around("watch()")
        public void watchPerformance(ProceedingJoinPoint joinPoint) {
    	try {
    	    System.out.println("Sliencing cell phones. @Audience. Around advice.");//先于@Before
    	    joinPoint.proceed();
    	    System.out.println("Clap, clap, Clap. @Around. Around advice.");//先于@After
    	} catch (Throwable e) {
    	    System.out.println("Demanding a refund. @Around. Around advice.");//先于@AfterThrowing
    	    throw new RuntimeException(e);//如果不抛出，则不会执行@AfterThrowing
    	}
        }
    }
    ```
    注意，这里切面必须要配置 `@Component` 注解，不然没法正常切入。 <br/>
-   切面简单示例配置类 <br/>
    见com.lfqy.trial.spring.aop.advice.PerformanceConfig： <br/>
    ```java { linenos=true, anchorlinenos=true, lineanchors=org-coderef--3f732f }
    package com.lfqy.trial.spring.aop.advice;
    
    import org.springframework.context.annotation.ComponentScan;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.context.annotation.EnableAspectJAutoProxy;
    
    @Configuration
    @ComponentScan(basePackageClasses = PerformanceConfig.class)
    @EnableAspectJAutoProxy    //启用AspectJ自动代理，切面才不会只是普通的bean                  (enableAspectJAutoProxy)
    public class PerformanceConfig {
    }
    ```
    这里需要[启用AspectJ自动代理](#org-coderef--3f732f-9)，不然切面只是普通的bean。 <br/>
-   切面简单示例运行测试类 <br/>
    见com.lfqy.trial.spring.aop.advice.PerformanceConfigMain： <br/>
    ```java
    package com.lfqy.trial.spring.aop.advice;
    
    import com.lfqy.trial.spring.aop.advice.intrfc.Performance;
    import org.springframework.context.ApplicationContext;
    import org.springframework.context.annotation.AnnotationConfigApplicationContext;
    
    public class PerformanceConfigMain {
        public static void main(String []args){
    	ApplicationContext ac = new AnnotationConfigApplicationContext(PerformanceConfig.class);
    	Performance sp = (Performance) ac.getBean("singPerformance");
    	sp.perform();
        }
    }
    ```
    运行结果如下： <br/>
    ```text
    Sliencing cell phones. @Audience. Around advice.
    Mobile phone mute. @Audience. Before advice.
    Take seat. @Audience. Before advice.
    Now, performing... @impl SingPerformance
    Clap, clap, Clap. @Around. Around advice.
    Clap. @Audience. After advice.
    
    Process finished with exit code 0
    ```


### 传入参数的切面 {#传入参数的切面}

如下是一个例子： <br/>

-   创建bean的类 <br/>
    见com.lfqy.trial.spring.aop.paramadvice.BlankDisc： <br/>
    ```java
    package com.lfqy.trial.spring.aop.paramadvice;
    
    import java.util.List;
    
    public class BlankDisc {
    
    	private String title;
    	private String artist;
    	private List<String> tracks;
    
    	public BlankDisc() {}
    
    	public BlankDisc(String title, String artist, List<String> tracks) {
    		this.title = title;
    		this.artist = artist;
    		this.tracks = tracks;
    	}
    
    	public String getTitle() {
    		return title;
    	}
    
    	public void setTitle(String title) {
    		this.title = title;
    	}
    
    	public String getArtist() {
    		return artist;
    	}
    
    	public void setArtist(String artist) {
    		this.artist = artist;
    	}
    
    	public List<String> getTracks() {
    		return tracks;
    	}
    
    	public void setTracks(List<String> tracks) {
    		this.tracks = tracks;
    	}
    
    	public void playTrack(int trackNum) {
    		System.out.println("Playing " + title + "【"+tracks.get(trackNum-1)+"】 by " + artist);
    	}
    }
    ```
-   切面类 <br/>
    见com.lfqy.trial.spring.aop.paramadvice.TrackCounter： <br/>
    ```java
    package com.lfqy.trial.spring.aop.paramadvice;
    
    import org.aspectj.lang.annotation.Aspect;
    import org.aspectj.lang.annotation.Before;
    import org.aspectj.lang.annotation.Pointcut;
    import org.springframework.stereotype.Component;
    import java.util.HashMap;
    import java.util.Map;
    
    @Component
    @Aspect
    public class TrackCounter {
    
        private Map<Integer, Integer> trackCounts = new HashMap<Integer, Integer>();
    
        //定义切点，通知playTrack方法
        @Pointcut("execution(** com.lfqy.trial.spring.aop.paramadvice.BlankDisc.playTrack(int)) && args(trackNum)")
        public void trackPlayed(int trackNum) {}
    
        //在播放前，为该磁道计数
        @Before("trackPlayed(trackNum)")
        public void countTrack(int trackNum) {
    	int currentCount = getPlayCount(trackNum);
    	trackCounts.put(trackNum, currentCount + 1);
        }
    
        //获取某磁道的播放次数
        public int getPlayCount(int trackNum) {
    	return trackCounts.containsKey(trackNum)?trackCounts.get(trackNum):0;
        }
        public void print(){
    	System.out.println(trackCounts);
        }
    }
    ```
-   切面配置类 <br/>
    见com.lfqy.trial.spring.aop.paramadvice.TrackCounterConfig： <br/>
    ```java
    package com.lfqy.trial.spring.aop.paramadvice;
    
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.ComponentScan;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.context.annotation.EnableAspectJAutoProxy;
    
    import java.util.ArrayList;
    import java.util.List;
    @Configuration
    @ComponentScan(basePackageClasses = TrackCounterConfig.class)
    @EnableAspectJAutoProxy	//开启AspectJ自动代理
    public class TrackCounterConfig {
    
        @Bean
        public BlankDisc blankDisc() {
    	BlankDisc blankDisc = new BlankDisc();
    	blankDisc.setTitle("Shades of Purple");
    	blankDisc.setArtist("M2M");
    	List<String> tracks = new ArrayList<String>();
    	tracks.add("Don't Say You Love Me");
    	tracks.add("The Day You Went Away");
    	tracks.add("Girl in Your Dreams");
    	tracks.add("Mirror Mirror");
    	blankDisc.setTracks(tracks);
    	return blankDisc;
        }
    // 这里创建bean的操作不用了，因为TrackCounter类上已经有了@Component注解
    //    @Bean
    //    public TrackCounter trackCounter() {
    //        return new TrackCounter();
    //    }
    }
    ```
-   运行测试类 <br/>
    见com.lfqy.trial.spring.aop.paramadvice.TrackCounterConfigMain： <br/>
    ```java { linenos=true, anchorlinenos=true, lineanchors=org-coderef--6ca05d }
    package com.lfqy.trial.spring.aop.paramadvice;
    
    import org.springframework.context.ApplicationContext;
    import org.springframework.context.annotation.AnnotationConfigApplicationContext;
    
    public class TrackCounterConfigMain {
        public static void main(String []args){
    	ApplicationContext ac = new AnnotationConfigApplicationContext(TrackCounterConfig.class);
    	BlankDisc blankDisc = (BlankDisc) ac.getBean("blankDisc");
    	blankDisc.playTrack(1);
    	blankDisc.playTrack(2);
    	blankDisc.playTrack(3);
    	blankDisc.playTrack(3);
    	blankDisc.playTrack(4);
    	blankDisc.playTrack(4);
    
    	TrackCounter tc = (TrackCounter) ac.getBean("trackCounter");
    	tc.print();
        }
    }
    ```
    运行结果如下： <br/>
    ```text
    Playing Shades of Purple【Don't Say You Love Me】 by M2M
    Playing Shades of Purple【The Day You Went Away】 by M2M
    Playing Shades of Purple【Girl in Your Dreams】 by M2M
    Playing Shades of Purple【Girl in Your Dreams】 by M2M
    Playing Shades of Purple【Mirror Mirror】 by M2M
    Playing Shades of Purple【Mirror Mirror】 by M2M
    {1=1, 2=1, 3=2, 4=2}
    
    Process finished with exit code 0
    ```
    [这里](#org-coderef--6ca05d-17)看出，切面其实也是一个bean，获取方式和一般的bean一样。 <br/>


### 通过切面引入新功能 {#通过切面引入新功能}

如下示例： <br/>

-   接口类 <br/>
    见com.lfqy.trial.spring.aop.proxyadvice.intrfc.Performance： <br/>
    ```java
    package com.lfqy.trial.spring.aop.proxyadvice.intrfc;
    
    public interface Performance {
    	void perform();
    }
    ```
-   接口实现类 <br/>
    见com.lfqy.trial.spring.aop.proxyadvice.impl.SingPerformance： <br/>
    ```java
    package com.lfqy.trial.spring.aop.proxyadvice.impl;
    
    import com.lfqy.trial.spring.aop.proxyadvice.intrfc.Performance;
    import org.springframework.stereotype.Component;
    
     @Component
     public class SingPerformance implements Performance {
    
         @Override
         public void perform() {
    	 System.out.println("Now, performing... @impl SingPerformance");
         }
     }
    ```
-   新功能引入相关类 <br/>
    -   新功能接口 <br/>
        见com.lfqy.trial.spring.aop.proxyadvice.encore.Encoreable： <br/>
        ```java
        package com.lfqy.trial.spring.aop.proxyadvice.encore;
        
        /**
        ​ * encore的意思是表演结束后要求返场。
         */
        public interface Encoreable {
        
        	void performEncore();
        }
        ```
    -   新功能接口默认实现类 <br/>
        见com.lfqy.trial.spring.aop.proxyadvice.encore.DefaultEncoreable： <br/>
        ```java
        package com.lfqy.trial.spring.aop.proxyadvice.encore;
        
        /**
        ​ * 默认代理类
         */
        public class DefaultEncoreable implements Encoreable {
        
        	@Override
        	public void performEncore() {
        		System.out.println("Back show, again.");
        	}
        
        }
        ```
    -   新功能引入配置类 <br/>
        见com.lfqy.trial.spring.aop.proxyadvice.encore.EncoreableIntroducer： <br/>
        ```java
        package com.lfqy.trial.spring.aop.proxyadvice.encore;
        
         import org.aspectj.lang.annotation.Aspect;
         import org.aspectj.lang.annotation.DeclareParents;
         import org.springframework.stereotype.Component;
        
        /**
        ​  * 通过注解引入新功能
          */
        @Component
         @Aspect
         public class EncoreableIntroducer {
        
             /**
        ​      * @DeclareParents 所标注的静态属性指明了要引入的接口
              *
        ​      * 将接口Encoreable引入到Performance的bean中
        ​      * 1.value指定哪种类型的bean要引入改接口，本例中即实现了Performance的类型（“+”表示Performance的所有子类，而不是其本身）
        ​      * 2.defaultImpl表示为被引入接口的实现类
        ​      * 3.静态属性指明了要引入的接口，这里是encoreable接口
              */
             @DeclareParents(value="com.lfqy.trial.spring.aop.proxyadvice.intrfc.Performance+",
        	     defaultImpl=DefaultEncoreable.class)
             public static Encoreable encoreable;
         }
        ```
-   新功能引入bean配置类 <br/>
    见com.lfqy.trial.spring.aop.proxyadvice.ProxyAdviceConfig： <br/>
    ```java
    package com.lfqy.trial.spring.aop.proxyadvice;
    
    import org.springframework.context.annotation.ComponentScan;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.context.annotation.EnableAspectJAutoProxy;
    
    @Configuration
    @ComponentScan(basePackageClasses = ProxyAdviceConfig.class)
    @EnableAspectJAutoProxy    //启用AspectJ自动代理，切面才不会只是普通的bean
    public class ProxyAdviceConfig {
    }
    ```
-   新功能引入运行测试类 <br/>
    见com.lfqy.trial.spring.aop.proxyadvice.ProxyAdviceConfigMain： <br/>
    ```java { linenos=true, anchorlinenos=true, lineanchors=org-coderef--7076df }
    package com.lfqy.trial.spring.aop.proxyadvice;
    
    import com.lfqy.trial.spring.aop.proxyadvice.encore.Encoreable;
    import com.lfqy.trial.spring.aop.proxyadvice.intrfc.Performance;
    import org.springframework.context.ApplicationContext;
    import org.springframework.context.annotation.AnnotationConfigApplicationContext;
    
    public class ProxyAdviceConfigMain {
        public ProxyAdviceConfigMain() {
        }
    
        public static void main(String []args){
    	ApplicationContext ac = new AnnotationConfigApplicationContext(ProxyAdviceConfig.class);
    	Performance sp = (Performance) ac.getBean("singPerformance");
    	sp.perform();
    	Encoreable encoreable = (Encoreable) sp;
    	encoreable.performEncore();
        }
    }
    ```
    运行结果如下： <br/>
    ```text
    Now, performing... @impl SingPerformance
    Back show, again.
    
    Process finished with exit code 0
    ```
    从[这里](#org-coderef--7076df-16)可以看出，尽管声明了 `SingPerformance` 和 `EncoreableIntroducer` 两个bean，这样配置之后，在获取名为 `singPerformance` 的bean时，拿到的实际是一个包含代理的bean。把它转成什么接口类型，就可以调用什么接口的方法。再想获取原来那个不带代理的纯 `SingPerformance` bean，应该是拿不到了。 <br/>

[^fn:1]: [SpringBoot 介绍](https://www.cainiaoplus.com/springboot/springboot-intro.html) <br/>
[^fn:2]: [Spring IoC 容器](https://www.w3cschool.cn/wkspring/f8pc1hae.html) <br/>
[^fn:3]: 《Spring实战》，第4版，Craig Walls著，张卫滨译，书中的完整代码在github有[^fn:4]。 <br/>
[^fn:4]: [lzco/springiA4_code](https://github.com/lzco/springiA4_code/tree/master) <br/>
[^fn:5]: [Spring中Bean的五个作用域](https://www.cnblogs.com/goody9807/p/7472127.html) <br/>
[^fn:6]: [Spring高级之注解@PropertySource详解（超详细）](https://blog.csdn.net/qq_40837310/article/details/106587158)  <br/>