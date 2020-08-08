---
title: Java-多线程
date: 2020-06-25 20:24:46
tags: Java
urlname: java-multithread
category:  笔记
description: Java基础：多线程
---

![多线程](https://i.loli.net/2020/06/25/aVB97SkPCuYIWpy.png)

## 一、概念

### 基本概念

```java
package com.landfill.java;
/*
多线程
1.程序、进程、线程
程序program是为完成特定任务、用某种语言编写的一组指令的集合。即指一段静态的代码，静态对象

进程process程序的一次执行过程，正在运行的一个程序，作为资源分配的单位。有产生存在和消亡的过程-生命周期，
系统在运行时会为每个进程分配不同的内存区域。

线程thread  进程进一步细化为线程，程序内部的一条执行路径。作为调度和执行的单位，每个线程拥有独立的运行栈
和程序计数器pc，但是共享堆和方法区。进程间的通信更加简便高效。所以就有线程安全的问题。
一个java至少有三个线程  main()主线程 gc()垃圾回收线程 异常处理线程

单核和多核CPU  服务器都是多核的
并行和并发 多个CPU同时执行多个任务/一个CPU(时间片)同时执行多个任务

多线程的优点
场景：需要同时执行两个或多个任务
程序需要实现一些需要等待的任务。如用户输入 文件读写操作 网络操作 搜索
需要一些后台运行的程序。

守护线程/用户线程


 */
public class MultiThread {
    public static void main(String[] args) {

    }
}
```

### 	thread lifecycle

```java
package com.landfill.java;
/*
线程的生命周期
新建: new了对象
就绪：调用start()  等待cpu分配资源
运行：cpu切换线程，又会失去执行权，或者调用yield(),不同于阻塞
阻塞：join()  sleep(long millis) 等待同步锁 wait() suspend()(deprecated,会导致死锁)
     sleep()时间到，join()的线程结束，获取同步锁，notify() notifyAll() resume()(搭配suspend,deprecated）  回到就    		绪，再等待CPU分配执行权
死亡：执行完run()   stop()  出现error/exception且没处理
 */
public class ThreadState {
}

```



### 	thread scheduling &priority

```java
package com.landfill.java;
/*
线程的调度
同优先级线程组成先进先出队列，使用时间片策略
高优先级的，使用优先调度的抢占式策略
但只是从概率上来说，高优先级的线程会被先执行，并不意味着一定先执行高优先级的

优先级等级
可以设置10档
MAX_PRIORITY:10
MIN_PRIORITY:1
NORM_PRIORITY:5

涉及的方法：获取和设置
getPriority()
setPriority(int newPriority)


 */
public class ThreadScheduling {
    public static void main(String[] args) {
        MyThread3 t = new MyThread3();
        t.start();
        for (int i = 0; i < 100; i++) {
            if(i % 2 == 0){
                System.out.println(Thread.currentThread().getName()+":"+i);
                Thread.currentThread().setPriority(Thread.MIN_PRIORITY);
            }
        }

    }
}

class MyThread3 extends Thread{
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if(i % 2 == 0){
                System.out.println(this.getName()+":"+i);
                setPriority(Thread.MAX_PRIORITY);
            }
        }
    }
}
```

### 	Thread类常见方法

```java
package com.landfill.java;
/*
Thread类的常用方法
1.start()：启动当前线程 调用当前线程的run()
2.run()：通常需要重写此方法，在创建的线程中需要执行的代码
3.currentThread()：静态方法 返回当前代码执行的线程
4.getName(): 获取当前线程的名字
5.setName(): 设置当前线程的名字  构造器，通过currentThread()或者new对象调用setName()
6.yield(): 释放当前CPU的执行权，但是也可能被CPU继续分配  线程让步
7.join(): 在线程A中调用另外一个线程的join(),线程A进入阻塞状态。等另一个线程执行完了，线程A结束阻塞，再执行A线程
8.stop()：强制结束线程生命期 不推荐使用   //deprecated
9.sleep(long millis): 阻塞millis毫秒  静态方法，可以直接调用。让当前线程睡眠指定的millis毫秒
10.isAlive(): 判断线程是否存活

 */
public class ThreadMethodTest {
    public static void main(String[] args) {
        MyThread1 t1 = new MyThread1("进程一");

        //给主线程命名
        Thread.currentThread().setName("main线程");   //静态方法 currentThread返回当前的Thread
        System.out.println("a");
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("b");
        System.out.println(Thread.currentThread().getName());
        System.out.println(t1.getName());
        t1.start();
        for (int i = 0; i <100; i++) {
            if (i % 2 == 0) {
                System.out.println(Thread.currentThread().getName() + ":" + i);
            }
            if(i == 20){
                try {
                    t1.join();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
        System.out.println(t1.isAlive());
        }

}

class MyThread1 extends Thread{
    @Override
    public void run() {
        for (int i = 0; i <100; i++) {
            if(i % 2 == 0){
//                try {
//                    sleep(1000);
//                } catch (InterruptedException e) {
//                    e.printStackTrace();
//                }
                System.out.println(Thread.currentThread().getName()+":"+i);
            }

//            if(i == 20){
//                this.yield(); //释放内存的执行权
//            }

        }

        System.out.println(isAlive());
        //Thread.currentThread().setName("线程1");//可以设置线程的名字
        // System.out.println(Thread.currentThread().getName());
    }
    public MyThread1(String name){  //通过构造器来给thread命名
        super(name);
    }
}

```



​		

## 二、创建多线程

### 	BEFOR JDK5.0

#### 		1. extends Thread

```
package com.landfill.java;
/*
多线程的创建
方式一：继承于Thread类
1.创建一个继承于Thread类的子类
2.重写Thread类的run() 将此线程执行的操作声明在run()中
3.创建子类的对象
4.通过此对象调用start()
//注意：
start启动了线程并且去调用run(),不能使用对象直接调用run()
不能再start()来创建一个线程，需要重新创建一个对象。
 */
public class ThreadTest {
    public static void main(String[] args) {
        MyThread t1 = new MyThread(); //3.创建子类的对象
        t1.start();         //4.通过此对象调用start()  作用：①启动当前线程，②调用当前线程的run方法
       // t1.run(); 无法启动线程，仍然是在主线程的中进行的。
       //以下操作仍然是在main线程执行的
        for(int i = 0;i<100;i++){
            if(i%2!=0){
                System.out.println("hello------------------------------");
                System.out.println(Thread.currentThread().getName());
            }
        }
        //通过再 new一个线程的对象来调用start
        MyThread t2 = new MyThread();
        t2.start();
    }

}


 class MyThread  extends Thread{ //1.创建一个继承于Thread类的子类
    @Override               //2.重写Thread类的run() 将此线程执行的操作声明在run()中
    public void run() {
        for(int i = 0;i<100;i++){
            if(i%2==0){
                System.out.println(i);
                System.out.println(Thread.currentThread().getName());
            }
        }
    }
}
```



#### 		2. implements Runnable

```java
package com.landfill.java;
/*
Thread的创建方式二：实现runnable接口
1.创建Runnable接口的实现类
2.实现类去实现抽象方法 run();
3.创建实现类的对象
4.将此对象作为参数传到Thread类的构造器，创建Thread类的对象
5.Thread类 调用start()

两种方法都得使用Thread类的start()

比较两种方式：  继承Thread类 vs 实现Runnable
1.继承Thread类，存在单继承的限制，如果已经有其他声明的父类，则不能使用了
2.实现Runnable 可以只创建一个对象，把共享数据封装到实现类里，多个线程共享数据,还可以实现多个接口，没有单继承的限制
所以开发中一般优先用实现Runnable接口,而且从源码可以看到 Thread类实际上就是实现了Runnable接口,何必多绕一步？
两种方式都要重写run() 将线程要执行的代码放入方法体。
 */
public class ThreadCreate {
    public static void main(String[] args) {
        MyThread4 target = new MyThread4();
        Thread t = new Thread(target);
        t.setName("线程1");
        t.start();
        //调用当前线程的run()-->调用了Runnable类型的target的run(),其中target就是传入的参数，
        // 所以也就是调用了实现类重写的run()
        //new Thread(t4).start()
        //再启动一个线程
        Thread t2 = new Thread(target);//可以共用同一个实现类的对象，反正run()是一样的
        t2.setName("线程2");
        t2.start();
    }

}


class MyThread4 implements Runnable{

    @Override
    public void run() {
        for(int i = 0;i<100;i++){
            if(i%2==0){

                System.out.println(Thread.currentThread().getName()+":"+i);//不能直接getName了
            }
        }
        System.out.println("sad");
    }
}
```



​		**difference between extends Thread&implements Runnable**

### 	AFTER JDK5.0

#### 		3. implements Callable

```java
package com.landfill.java2;
/*
创建线程的方式三：实现Callable接口   ARTER JKD5.0

Callable接口创建线程比Runnable更强
可以有返回值
可以抛出异常，可以被外面的操作捕获，获取异常的信息
支持泛型的返回值
需要借助FutureTask类，比如获取返回结果


 */

import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;
//1.实现Callable接口
class NumThread implements Callable{
    //2.实现call方法，写入此线程要执行的操作
    @Override
    public Object call() throws Exception {
        int sum = 0;
        for (int i = 1; i <101 ; i++) {
            if(i%2 == 0){
                System.out.println(i);

                sum+= i;
            }

        }
        return sum;
    }
}

public class CallableTest {
    public static void main(String[] args) {
        //3.创建Callable实现类的对象
        NumThread t = new NumThread();
        //4.将Callable实现类的对象作为参数传入，创建 FutureTask对象
        FutureTask futureTask = new FutureTask(t);
        //5.将FutureTask对象传入，创建Thread对象，调用Start方法
        new Thread(futureTask).start();   //FutureTask类同时实现了Callable和Runnable，
        // 这里new Thread要求的返回值的形参是Runnable类型的，所以用FutureTask
        //6.如果需要返回值，通过FutureTask调用get()获得 实现的call()返回的对象
        try {
            //get()返回值即为FutureTask构造器参数Callable实现类重写的call()返回值。如果不需要返回值
            //可以不写下面的方法
            Object sum = futureTask.get();
            System.out.println("总和为"+sum);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
    }
}

```



##### 			

#### 		4. Thread Pool

​			ExecutorSercice API相关功能

```java
package com.landfill.java3;

import java.util.concurrent.*;

/*
创建线程的方式四：线程池  AFTER JDK5.0
开发中都是用线程池，手动的造线程效率差，和后面的数据库连接池是一个道理,很多时候是用框架实现的，不一定是手写的
1.问题：经常创建和销毁、使用量特别大的资源，比如并发情况下的线程，对性能影响很大
2.思路：提前创建好多个线程，放入线程池中，使用时直接获取，使用完放回池中。可以避免频繁创建销毁、实现重复利用。
3.好处：
    3.1 提高响应的速度：减少了创建线程的时间
    3.2 降低资源的消耗：重复利用线程池中线程  不需要每次都创建
    3.3 便于线程的管理
        - corePoolSize:核心池的大小
        - maximumPoolSize：最大线程数
        - keepAliveTime:线程没有任务时最多保持多长时间后会终止
创建多线程有几种方式：四种 继承Thread  实现Runnable 实现Callable  创建线程池。
 */
class NumberThread implements Runnable{

    @Override
    public void run() {
        for (int i = 0; i <100 ; i++) {
            if(i % 2 == 0){
                System.out.println(Thread.currentThread().getName()+":"+i);
            }
        }
    }
}
class Number1Thread implements Callable {

    @Override
    public Object call() {
        int sum = 0;
        for (int i = 0; i <100 ; i++) {
            if(i % 2 != 0){
                System.out.println(Thread.currentThread().getName()+":"+i);
                sum +=i;
            }
        }return sum;
    }
}


public class ThreadPool {
    public static void main(String[] args) {
        //1.创建指定数量的线程池
        ExecutorService service = Executors.newFixedThreadPool(10);
        //ExecutorService是接口，Executors是该接口的工具类，工厂类
        ThreadPoolExecutor service1  = (ThreadPoolExecutor) service;  //返回的实际是ThreadPoolExecutor的对象，可以向下转型

        //设置线程池的属性
        System.out.println(service.getClass());
        service1.setCorePoolSize(12);//就可以调用相关的方法去设置线程池的属性，管理线程
       // service1.setKeepAliveTime();

       // 2.传入相应的对象，执行指定线程的操作
        service.execute(new NumberThread());   //适合用Runnable
//        Number1Thread number1Thread = new Number1Thread();
//        FutureTask futureTask = new FutureTask(number1Thread);  // FutureTask来获取返回值
//        try {
//            Object sum = futureTask.get();
//            System.out.println("总和为"+sum);
//        } catch (InterruptedException e) {
//            e.printStackTrace();
//        } catch (ExecutionException e) {
//            e.printStackTrace();
//        }
//        service.submit(number1Thread);  //适合用Callable，

        //3.关闭线程池
        service.shutdown();

    }
}

```



## 三、线程安全

### sychronized

```java
package com.landfill.java;
/*
线程的同步
1.线程安全问题：重票和错票
2.出现原因：当某个线程操作车票的过程中，尚未完成操作，其他线程就进来了，也操作车票
3.解决方法：当一个线程A操作共享数据时，其他线程不能参与进来，知道线程A操作完，其他线程才可以进来，即使线程A出现阻塞也不能改变

4.在Java中，通过同步机制来解决线程安全问题
    方式一：同步代码块
    synchronized(同步监视器){
        //需要被同步的代码
        }
     说明: 1.操作共享数据的代码，即为需要被同步的代码   -->不能包含过多也不能过少，就变成单线程了
           2.共享数据 多个线程共同操作的变量
           3.同步监视器：锁 任何一个类的对象都可以充当锁，随便new一个object //为什么？
                要求：多个线程必须要共用同一把锁。都是同一个对象
                补充：实现Runnable接口创建的多线程中可以考虑使用this充当锁
                      在继承Thread创建多线程的方式中，慎用this，可以考虑使用当前类充当同步锁 类名.class

    方式二：同步方法
        如果操作共享数据的代码完整地声明在一个方法中，将方法声明为同步的

        关于同步方法的总结
        同步方法仍然涉及到同步监视器，只是不需要声明
        - 非静态的同步方法，同步监视器是this
        - 静态的同步方法，同步监视器是static
5.同步的方式：线程安全问题解决了；但操作同步代码时，只能有一个线程参与，其他线程等待，相当于是一个单线程的过程，效率低
 */
public class ThreadSynchronized {
}

```



#### 1. sychronized block

```java
package com.landfill.java;

import static java.lang.Thread.sleep;
//synchronize的使用 ：同步代码块  synchronize(同步监视器）{重复执行的方法}
//创建线程的方式之二：实现Runnable接口 卖票
//区别于继承Thread类，只用new一个实现类的对象。
public class WindowRunnable {
    public static int total = 100; //此时可以不用static，声明在实现类里的属性。因为只有一个对象，作为参数传递给了三个线程，所以还是只有一个属性,可以放在实现类里
    public static void main(String[] args) {
        window1 w1 = new window1();
        Thread t1 = new Thread(w1);
   //不用写三个类，同一个类new三个线程
        Thread t2 = new Thread(w1);
        Thread t3 = new Thread(w1);
        t1.setName("线程1");
        t2.setName("线程2");
        t3.setName("线程3");
        t2.start();
        t1.start();
        t3.start();




    }

}

class window1 implements Runnable{
    Object obj = new Object();
    @Override
    public void run() {
        while (true) {
            synchronized (this) {
                if (WindowRunnable.total > 0) {
                    //this是唯一的对象，只有用于实现Runnable接口的方法

                    try {
                        sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    WindowRunnable.total--;
                    System.out.println(Thread.currentThread().getName() + ":剩余" + WindowRunnable.total + "张票" + " 你的票号为：" +
                            (WindowRunnable.total + 1));
                }else break;
            }
        }
    }
}

```



#### 2 .sychronized method

```java
package com.landfill.java;
/*
使用同步方法解决实现Runnable接口的线程安全问题。
 */
public class WindowImpleMethod {
    public static void main(String[] args) {
        Window w = new Window();
        Thread t1 = new Thread(w);
        Thread t2 = new Thread(w);
        Thread t3= new Thread(w);
        t1.setName("线程1");
        t2.setName("线程2");
        t3.setName("线程3");
        t1.start();
        t2.start();
        t3.start();
    }
}


class Window implements Runnable{

    @Override
    public void run() {
        while(WindowTest.TOTAL_TICKETS>0){
            show();

        }
    }

    private synchronized void show(){  //同步监视器是this
        if (WindowTest.TOTAL_TICKETS > 0) {
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            WindowTest.TOTAL_TICKETS--;
            System.out.println(Thread.currentThread().getName() + " left:" + WindowTest.TOTAL_TICKETS);
        }
    }
}

```

```java
package com.landfill.java;
/*
Thread类继承创建的线程 使用同步方法解决线程安全问题。
 */
public class WindowExtMethod {

    public static void main(String[] args) {
        windows2 w1 = new windows2();
        windows2 w2 = new windows2();
        windows2 w3 = new windows2();
        w3.start();
        w2.start();
        w1.start();


    }
}

class windows2 extends Thread {
    private static int ticket = 100;

    @Override

    public void run() {
        while (true) {
            show();
        }
    }

   // private static  void show(){  //锁的问题，同步监视器有三个了
        private static synchronized void show(){   //this是当前的类
        if (ticket > 0) {
            try {
                sleep(1000);  //父类的静态方法，可以直接调用
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            ticket--;
            System.out.println(Thread.currentThread().getName() + " left:" + ticket);
            //静态方法内部不用直接调用动态方法，需通过 对象.方法 的方式来调用
        }

    }
}

```



#### 单例模式懒汉式改写成线程安全的

```java
package com.landfill.java1;
/*
使用同步机制将单例模式中的懒汉式改写为线程安全的
 */
public class BankTest {

}
class Bank{

    private Bank(){

    }
    private static Bank bank = null;

    public static synchronized Bank getBank(){  //可能几个线程来同时调用，就会存在线程安全问题,静态同步方法的锁是类本身
        //方式一：同步代码块，效率差，每次都会进去判断，和直接用同步方法一样，效率低
//        synchronized(Bank.class) {
//            if (bank == null) {
//                bank = new Bank();
//            }
//            return bank;
//        }
        //方式二：效率更高
        if(bank == null){     //后面的大部分线程就不进去了  效率更高
            synchronized (Bank.class){
                if (bank == null) {
                bank = new Bank();
               }

            }
        }return bank;
    }


}

```



### ReentrantLock(AFTER JDK5.0)

#### 3. lock/unlock

```java
package com.landfill.java1;

import java.util.concurrent.locks.ReentrantLock;

/*
解决线程安全问题的方式三：Lock锁   --AFTER JDK5.0
synchronized 和 lock的异同？
相同：都可以解决线程安全的问题
不同：synchronized 等执行完代码块或者同步方法，再自动释放同步监视器
      lock需要手动的启动同步，手动的结束同步 unlock
ReentrantLock是Lock接口的实现类，扩展性更好，更多子类

建议优先度：lock  同步代码块（已经进入方法体） 同步方法

 */
class Window implements Runnable{

    private int tickets = 100;
    //1.实例化reentrantlock
    private ReentrantLock lock = new ReentrantLock(true);   //fair 先进先出
    @Override
    public void run() {
        while (true){
           try{
               lock.lock();         //使用try-catch结构，加锁
               if(tickets>0){
                   try {
                       Thread.sleep(100);
                   } catch (InterruptedException e) {
                       e.printStackTrace();
                   }
                   System.out.println(Thread.currentThread().getName()+"票号为："+tickets);
                   tickets--;
               }else break;
           } finally {
               lock.unlock();       //解锁
           }

        }
    }
}
public class LockTest {
    public static void main(String[] args) {
        Window w = new Window();
        Thread t1 = new Thread(w);
        Thread t3 = new Thread(w);
        Thread t2 = new Thread(w);
        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");
        t1.start();
        t2.start();
        t3.start();
    }
}

```



​		difference between ReentrantLock&sychronized

### 线程同步问题DeadLock

```java
package com.landfill.java1;
/*
线程的死锁问题
定义：不同的线程分别占用对方需要的同步资源不放弃，都在等待对方放弃自己需要的同步资源，就形成的线程的死锁
出现死锁后不会出现异常，不会提示，只是所有线程都处于阻塞状态，无法继续，使用时要避免死锁

解决方法
1.专门的算法、原则
2.尽量减少使用同步资源
3.避免嵌套同步
 */
public class DeadLock {

    public static void main(String[] args) {
        StringBuffer s1 = new StringBuffer();
        StringBuffer s2 = new StringBuffer();


        new Thread(){
            @Override
            public void run() {
                synchronized (s1){
                    s1.append("a");
                    s2.append("1");

                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                    synchronized (s2){
                        s1.append("b");
                        s2.append("2");
                        System.out.println(s1);
                        System.out.println(s2);
                    }
                }
            }
        }.start();

        new Thread(new Runnable() {
            @Override
            public void run() {
                synchronized (s2){
                    s1.append("c");
                    s2.append("3");
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    synchronized (s1){
                        s1.append("d");
                        s2.append("4");
                        System.out.println(s1);
                        System.out.println(s2);
                    }
                }
            }
        }).start();

    }
}

```

```java
package com.landfill.java1;

class A {
	public synchronized void foo(B b) {  //锁 A类的对象 a
		System.out.println("当前线程名: " + Thread.currentThread().getName()
				+ " 进入了A实例的foo方法"); // ①
		try {
			Thread.sleep(200);
		} catch (InterruptedException ex) {
			ex.printStackTrace();
		}
		System.out.println("当前线程名: " + Thread.currentThread().getName()
				+ " 企图调用B实例的last方法"); // ③
		b.last();
	}

	public synchronized void last() {    //同步监视器 a
		System.out.println("进入了A类的last方法内部");
	}
}

class B {
	public synchronized void bar(A a) {  //同步监视器 b
		System.out.println("当前线程名: " + Thread.currentThread().getName()
				+ " 进入了B实例的bar方法"); // ②
		try {
			Thread.sleep(200);
		} catch (InterruptedException ex) {
			ex.printStackTrace();
		}
		System.out.println("当前线程名: " + Thread.currentThread().getName()
				+ " 企图调用A实例的last方法"); // ④
		a.last();
	}

	public synchronized void last() {   //同步监视器 对象b
		System.out.println("进入了B类的last方法内部");
	}
}

public class DeadLock1 implements Runnable {
	A a = new A();
	B b = new B();

	public void init() {
		Thread.currentThread().setName("主线程");
		// 调用a对象的foo方法
		a.foo(b);
		System.out.println("进入了主线程之后");
	}

	public void run() {
		Thread.currentThread().setName("副线程");
		// 调用b对象的bar方法
		b.bar(a);
		System.out.println("进入了副线程之后");
	}

	public static void main(String[] args) {
		DeadLock1 dl = new DeadLock1();
		new Thread(dl).start();
		dl.init();
	}
}

```



​		

## 四、线程通信

```java
package com.landfill.java3;

import static java.lang.Thread.sleep;

/*
线程通信
两个线程交替打印1-100。

涉及的三个方法
wait(): 一旦执行此方法，线程阻塞，释放同步监视器
notifyAll()：一旦执行此方法，唤醒所有被wait的线程
notify();只唤醒一个线程，如果有多个wait的线程，就唤醒优先级高的线程

1.使用前提：只能在同步方法和同步代码块中的调用，使用ReentrantLock以另外的方式通信
2.三个方法的调用者必须是同步方法和同步代码块中的同步监视器，否则会报错
//java.lang.IllegalMonitorStateException，this和同步监视器不一样的话会报错
在类中调用方法的时候，非静态方法，省略了“this，” 静态方法省略了 “类.”
3.因为任何一个类的对象都可以充当同步监视器，所以任何一个对象都得有这三个方法，所以这三个方法时定义在java.lang.Object类中的。

4.比较：sleep()和wait()的异同
  相同：当前线程都会进入阻塞状态
  不同：在都用在同步代码块和同步方法中的时候，sleep()不会释放锁，wait()会释放锁
         sleep()声明在Thread且是静态的，wait()声明在Object类
         sleep()可以在任何需要的场景下调用，wait()必须使用在同步代码块和同步方法中
 */
class Number implements Runnable{
    private int num = 1;
    Object obj = new Object();
    @Override
    public void run() {
        while (true){
        synchronized (obj) {

                obj.notifyAll();   //notifyAll()唤醒所有线程，按优先级
                //java.lang.IllegalMonitorStateException
                if(num<=100){
                    try {
                        sleep(10);    //不释放锁，不同于wait
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName()+":"+num);
                    num++;

                    try {
                        obj.wait();                          //使得调用该方法的线程进入阻塞状态，一旦执行wait() 会释放锁
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }


                }else break;
            }
        }

    }
}

public class ThreadComumnication {
    public static void main(String[] args) {
        Number num = new Number();
        Thread t1 = new Thread(num);
        Thread t2 = new Thread(num);
        t1.setName("线程1");
        t2.setName("线程2");
        t1.start();
        t2.start();
    }
}

```

### producer-consumer problem

```java
package com.landfill.java3;
/*
线程通信的应用： Producer-Consumer problem(Bounded-buffer problem)

分析：生产者线程  消费者线程
共享数据：clerk
解决线程安全问题：同步机制
涉及线程的通信


 */
class Clerk {
    private int num = 0;
    //成产
    public synchronized void procudeProcuct() {

        if(num<20){
            num++;
            System.out.println(Thread.currentThread().getName()+":生产产品"+num);
            notify();
        }else{//等待
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
    //消费
    public synchronized void consumeProcuct() {
        notify();
        if(num>0){
            System.out.println(Thread.currentThread().getName()+":消费产品"+num);
            num--;
            notify();
        }else{//等待
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}


class Producer implements Runnable{
    private Clerk clerk;
    public Producer(Clerk clerk){
        this.clerk = clerk;
    }

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName()+":开始成产产品");
        while (true){
            clerk.procudeProcuct();
        }
    }
}

class Consumer implements Runnable {
    private Clerk clerk;

    public Consumer(Clerk clerk) {
        this.clerk = clerk;
    }

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + ":开始消费产品");
        while (true) {
            clerk.consumeProcuct();

        }
    }
}


public class ProducerConsumer {

    public static void main(String[] args) {
        Clerk c = new Clerk();
        Producer p1 = new Producer(c);
        Consumer c1 = new Consumer(c);
        Thread t1 = new Thread(p1);
        Thread t2 = new Thread(c1);
        t1.setName("生产者");
        t2.setName("消费者");
        t1.start();
        t2.start();
    }
}


```



