---
layout: post
title: Java多线程下 --- 线程同步
categories: JavaSE
description: 
keywords: Thread,Java，synchronization
---

接上文[Java多线程上 --- 创建多线程 和 多线程的其他状态](https://mikasalee.github.io/2020/07/21/ThreadCreate/)，继续解决线程基础的最后一个点：线程同步


# 1、 概念
+ 并发：同一个对象被多个线程同时操作
+ 线程同步：本质是一种等待机制，多个需要同时访问此对象的线程进入这个**对象的等待池** 形成队列，等待前面线程使用完毕，下一个线程再使用
+ 锁：保证数据在方法中被访问时的正确性，引入锁机制

# 2、 三大不安全案例
# 2.1、 买票案例
```java

public class UnsafeBuyTicket {
    public static void main(String[] args) {
        BuyTicket buyTicket = new BuyTicket();

        //三个人抢票
        new Thread(buyTicket,"我").start();
        new Thread(buyTicket,"你").start();
        new Thread(buyTicket,"黄牛").start();
    }
}

class BuyTicket implements Runnable{

    private int ticket = 10;   //总共10张票

    @Override
    public void run() {
        while(true){
            if (ticket <= 0) {
                System.out.println("没票了");
                return;
            }
            try {
                Thread.sleep(100);	//0.1s的延时，用来放大问题
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + "买到了第" + ticket-- + "张票");
        }
    }
}
```

输出：
```
你买到了第10张票
黄牛买到了第9张票
我买到了第8张票
你买到了第7张票
黄牛买到了第6张票
我买到了第5张票
黄牛买到了第4张票
我买到了第2张票
你买到了第3张票
你买到了第1张票
没票了
我买到了第1张票
没票了
黄牛买到了第0张票
没票了
```
可以很容易的看到最后一张票被两个人同时买了，并且还出现`黄牛买到了第0张票`的错误现象

## 2.2、 银行取钱案例
```java
public class UnsafeBank {
    public static void main(String[] args) {
        Bank bank = new Bank();
        new Thread(bank,"我").start();
        new Thread(bank,"你").start();
    }
}
class Account {
    int money;
    public Account(int money) {
        this.money = money;
    }

    public void giveMoney(int number){
        if(money - number < 0){
            System.out.println("余额不足！");
            return;
        }
        try {
            Thread.sleep(500);      //0.5s的延时，用来放大问题
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        money = money - number;
        System.out.println(Thread.currentThread().getName()+"取了"+number);
    }
}


class Bank implements Runnable{

    private Account account = new Account(100);

    @Override
    public void run() {
        if(Thread.currentThread().getName().equals("我"))
            account.giveMoney(80);
        else if(Thread.currentThread().getName().equals("你"))
            account.giveMoney(80);
    }
}
```

输出：
```
你取了80
我取了80
```
总共只有100 ，但是却取出来160。

## 2.3、 集合不安全案例

```java
public class UnsafeList {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        for (int i = 0; i < 100000; i++)
            new Thread(() -> {
                list.add(Thread.currentThread().getName());
            }).start();

        try {
            Thread.sleep(1000);		//1s的延时，差不多能保证main线程是最后执行了
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(list.size());
    }
}

```
输出：
```
99987
```
总共将 100000 个线程放入集合中，但是最终集合的 size() 为 99987 。这是因为发生多个线程放入list的同一个槽。

# 3、 同步方法以及同步块
+ 我们可以通过private关键字保证成员只能被方法访问，所以没有同步成员这一说法
+ 同步方法以及同步块都是通过 `synchronized` 关键字实现

## 3.1、 同步方法
+ 只需要在方法头加上 synchronized 关键字修饰即可 ：
   `public **synchronized** void method(){}`

+ synchronized 方法控制对"对象"的访问，每个对象对应一把锁，每个 synchronized 方法都必须获得调用该方法的对象的锁才能进行，否则线程会阻塞，方法一旦执行，就独占该锁，知道方法返回才释放锁。
+ 缺陷：
	+ 若将一个大的方法声明为 synchronized 将会影响效率
	+ 锁的范围是整个方法，锁的太多，浪费资源
+ **所以比起同步方法，更建议使用同步块**

## 3.2、 同步块
+ `synchronized(obj){}`
+ obj 称之为同步监视器
	+ obj可以为任何对象，但是推荐使用共享资源作为同步监视器
	+ 同步方法中无需指定同步监视器，因为同步方法的同步监视器就是this，或者是class
+ 监视器执行过程
	1. 第一个线程访问，锁定同步监视器，执行其中代码
	2. 第二个线程访问，发现同步监视器被锁定，无法访问。
	3. 第一个线程访问完毕，解除同步监视器。
	4. 第二个线程访问，发现同步监视器没有锁，然后锁定并访问。

# 4、 使用 同步方法以及同步块 来解决三大不安全案例
## 4.1、 买票案例
```java
//不安全的买票
public class UnsafeBuyTicket {
    public static void main(String[] args) {
        BuyTicket buyTicket = new BuyTicket();

        //三个人抢票
        new Thread(buyTicket,"我").start();
        new Thread(buyTicket,"你").start();
        new Thread(buyTicket,"黄牛").start();
    }
}

class BuyTicket implements Runnable{

    private int ticket = 10;   //总共10张票

    @Override
    public  void run() {
        while(true){
            synchronized (this) {		//ticket为共享资源，所以对所有涉及到ticket的代码上锁
                if (ticket <= 0) {
                    System.out.println("没票了");
                    return;
                }
                System.out.println(Thread.currentThread().getName() + "买到了第" + ticket-- + "张票");
            }//synchronized代码块结束
            
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

输出：
```
我买到了第10张票
黄牛买到了第9张票
你买到了第8张票
你买到了第7张票
黄牛买到了第6张票
我买到了第5张票
你买到了第4张票
黄牛买到了第3张票
我买到了第2张票
你买到了第1张票
没票了
没票了
没票了
```
## 4.2、 银行取钱案例
```java
public class UnsafeBank {
    public static void main(String[] args) {
        Bank bank = new Bank();
        new Thread(bank,"我").start();
        new Thread(bank,"你").start();
    }
}
class Account {
    int money;
    public Account(int money) {
        this.money = money;
    }

    public synchronized void giveMoney(int number){	//共享变量为money 这里使用同步方法
        if(money - number < 0){
            System.out.println("余额不足！");
            return;
        }
        try {
            Thread.sleep(500);      //0.5s的延时，用来放大问题
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        money = money - number;
        System.out.println(Thread.currentThread().getName()+"取了"+number);
    }
}


class Bank implements Runnable{

    private Account account = new Account(100);

    @Override
    public void run() {
        if(Thread.currentThread().getName().equals("我"))
            account.giveMoney(80);
        else if(Thread.currentThread().getName().equals("你"))
            account.giveMoney(80);
    }


}

```

输出
```
我取了80
余额不足！
```

## 4.3、 集合不安全案例
```java
public class UnsafeList {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        for (int i = 0; i < 100000; i++)
            new Thread(() -> {
                synchronized (list){
                    list.add(Thread.currentThread().getName());
                }
            }).start();

        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(list.size());
    }
}

```

输出
```
100000
```

## 4.4、 用 CopyOnWriteArrayList 来解决 集合不安全案例
CopyOnWriteArrayList 是juc包下面集合类，它与ArrayList最大的不同是线程安全
```java
public class UnsafeList {
    public static void main(String[] args) {

        CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList<>();		//把ArrayList改成CopyOnWriteArrayList
        for (int i = 0; i < 100000; i++)
            new Thread(() -> {
                list.add(Thread.currentThread().getName());	//这里就不需要同步块了，因为CopyOnWriteArrayList已经保证线程安全了。
            }).start();

        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(list.size());
    }
}
```

输出
```
100000
```

# 5、 死锁
+ 死锁：多个线程互相抱着对方需要的资源，然后形成僵持
+ 产生死锁的四个必要条件：（只要破坏任意一个就能避免死锁发生）
	1. 互斥条件：一个资源每次只能被一个进程使用
	2. 请求与保持条件：一个资源因请求资源而阻塞时，对已获得的资源保持不放
	3. 不剥夺条件：进程已获得的资源，在未使用完之前，不能强行剥夺
	4. 循环等待条件：若干进程之间形成一种头尾相接的循环等待资源关系

# 6、 Lock 接口
+ 从 JDK 5.0 开始，Java提供了更强大的线程同步机制——通过显式定义*同步锁对象*来实现同步(synchronized 是隐式定义的同步锁，因为开关锁的操作都不用我们编写)。同步锁使用 Lock 对象充当
+ java.util.concurrent.locks.Lock 接口是控制多个线程对共享资源进行访问的工具。锁提供了对共享资源的独占访问，每次只能有一个线程对 Lock 对象加锁，线程开始访问共享资源前应先获得 Lock 对象
+ **ReentrantLock**  类实现了 Lock，它拥有与 synchronized 相同的并发性和内存语义，在实现线程安全的控制中，比较常用的是 ReentrantLock，可以显式加锁、释放锁 

## 6.1、 买票demo
```java
public class TestLock implements Runnable{

    public static void main(String[] args) {
        TestLock testLock = new TestLock();
        new Thread(testLock).start();
        new Thread(testLock).start();
        new Thread(testLock).start();
    }
    int ticket = 10;

    private final ReentrantLock lock = new ReentrantLock();

    @Override
    public void run() {
        while (true){
            //lock上锁解锁操作建议放在try-finally里面，上锁在try 解锁在finally
            try{
                lock.lock();        //加锁
                if(ticket <= 0 ) break;
                System.out.println(ticket--);
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }finally{
                lock.unlock();      //解锁
            }

        }
    }
}
```
## 6.2、 synchronized 与 Lock 的对比
+ Lock 是显示锁（手动开锁关锁），synchronized 是隐式锁，出了作用域自动释放
+ Lock 只有代码块锁，synchronized 有代码块锁和方法锁
+ 使用 Lock 锁，JVM 将花费较少的时间来调度线程，性能更好。并且具有更好的扩展性（提供更多子类）
+ 优先使用顺序：
	Lock > 同步代码块 > 同步方法
	
# 7、 线程通信
+ wait() ：表示线程一直等待，直到其他线程通知，与 sleep 不同，会释放锁
+ notify()：唤醒一个处于等待状态的线程
+ **wait() 和 notify()  都是 Object 类的方法，并且使用时必须放在同步代码块或者同步方法中，否则报 java.lang.IllegalMonitorStateException 异常**



## 7.1、 管程法解决生产者消费者模式demo

```java
public class TestPC {
    public static void main(String[] args) {
        Plate plate = new Plate();
        Productor productor = new Productor(plate);
        Customer customer = new Customer(plate);
        productor.start();
        customer.start();
    }
}
//生产者
class Productor extends Thread{
    Plate plate;
    public Productor(Plate plate) {
        this.plate = plate;
    }
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            plate.push(new Chicken(i+1));  //往盘子里面加了只鸡
            System.out.println("往盘子放入第"+(i+1)+"只鸡");
        }
    }
}
//消费者
class Customer extends Thread{
    Plate plate;
    public Customer(Plate plate) {
        this.plate = plate;
    }
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            Chicken chicken = plate.pop();
            System.out.println("从盘子拿走第"+chicken.id+"只鸡");
        }
    }
}
//鸡（产品）
class Chicken{int id;
    public Chicken(int id) {
        this.id = id;
    }
} //区分一下是哪只鸡
//盘子（缓冲区）
class Plate{
    Chicken[] chickens = new Chicken[10];   //盘子里面只能放10只鸡
    int size = 0;
    public synchronized void push(Chicken chicken){
        if (size == 10) { //盘子满了
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        chickens[size++] = chicken;
        //通知来拿鸡
        notify();
    }
    public synchronized Chicken pop() {
        if (size == 0) { //盘子空了
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        //还有鸡
        notify();
        return chickens[--size];
    }

}
```

# 8、 线程池
+ 背景：对使用量特别大的资源进行频繁的创建和销毁（比如并发情况下的线程），对性能影响很大
+ 思路：提前创建好多个线程，放入线程池中，使用时直接获取，使用完放回池中。可以避免频繁创建销毁、实现重复利用。
+ 好处：
	+ 提高响应速度（减少了创建新线程的时间）
	+ 降低资源消耗（重复利用线程池中线程，不需要每次都创建）
	+ 便于线程管理
+ JDK 5.0起提供了线程池相关API：ExecutorService 和 Executors
	+ ExecutorService：真正的线程池接口。常见子类ThreadPoolExecutor
	+ Executors：工具类、线程池的工厂类，用于创建并返回不同类型的线程池

## 8.1、 Demo
```java
public class TestPool {
    public static void main(String[] args) {
        //1. 创建服务，创建线程池
        //newFixedThreadPool 参数为线程池大小
        ExecutorService service = Executors.newFixedThreadPool(10);    
        //执行
        service.execute(new MyThread());
        service.execute(new MyThread());
        service.execute(new MyThread());
        service.execute(new MyThread());        
        //2. 关闭
        service.shutdown();
    }
}
class MyThread implements Runnable{
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }
}
```