+++
title = "java异步执行简单示例"
author = ["Cheng Xia"]
date = 2024-03-13
draft = false
+++

<div class="ox-hugo-toc toc">

<div class="heading">Table of Contents</div>

- [说明](#说明)
- [同步执行示例](#同步执行示例)
- [异步执行修改](#异步执行修改)
- [写在后面](#写在后面)

</div>
<!--endtoc-->



## 说明 {#说明}

在java代码中，有时多个步骤顺序执行，但是。他们之间并没有严格地先后关系，只是“顺手”一先一后写在了一起。这样，这些步骤先后执行，会导致执行的时间是各个步骤之和，耗时过长。 <br/>
为了解决这个问题，可以将这写方法写成异步执行的方式[^fn:1]。 <br/>


## 同步执行示例 {#同步执行示例}

如下是一个同步执行的例子，com/lfqy/trial/asyn/exec/SyncSerialExec.java： <br/>

```java
package com.lfqy.trial.asyn.exec;

import java.text.SimpleDateFormat;
import java.util.Date;

public class SyncSerialExec {
    public String runTask1() throws InterruptedException {
	System.out.println(" --- task 1 start --- ");
	Thread.sleep(3000);
	System.out.println(" --- task 1 finish ---");
	return "Result of task 1!";
    }
    public String runTask2() throws InterruptedException {
	System.out.println(" --- task 2 start --- ");
	Thread.sleep(3000);
	System.out.println(" --- task 2 finish ---");
	return "Result of task 2!";
    }

    public static void main(String []args){
	SyncSerialExec s = new SyncSerialExec();
	try {
	    SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss.SSS");
	    Date date1 = new Date();
	    System.out.println("Program begin at：" + sdf.format(date1));

	    System.out.println("Task 1 result：" + s.runTask1());
	    System.out.println("Task 2 result：" + s.runTask2());

	    Date date2 = new Date();
	    System.out.println("Program end at：" + sdf.format(date2));
	    System.out.println("It takes " + (date2.getTime() - date1.getTime()) / 1000 + " seconds." );
	}catch (Exception e){
	    e.printStackTrace();
	}
    }
}
```

这个代码执行的结果如下： <br/>

```text
Program begin at：2024-03-13 23:53:07.904
 --- task 1 start --- 
 --- task 1 finish ---
Task 1 result：Result of task 1!
 --- task 2 start --- 
 --- task 2 finish ---
Task 2 result：Result of task 2!
Program end at：2024-03-13 23:53:13.914
It takes 6 seconds.

Process finished with exit code 0
```

每个任务方法耗时3秒，顺序执行整个耗时就是6秒。 <br/>


## 异步执行修改 {#异步执行修改}

可以将前面的例子，改成异步执行的方式，每个任务都通过一个线程池异步执行，最后获取执行的结果。 <br/>
com/lfqy/trial/asyn/exec/AsynParellelExec.java，如下： <br/>

```java
package com.lfqy.trial.asyn.exec;

import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.concurrent.*;

public class AsynParellelExec {



    public static void main(String []args){

	ExecutorService executor = Executors.newFixedThreadPool(2);

	SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss.SSS");
	Date date1 = new Date();
	System.out.println("Program begin at：" + sdf.format(date1));
	Future<String> future1 = executor.submit(new Callable<String>() {
	    public String runTask1() throws InterruptedException {
		System.out.println(" --- task 1 start --- ");
		Thread.sleep(3000);
		System.out.println(" --- task 1 finish ---");
		return "Result of task 1!";
	    }
	    @Override
	    public String call() throws Exception {
		return runTask1();
	    }
	});

	Future<String> future2 = executor.submit(new Callable<String>() {
	    public String runTask2() throws InterruptedException {
		System.out.println(" --- task 2 start --- ");
		Thread.sleep(3000);
		System.out.println(" --- task 2 finish ---");
		return "Result of task 2!";
	    }
	    @Override
	    public String call() throws Exception {
		return runTask2();
	    }
	});
	//这里需要返回值时会阻塞主线程
	try {
	    String result1 = future1.get();
	    System.out.println("Task 1 result：" + result1);
	    String result2 = future2.get();
	    System.out.println("Task 2 result：" + result2);
	} catch (Exception e) {
	    e.printStackTrace();
	}

	Date date2 = new Date();
	System.out.println("Program end at：" + sdf.format(date2));
	System.out.println("It takes " + (date2.getTime() - date1.getTime()) / 1000 + " seconds." );
    }
}
```

这个程序运行的结果如下： <br/>

```text
Program begin at：2024-03-13 23:56:21.865
 --- task 1 start --- 
 --- task 2 start --- 
 --- task 2 finish ---
 --- task 1 finish ---
Task 1 result：Result of task 1!
Task 2 result：Result of task 2!
Program end at：2024-03-13 23:56:24.872
It takes 3 seconds.
```

从这里可以看出，两个任务都是耗时3秒，异步执行的话，总耗时也是3秒，相当于两个任务并行执行了。当然，这里如果将异步执行的线程池大小设置为1，耗时就会又变为6秒，相当于一个线程串行执行了。 <br/>
这里需要注意，代码中最后调用 `java.util.concurrent.Future#get()` 方法获取执行结果时，会阻塞主线程执行。为了避免长时间阻塞的情况，这里在获取结果时加一个超时的控制(改成调用 `java.util.concurrent.Future#get(long, java.util.concurrent.TimeUnit)` )，com/lfqy/trial/asyn/exec/AsynParellelExecV2.java，如下： <br/>

```java
package com.lfqy.trial.asyn.exec;

import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.concurrent.*;

public class AsynParellelExecV2 {



    public static void main(String []args){

	ExecutorService executor = Executors.newFixedThreadPool(2);

	SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss.SSS");
	Date date1 = new Date();
	System.out.println("Program begin at：" + sdf.format(date1));
	Future<String> future1 = executor.submit(new Callable<String>() {
	    public String runTask1() throws InterruptedException {
		System.out.println(" --- task 1 start --- ");
		Thread.sleep(3000);
		System.out.println(" --- task 1 finish ---");
		return "Result of task 1!";
	    }
	    @Override
	    public String call() throws Exception {
		return runTask1();
	    }
	});

	Future<String> future2 = executor.submit(new Callable<String>() {
	    public String runTask2() throws InterruptedException {
		System.out.println(" --- task 2 start --- ");
		Thread.sleep(3000);
		System.out.println(" --- task 2 finish ---");
		return "Result of task 2!";
	    }
	    @Override
	    public String call() throws Exception {
		return runTask2();
	    }
	});
	//这里需要返回值时会阻塞主线程
	try {
	    String result1 = future1.get(4000, TimeUnit.MILLISECONDS);
	    System.out.println("Task 1 result：" + result1);
	    String result2 = future2.get(4000, TimeUnit.MILLISECONDS);
	    System.out.println("Task 2 result：" + result2);
	} catch (TimeoutException e) {
	    System.out.println("Timeout : " + e);
	}catch (Exception e) {
	    e.printStackTrace();
	}

	Date date2 = new Date();
	System.out.println("Program end at：" + sdf.format(date2));
	System.out.println("It takes " + (date2.getTime() - date1.getTime()) / 1000 + " seconds." );
    }
}
```

运行结果没有变化，如下： <br/>

```text
Program begin at：2024-03-14 00:02:49.650
 --- task 1 start --- 
 --- task 2 start --- 
 --- task 1 finish ---
 --- task 2 finish ---
Task 1 result：Result of task 1!
Task 2 result：Result of task 2!
Program end at：2024-03-14 00:02:52.657
It takes 3 seconds.
```

最终的测试工程结构如下图： <br/>
![](/ox-hugo/240313_asyn_exec_structure.png) <br/>


## 写在后面 {#写在后面}

这个例子中的线程池是新建的一个线程池，实际上，如果类似的逻辑运行在提供dubbo服务的服务器上，可以直接拿到dubbo的线程池进行这种异步任务的处理。 <br/>
可行性上说，dubbo的rpc调用和本地java的执行机制应该一样，所以，可以复用(假如有手段可以拿到应用服务器中间件的线程池，就不能这么用，因为那个线程池可能不是java实现的)。我猜，如果看下dubbo的源码，搞不好和这个机制里面的线程池就是一套。在网上，随便搜下，"获取dubbo线程池"，可以找到好多说明材料，也就是说，确实可以拿到。 <br/>
如果复用dubbo的线程池，有什么好处呢？可以这样想，假设dubbo线程池设置的很大，但实际使用率又没有那么高，这样，复用dubbo线程池做异步处理实际上可以提高线程池的使用率，相较于新建专门的线程池，对系统的负载更小些。 <br/>

[^fn:1]: [Java实现异步编程的8种方式](https://juejin.cn/post/7165147306688249870) <br/>