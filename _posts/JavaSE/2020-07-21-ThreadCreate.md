---
layout: post
title: Java多线程上 --- 创建多线程 和 多线程的其他状态
categories: JavaSE
description: 
keywords: Thread,Java
---

# 创建多线程
三种创建方式：

+ 继承 Thread 类
+ 实现 Runnable 接口
+ 实现 Callable 接口

前两种是基础，第三种是以后实际用的最多的。

## 1、继承 Thread 类

### 1.1、 步骤
1. 自定义多线程类继承 Thread 类
2. 重写 run() 方法，编写线程执行体
3. 创建该线程对象，调用 start() 方法启动线程

### 1.2、 Demo
```java
//创建线程方式一：继承 Thread 类
public class TestThread1 extends Thread{
    @Override
    public void run() {
        //重写run方法体
        for (int i = 0; i < 2000; i++) {
            System.out.println("run():----------"+i);
        }
    }

    public static void main(String[] args) {
        TestThread1 testThread1 = new TestThread1();
        testThread1.start();        //***注意是start()方法开启多线程，而不是调用run()方法***

        for (int i = 0; i < 2000; i++) {
            System.out.println("main():----------"+i);
        }
    }
}
```

### 1.3、 网图下载
需要借助 apache 的 commons-io.jar 包

```java
//联系Thread 实现多线程同步下载图片
public class TestThread2 extends  Thread{
    private String url; //网络图片地址
    private String name; //保存的文件名

    public TestThread2(String url,String name){
        this.url = url;
        this.name = name;
    }

    @Override
    public void run() {
        WebDownLoader webDownLoader = new WebDownLoader();
        webDownLoader.downloader(url,name);
        System.out.println("下载了文件名为："+name);
    }

    public static void main(String[] args) {
        TestThread2 t1 = new TestThread2("https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=2108204240,3928402857&fm=26&gp=0.jpg","1.jpeg");
        TestThread2 t2 = new TestThread2("https://i0.hdslb.com/bfs/article/a6c6e7936c593e67ed3640e8b0f4ec13d70a3eef.jpg@1320w_824h.jpg","2.jpg");
        TestThread2 t3 = new TestThread2("https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=4283250746,3502970724&fm=26&gp=0.jpg","3.jpg");

		//并不能确定三张图片下载完成的先后顺序
        t1.start();
        t2.start();
        t3.start();
    }
}

class WebDownLoader{
    public void downloader(String url,String name){
        try {
            FileUtils.copyURLToFile(new URL(url),new File(name));		// commons 包的 FileUtils 工具类
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## 2、 实现 Runnable 接口
### 2.1、 步骤
1. 自定义类，实现Runnable接口
2. 实现 run() 方法，编写线程执行体
3. 创建其他线程对象，调用 start() 方法启动线程

### 2.2、 Demo 
```java
//创建线程方式二：实现 Runnable 接口
public class TestThread3 implements Runnable{
    @Override
    public void run() {
        //重写run方法体
        for (int i = 0; i < 2000; i++) {
            System.out.println("run():----------"+i);
        }
    }

    public static void main(String[] args) {
        TestThread3 testThread3 = new TestThread3();
        new Thread(testThread3).start();

        for (int i = 0; i < 2000; i++) {
            System.out.println("main():----------"+i);
        }
    }
}
```

## 3、 实现 Callable 接口
### 3.1、 步骤

1. 实现 Callable 接口，需要返回值类型
2. 重写 call 方法，需要抛出异常
3. 创建目标对象
4. 创建执行服务
5. 提交执行
6. 获取结果
7. 关闭服务

### 3.2、 Demo
使用实现 Callable 接口来完成 网图下载
```java
public class TestThread4 implements Callable<Boolean> {
    private String url; //网络图片地址
    private String name; //保存的文件名

    public TestThread4(String url,String name){
        this.url = url;
        this.name = name;
    }

    @Override
    public Boolean call() {
        WebDownLoader webDownLoader = new WebDownLoader();
        webDownLoader.downloader(url,name);
        System.out.println("下载了文件名为："+name);
        return true;
    }

    public static void main(String[] args) throws ExecutionException, InterruptedException {
        TestThread4 t1 = new TestThread4("https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=2108204240,3928402857&fm=26&gp=0.jpg","1.jpeg");
        TestThread4 t2 = new TestThread4("https://i0.hdslb.com/bfs/article/a6c6e7936c593e67ed3640e8b0f4ec13d70a3eef.jpg@1320w_824h.jpg","2.jpg");
        TestThread4 t3 = new TestThread4("https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=4283250746,3502970724&fm=26&gp=0.jpg","3.jpg");

        //创建执行服务：
         ExecutorService service = Executors.newFixedThreadPool(3);
        //提交执行：
        Future<Boolean> result1 = service.submit(t1);
        Future<Boolean> result2 = service.submit(t2);
        Future<Boolean> result3 = service.submit(t3);
        //获取结果：
        boolean r1 = result1.get();
        boolean r2 = result2.get();
        boolean r3 = result3.get();
        System.out.println(r1);
        System.out.println(r2);
        System.out.println(r3);
        //关闭服务：
        service.shutdownNow();
    }
}
```
### 3.3、 使用 Callable 的好处
+ 可以得到返回值
+ 可以抛出异常

# 多线程的其他状态
## 1、 线程停止
+ 不推荐使用JDK提供的 stop() ， destroy() 方法（已废弃）
+ 推荐线程自己停下来。也就是将run()方法执行完毕
+ 建议使用一个标志位 flag 来使线程停止

### 1.1、 Demo：
```java
public class TestStop implements Runnable{
    private boolean flag;
    private int i = 0;
    @Override
    public void run() {
        flag = true;
        while(flag){
            System.out.println("thread running....."+i++);
        }
    }
    public void stop(){
        this.flag = false;
    }
    public static void main(String[] args) {
        TestStop testStop = new TestStop();
        new Thread(testStop).start();
        for (int i = 0; i < 1000; i++) {
            System.out.println("main running....."+i++);

        }
        //主动停止线程
        testStop.stop();
    }
}

```

## 2、 线程休眠

+ sleep (时间) 指定当前线程阻塞的毫秒数
+ sleep 存在异常 InterruptedException ;
+ sleep 时间到达后线程进入就绪状态
+ **每一个对象都有一个锁，sleep不会释放锁**

### 2.1、 Demo：模拟倒计时和显示当前时间
```java
public class TestSleep {
    public static void main(String[] args) {
        daojishi(10);
        showTime();
    }
    
    //实时显示当前时间
    public static void showTime(){
        Date date = new Date(System.currentTimeMillis());
        while(true){
            try {
                Thread.sleep(1000);
                System.out.println(new SimpleDateFormat("HH:mm:ss").format(date));
                date = new Date(System.currentTimeMillis());
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
    
    //倒计时
    public static void daojishi(int second){
        while(second>=0){
            try {
                System.out.println(second--);
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

```

## 3、 线程礼让

+ 礼让线程，让当前正在执行的线程暂停，回到就绪队列
+ **让 cpu 重新调度，有可能还是调度到刚才礼让的线程，所以礼让不一定会成功**

### 3.1、 Demo
```java
public class TestYield {
    public static void main(String[] args) {
        MyYield myYield = new MyYield();
        new Thread(myYield,"1").start();
        new Thread(myYield,"2").start();
    }
}
class MyYield implements Runnable{

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName()+"线程开始执行");
        Thread.yield();     //礼让
        System.out.println(Thread.currentThread().getName()+"线程结束执行");
    }
}
```

## 4、 线程强制执行

+ join合并线程，强制让当前线程阻塞，执行join合并的线程，待此线程执行完成后，在执行其他线程
+ 可以理解为现实生活中的插队

### 4.1、 Demo
```java
public class TestJoin implements Runnable{
    @Override
    public void run() {
        for (int i = 0; i < 1000; i++) {
            System.out.println("VIP:"+i);
        }
    }

    public static void main(String[] args) {

        TestJoin testJoin = new TestJoin();
        Thread thread = new Thread(testJoin);
        thread.start();


        for (int i = 0; i < 500; i++) {
            System.out.println("main:"+i);
            if(i == 200){
                try {
                    thread.join();  //强制插入
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```
输出说明：当main中的i<200的时候 main和VIP交替输出，但是调用thread.join();之后，main线程暂停，等待VIP线程执行完毕之后(输出到1000)，main线程重新继续执行。

## 5、 线程状态观测

+ Thread.State 是一个 Enum 类型，其6个值表示线程的六种状态
+ NEW：尚未启动的线程处于此状态
+ RUNNABLE：在Java虚拟机中执行的线程处于此状态
+ BLOCKED：被阻塞的线程处于此状态
+ WAITTING：正在等待另一个线程执行特定动作的线程处于此状态
+ TIME_WAITTING：正在等待另一个线程执行动作到达指定等待时间的线程处于此状态
+ TERMINATED：已退出的线程处于此状态，**线程一旦进入死亡状态，就不能再次启动**

### 5.1、 Demo
```java
package com.lrr.thread;
public class TestState implements Runnable{
    public static void main(String[] args) throws InterruptedException {
        TestState testState = new TestState();
        Thread thread = new Thread(testState);

        System.out.println(thread.getState());	//NEW状态

        thread.start();
        System.out.println(thread.getState());	//RUNNABLE状态
        while(true) {
            Thread.sleep(10);
            System.out.println(thread.getState());	//TIMED_WAITING状态
            if(thread.getState() == Thread.State.TERMINATED) break;
        }
        System.out.println(thread.getState());	//TERMINATED状态
        
        thread.start();			//线程一旦死亡，就不能再次启动，这行代码会报IllegalThreadStateException异常
    }

    @Override
    public void run() {
        for (int i = 0; i < 3; i++) {
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```
## 6、 线程优先级
+ java提供一个线程调度器来监控程序中启动后进入就绪状态的所有线程，线程调度器按照优先级决定应该调度哪个线程来执行
+ **优先级必须在线程启动前设置**
+ 线程的优先级用数字表示，范围为1~10。并且java提供了三个优先级常量
	+ Thread.MIN_PRIORITY = 1;		//最小的优先级
	+ Thread.NORM_PRIORITY = 5;	   	  //默认的优先级
	+ Thread.MAX_PRIORITY = 10;		//最高的优先级
+ **优先级低只是意味着获得调度的概率低，并不是优先级低就不会被调用了。这都是看CPU的调度**

## 7、 守护线程

+ 线程分为用户线程 和守护线程
+ **虚拟机必须确保用户线程执行完毕**
+ **虚拟机不用等待守护线程执行完毕**

### 7.1、 Demo
```java
public class TestDaemo implements Runnable{
    @Override
    public void run() {
        while(true){}
    }

    public static void main(String[] args) {
        TestDaemo testDaemo = new TestDaemo();
        Thread thread = new Thread(testDaemo);
        
        /**
         如果没有设置为守护线程，那么该程序永远不会停下(main线程跑完但是thread线程不会停止)
         如果设置为守护线程，那么main线程跑完程序停止(因为thread线程是守护线程，而虚拟机不用等待守护线程执行完毕，gc同理)
         */
        thread.setDaemon(true);	
        thread.start();
    }
}
```