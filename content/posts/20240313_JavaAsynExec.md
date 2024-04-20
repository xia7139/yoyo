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
- [异步执行线程池创建参数](#异步执行线程池创建参数)

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
PS： <br/>
这个例子中的线程池是新建的一个线程池，实际上，如果类似的逻辑运行在提供dubbo服务的服务器上，可以直接拿到dubbo的线程池进行这种异步任务的处理。 <br/>
可行性上说，dubbo的rpc调用和本地java的执行机制应该一样，所以，可以复用(假如有手段可以拿到应用服务器中间件的线程池，就不能这么用，因为那个线程池可能不是java实现的)。我猜，如果看下dubbo的源码，搞不好和这个机制里面的线程池就是一套。在网上，随便搜下，"获取dubbo线程池"，可以找到好多说明材料，也就是说，确实可以拿到。 <br/>
如果复用dubbo的线程池，有什么好处呢？可以这样想，假设dubbo线程池设置的很大，但实际使用率又没有那么高，这样，复用dubbo线程池做异步处理实际上可以提高线程池的使用率，相较于新建专门的线程池，对系统的负载更小些。 <br/>


## 异步执行线程池创建参数 {#异步执行线程池创建参数}

异步执行的线程池在创建时，有一些可以进行设置的参数，如下： <br/>

```java
/**
 * Creates a new {@code ThreadPoolExecutor} with the given initial
 * parameters and default thread factory.
 *
 * @param corePoolSize the number of threads to keep in the pool, even
 *        if they are idle, unless {@code allowCoreThreadTimeOut} is set
 * @param maximumPoolSize the maximum number of threads to allow in the
 *        pool
 * @param keepAliveTime when the number of threads is greater than
 *        the core, this is the maximum time that excess idle threads
 *        will wait for new tasks before terminating.
 * @param unit the time unit for the {@code keepAliveTime} argument
 * @param workQueue the queue to use for holding tasks before they are
 *        executed.  This queue will hold only the {@code Runnable}
 *        tasks submitted by the {@code execute} method.
 * @param handler the handler to use when execution is blocked
 *        because the thread bounds and queue capacities are reached
 * @throws IllegalArgumentException if one of the following holds:<br>
 *         {@code corePoolSize < 0}<br>
 *         {@code keepAliveTime < 0}<br>
 *         {@code maximumPoolSize <= 0}<br>
 *         {@code maximumPoolSize < corePoolSize}
 * @throws NullPointerException if {@code workQueue}
 *         or {@code handler} is null
 */
public ThreadPoolExecutor(int corePoolSize,
			  int maximumPoolSize,
			  long keepAliveTime,
			  TimeUnit unit,
			  BlockingQueue<Runnable> workQueue,
			  RejectedExecutionHandler handler) {
    this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
	 Executors.defaultThreadFactory(), handler);
```

各个参数的含义： <br/>

-   corePoolSize，核心线程池大小； <br/>
-   maximumPoolSize，线程池大小上限； <br/>
-   keepAliveTime，存活时间，当线程池中的线程超过cpu核心数时，空闲的线程的存活时间。也就是说，如果没有接到新的任务，最多存活这个时间就挂了； <br/>
-   unit，存过时间单位； <br/>
-   workQueue，等待队列，当任务提交到线程池，但是线程池中线程都在忙时，会将任务放在等待队列中，这个队列大小默认没有上限(直到内存爆了)，最小是1(不能新建大小为0的队列)； <br/>
-   handler，当线程池中没有空闲，且等待队列已满时的拒绝策略。 <br/>
    主要有如下几种： <br/>
    
    -   CallerRunsPolicy：由调用线程处理该任务。 <br/>
    -   AbortPolicy：默认策略，不执行任务，直接抛出RejectedExecutionException异常。 <br/>
    -   DiscardPolicy：不执行任务，也不抛出异常，直接丢弃。 <br/>
    -   DiscardOldestPolicy：抛弃队列中最老的任务，然后尝试再次执行任务。 <br/>
    
    如下是一个当线程池等待队列满时，在当前调用线程顺序执行的一个例子： <br/>
    ```java
    package com.lfqy.trial.asyn.exec;
    
    import java.text.SimpleDateFormat;
    import java.util.Date;
    import java.util.concurrent.*;
    
    public class AsynParellelExecV3 {
    
        public static void main(String []args){
    
    	//ExecutorService executor = Executors.newFixedThreadPool(2);
    	ExecutorService executor = new ThreadPoolExecutor(1,
    		1, 2, TimeUnit.MINUTES,
    		new ArrayBlockingQueue<Runnable>(1),
    		new ThreadPoolExecutor.CallerRunsPolicy());;
    
    	SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss.SSS");
    	Date date1 = new Date();
    	System.out.println("Program begin at：" + sdf.format(date1));
    	Future<String> future1 = executor.submit(new Callable<String>() {
    	    public String runTask1() throws InterruptedException {
    		System.out.println(" --- task 1 start --- ");
    		Thread.sleep(10000);
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
    
    	Future<String> future3 = executor.submit(new Callable<String>() {
    	    public String runTask3() throws InterruptedException {
    		System.out.println(" --- task 3 start --- ");
    		Thread.sleep(3000);
    		System.out.println(" --- task 3 finish ---");
    		return "Result of task 3!";
    	    }
    	    @Override
    	    public String call() throws Exception {
    		return runTask3();
    	    }
    	});
    
    
    
    	Future<String> future4 = executor.submit(new Callable<String>() {
    	    public String runTask4() throws InterruptedException {
    		System.out.println(" --- task 4 start --- ");
    		Thread.sleep(3000);
    		System.out.println(" --- task 4 finish ---");
    		return "Result of task 4!";
    	    }
    	    @Override
    	    public String call() throws Exception {
    		return runTask4();
    	    }
    	});
    
    	System.out.println("Now, start to get result.");
    	//这里需要返回值时会阻塞主线程
    	try {
    	    String result1 = future1.get(4000, TimeUnit.MILLISECONDS);
    	    System.out.println("Task 1 result：" + result1);
    	} catch (TimeoutException e) {
    	    System.out.println("Timeout : " + e);
    	}catch (Exception e) {
    	    e.printStackTrace();
    	}
    
    	try {
    	    String result2 = future2.get(4000, TimeUnit.MILLISECONDS);
    	    System.out.println("Task 2 result：" + result2);
    	} catch (TimeoutException e) {
    	    System.out.println("Timeout : " + e);
    	}catch (Exception e) {
    	    e.printStackTrace();
    	}
    
    	try {
    	    String result3 = future3.get(1000, TimeUnit.MILLISECONDS);
    	    System.out.println("Task 3 result：" + result3);
    	} catch (TimeoutException e) {
    	    System.out.println("Timeout : " + e);
    	}catch (Exception e) {
    	    e.printStackTrace();
    	}
    
    	try {
    	    String result4 = future4.get(1000, TimeUnit.MILLISECONDS);
    	    System.out.println("Task 4 result：" + result4);
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
    输出如下： <br/>
    ```text
    Program begin at：2024-04-21 00:57:01.760
     --- task 1 start --- 
     --- task 3 start --- 
     --- task 3 finish ---
     --- task 4 start --- 
     --- task 4 finish ---
    Now, start to get result.
     --- task 1 finish ---
    Task 1 result：Result of task 1!
     --- task 2 start --- 
     --- task 2 finish ---
    Task 2 result：Result of task 2!
    Task 3 result：Result of task 3!
    Task 4 result：Result of task 4!
    Program end at：2024-04-21 00:57:14.772
    It takes 13 seconds.
    ```

[^fn:1]: [Java实现异步编程的8种方式](https://juejin.cn/post/7165147306688249870) <br/>