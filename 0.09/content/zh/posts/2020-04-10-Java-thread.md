---
title: Java 多线程编程
date: 2020-04-10
description: Java 多线程
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: lin
authorEmoji: 🤣
type: theme
tags:
- Java
#类别
categories:
- Java
#系列
image: https://y.gtimg.cn/music/photo_new/T002R300x300M000003Mowrk2QZgq5_2.jpg
---



## 进程与线程

### 进程

**进程**（Process）是计算机中的程序关于某数据集合上的一次运行活动，是系统进行资源分配和调度的基本单位，是[操作系统]结构的基础。在早期面向进程设计的计算机结构中，进程是程序的基本执行实体；在当代面向线程设计的计算机结构中，进程是线程的容器。程序是指令、数据及其组织形式的描述，进程是程序的实体。

### 线程

**线程**（Thread）是[操作系统]能够进行运算调度的最小单位。它被包含在[进程]之中，是[进程]中的实际运作单位。一条线程指的是[进程]中一个单一顺序的控制流，一个进程中可以并发多个线程，每条线程并行执行不同的任务。

### 主线程

**主线程** (Main Thread）是当一个程序启动时，就有一个进程被操作系统（OS）创建，与此同时一个线程也立刻运行,因为它是程序开始时就执行的，如果你需要再创建线程，那么创建的线程就是这个主线程的子线程。每个进程至少都有一个主线程！

### 子线程
**子线程**   (Sub Thread)  同上，即被进程创建的除主线程外的线程。

## 创建子线程

Java中，创建一个子线程一般涉及到`java.lang.Thread`类和`java.lang.Runnable`接口。

Thread 即线程类，每当创建一个Thread对象即产生一个新的子线程。

线程所运行的代码由`Runnable`接口的`run()` 方法实现。

我们需要对`run()` 进行重写，以驱动子线程。`run()`方法也被称为线程体。

下面将介绍几种关于创建`Thread`的应用示例。

### 获取主线程

```java
// 获取主线程示例

Thread mainThread = Thread.currentThread();

System.out.println("主线程名 : " + mainThread.getName()) ;
```

### 使用线程类

 在一个类中使用`Runnable接口`，在这个类中重写接口的`run()`.

```java
//currentThread.getName()  这个方法 将返回子线程的名字[String]。
//Thread.sleep(long millis) 这个是Thread类的静态方法，将使当前线程进入休眠。
//提示： 可以注释掉sleep 来观察区别
public class Runner implements Runnable {
    @Override
    public void run() {
        //打印次数和线程名字
        for( int i = 0 ;i < 10; i++) {
            System.out.printf("第 %d 次执行 - %s \n" ,i ,Thread.currentThread().getName()); 

            try{
            //随机生成时间
                long sleepTime = (long)(1000 * Math.random());
                Thread.sleep (sleepTime);
            //线程休眠
            } catch (InterruptedException e) { }


        }
        //线程执行结束
        System.out.println("执行完成" + Thread.currentThread().getName());
    }
}
```

```java
/**
	所使用的的是Thread类中的这两个构造方法。
	Thread(Runnable target);
	Thread(Runnable target , String Name);
	start(); 子线程调整到可运行状态
**/
//调用示例
Thread t1 = new Thread(new Runner());
t1.start();

Thread t2 = new Thread(new Runner(), "Lucky girl");
t2.start();
```

### 继承线程类

Thread类其实也实现了`Runnable 接口`，所以`Thread`类也可以作为线程执行对象，我们可以通过让类继承 `Thread`来重写`run()`方法来创建子线程。

```java
public class Luckygirl extends Thread {
    // 继承线程类
    public Luckygirl(){
        super();
    }
    //构造
    public Luckygirl(String name){
        super(name);
    }
  
    @Override
    public void run() {
        for (int i = 0 ; i<10 ; i++){
            System.out.printf("第%d次执行 - %s \n" , i , super.getName());

            try{
                //随机生成时间
                long sleepTime = (long)(1000 * Math.random());
                Thread.sleep (sleepTime);
                //线程休眠
            } catch (InterruptedException e) { }
        }
        //线程执行结束
        System.out.println("执行完成" + Thread.currentThread().getName());
    }
}
```

```Java
//调用示例
Thread t1 = new Luckygirl();
t1.start();

Thread t2 = new Luckygirl("Lucky girl");
t2.start();
```

### 匿名内部类

如果线程体的任务并不多，我们实在没有必要都给它单独写一个类。可以使用匿名内部类或Lambda表达式直接实现Runnable接口。

```Java
// 匿名内部类使用示例
public static void main(String [] args ) {
Thread t1 = new Thread( new Runnable(){
    @Override
    public void run() {
        for(int i = 0 ;i<10 ; i++){
            //打印次数和线程的名字
            System.out.printf("第 %d 次 执行 - %s\n",i,Thread.currentThread().getName());
            try{
                long sleepTime = (long) (1000* Math.random());
                Thread.sleep(sleepTime);
            }catch(InterruptedException e){}
        }
        System.out.printf("执行完成！ " + Thread.currentThread().getName());
    }
},
"Luck girl");
t1.start();
}
```

### Lambda传参

```java
// 使用Lambda表达式传递Runnable函数参数
Thread t2 = new Thread(() -> {
    for (int i = 0; i < 10; i++) {
        //打印次数和线程的名字
        System.out.printf("第 %d 次 执行 - %s\n", i, Thread.currentThread().getName());
        try {
            long sleepTime = (long) (1000 * Math.random());
            Thread.sleep(sleepTime);
        } catch (InterruptedException e) {
        }
    }
    System.out.printf("执行完成！ " + Thread.currentThread().getName());
},
"Luck girl");
t2.start();
```

## 线程状态

[Java]中的线程的生命周期大体可分为5种状态。

1. **新建(NEW)**：新创建了一个线程对象。

2. **就绪(RUNNABLE)**：线程对象创建后，其他线程(比如main线程）调用了该对象的start()方法。该状态的线程位于可运行线程池中，等待被线程调度选中，获取cpu 的使用权 。

3. **运行(RUNNING)**：可运行状态(runnable)的线程获得了cpu 时间片（timeslice） ，执行程序代码。
4. **阻塞(BLOCKED)**：阻塞状态是指线程因为某种原因放弃了cpu 使用权，也即让出了cpu timeslice，暂时停止运行。直到线程进入可运行(runnable)状态，才有机会再次获得cpu timeslice 转到运行(running)状态。阻塞的情况分三种： 
> (一). 等待阻塞：运行(running)的线程执行o.wait()方法，JVM会把该线程放入等待队列(waitting queue)中。
> (二). 同步阻塞：运行(running)的线程在获取对象的同步锁时，若该同步锁被别的线程占用，则JVM会把该线程放入锁池(lock pool)中。
> (三). 其他阻塞：运行(running)的线程执行Thread.sleep(long ms)或t.join()方法，或者发出了I/O请求时，JVM会把该线程置为阻塞状态。当sleep()状态超时、join()等待线程终止或者超时、或者I/O处理完毕时，线程重新转入可运行(runnable)状态。

5. **死亡(DEAD)**：线程`run()、main()` 方法执行结束，或者因异常退出了`run()`方法，则该线程结束生命周期。死亡的线程不可再次复生。

## 线程管理

### 优先级

 线程的调度程序提供线程决定每次线程应当何时运行，Java提供了10种优先级，分别用1~10 的整数表示，提供了最高   `MAX_PRIORITY = 10` 、最低 `MIN_PRIORITY = 1`、默认 `NORM_PRIORITY = 5` 三种。

Thread 类提供了 `setPriority(int newPriority)`方法用以设置线程优先级，通过 `getPriority()`方法可以获得线程优先级。

```java
//示例：

public static void main(String [] args){
        Thread t1 = new Thread(new Runner());
        t1.setPriority(Thread.MAX_PRIORITY); //level 10
        t1.start();

        Thread t2 = new Thread(new Runner(),"Lucky girl");
        t2.setPriority(Thread.MIN_PRIORITY); //level 1
        t2.start();
    }
    //静态内部类
    public static class Runner implements Runnable {
        @Override
        public void run() {
            //打印次数和线程名字
            for( int i = 0 ;i < 10; i++) {
                System.out.printf("第 %d 次执行 - %s \n" ,i ,Thread.currentThread().getName());

                try{
                    //随机生成休眠时间
                    long sleepTime = (long)(1000 * Math.random());
                    Thread.sleep (sleepTime);
                    //线程休眠
                } catch (InterruptedException e) { }
            }
            //线程执行结束
            System.out.println("执行完成" + Thread.currentThread().getName());
        }
    }
}
```

### 等待结束

在线程状态-阻塞状态中有提及到`join()`这个方法，当前线程调用t1线程的`join()`方法，则会阻塞当前线程，等待t1的线程任务结束，如果t1线程结束或等待超时，则当前线程回到就绪状态。

```java
public class HelloWorld {
    static int value = 0;
    public static void main(String [] args) throws InterruptedException{
    Thread t1 = new Thread(() -> {
        System.out.println(" Lucky gril！！！！");
        for(int i = 0; i < 2; i++){
            System.out.println("Lucky gril 冲冲冲！");
            value = value +1;
        }
        System.out.println(" Lucky gril 比赛胜利！");
      
    },"Lucky gril");
       t1.start();
				//观察 变量-Value
        //假设不使用join或者sleep休眠等方式让主线程延迟到子线程结束，则主线程将会返回0，因为其没有权限得到value的值 也就是线程阻塞现象。

       //t1.join(0);
       //t1.sleep(1000);

        System.out.println("Value  = " + value);
        System.out.println("Lucky gril 万岁！！！！");
    }
}
```

### 让步

Thread 类提供了一个静态方法`yield()`，调用`yield()` 能够使当前线程给其它线程让步，但它只给相同优先级或更高优先级的线程提供机会运行。它和`sleep`看起来很类似，但是`sleep`不论其它线程任何等级都会有机会运行，更重要的是，`sleep`可以控制时间，但`yield()`却不能，仔细观察输出结果，就可以发现这个特点。

```java
public class Runner implements Runnable {
    @Override
    public void run() {
        for(int i = 0;i<10;i++)
        {
            //打印次数和线程的名字
            System.out.printf("第%d次执行 - %s \n",i,Thread.currentThread().getName());
            System.out.printf(Thread.currentThread().getName() + "：我累了，你先帮我顶一下。\n" );
            Thread.yield();
            /** if(Thread.currentThread().getName().equals("Lucky girl"))
            Thread.yield(); **/
        }
        //线程执行结束
        System.out.println("执行完成！" + Thread.currentThread().getName());
    }

}
```

```java
public class HelloWorld {
    public static void main(String[] args) {
        Thread t1 = new Thread(new Runner(), "Lucky girl");
        t1.start();

        Thread t2 = new Thread(new Runner());
        t2.start();
    }
}
```

### 停止

***<u>虽然Thread提供了`stop()`的方法，但它已经不被推荐使用，因为这个方法有时会引发严重的系统故障，类似的还有 `suspend()` 和 `resume()`挂机方法。Java现在推荐的做法就是采用本例的结束变量方式。</u>***

线程体中的run方法将结束，线程就会进入死亡状态，线程便停止了，但是有些业务比较复杂，例如想开发一个下载程序，每隔一段执行下载任务，下载任务一般会由子线程执行，休眠一段时间再执行。 这个下载子线程就会有一个死循环，为了能够停止子线程，需要设置一个结束变量。

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class HelloWorld {
    //想线程进入死亡状态 只需要让RUN方法结束，虽然Thread提供了stop()，但我不推荐使用。
    
    private static String command = "";
    public static void main(String[] args){
        Thread t1 = new Thread(() -> {
            while (!command.equalsIgnoreCase("exit")) {
                //输出"exit" 停止线程
                System.out.println("下载中！");
                try {
                    Thread.sleep(10000);
                } catch (InterruptedException e) {
                }
            }
            System.out.println("执行完成！");

        },"InkHin!");

        t1.start();

        try(InputStreamReader ir = new InputStreamReader(System.in);
            BufferedReader in = new BufferedReader(ir)){

            command = in.readLine();
        } catch(IOException e){}
    }
}
```

## 线程安全

在多线程的环境下，访问相同的资源，就有可能引发线程不安全问题。

### 共享资源

多个线程同时运行，有时线程之间需要共享数据，一个线程需要其他线程的数据，否则就不能保证程序运行结果的正确性。

以这个模拟销售机票系统为例,

```Java
public class TicketDB {
    // 机票的数量
    private int ticketCount = 5;
    // 获得当前机票数量
    public int getTicketCount() {
        return ticketCount;
    }
    // 销售机票
    public void sellTicket() {
        try {
            // 等于用户付款
            // 线程休眠，阻塞当前线程，模拟等待用户付款
            Thread.sleep(1000);
        } catch (InterruptedException e) {
        }
        System.out.printf("第%d号票,已经售出\n", ticketCount);
        ticketCount--;
    }
}
```

```Java
public static void main(String[] args) {
        TicketDB db = new TicketDB();
        // 创建线程t1
        Thread t1 = new Thread(() -> {
            while (true) {
                int currTicketCount = db.getTicketCount();
                // 查询是否有票
                if (currTicketCount > 0) {
                    db.sellTicket();
                } else {
                    // 无票退出
                    break;
                }
            }
        });
        // 开始线程t1
        t1.start();
        // 创建线程t2
        Thread t2 = new Thread(() -> {
            while (true) {
                int currTicketCount = db.getTicketCount();
                // 查询是否有票
                if (currTicketCount > 0) {
                    db.sellTicket();
                } else {
                    // 无票退出
                    break;
                }
            }
        });
        // 开始线程t2
        t2.start();
    }
```

运行以后，有时会出现重复票号，甚至会出现第0号票，这些问题都是多个线程间共享的数据导致数据不一致造成的。

### 多线程同步

为了解决上面这种安全问题，Java提供了“互斥”机制，可以为这些资源对象加上一把“互斥锁”，在任一时刻只能由一个线程访问，即使该线程出现阻塞，该对象的被锁定状态也不会被解除，其他线程仍不能访问该对象，这就是多线程同步。

可以通过两种方式来实现线程同步，两种方式都涉及使用`synchronized`关键字，一种是`synchronized`方法，使用`synchronized`关键字修饰方法，对方法进行同步；另一种是`synchronized`语句，将`synchronized`关键字放在对象前面限制一段代码的执行。

```java
//多线程同步 方法一
public class TicketDB {
    private int ticketCount = 5; //机票数量
    //获取当前机票
    public synchronized int getTicketCount(){
        return ticketCount;
    }
    //销售机票
    public synchronized void sellTicket(){
        try{
            //线程休眠 模拟用户付款
            Thread.sleep(1000);

        } catch(InterruptedException e){
        }
        System.out.printf("第%d号票,已经售出\n",ticketCount);
        ticketCount--;
    }

}
```

```Java
//多线程同步 方法二
public static void main(String[] args) {
    TicketDB db = new TicketDB();
    Thread t1 = new Thread(() -> {
        while (true) {
            synchronized (db) {
                int currTicketCount = db.getTicketCount();
                if (currTicketCount > 0) {
                    db.sellTicket();
                } else {
                    break;
                }
            }
        }
    }, "窗口A");
    t1.start();
    Thread t2 = new Thread(() -> {
        while (true) {
            synchronized (db) {
                int currTicketCount = db.getTicketCount();
                // 查询是否有票
                if (currTicketCount > 0) {
                    db.sellTicket();
                } else {  // 无票退出
                    break;
                }
            }
        }
    }, "窗口B");
    t2.start();
}
```

## 线程间通信

多线程同步的做法只是简单地为特定对象或方法加锁，但有时候情况会更加复杂，如果两个线程之间有依赖关系，线程之间必须进行通信，互相协调才能完成工作。

以一个经典的堆栈问题为例:
一个线程生产了数据，将数据压栈，另一个线程消费了这些数据，将数据出栈。

这两个线程互相依赖，当堆栈为空，消费线程无法取出数据时，应该告诉生产线程去添加数据；当堆栈满了，生产线程无法添加数据时，应该告诉消费线程来取出数据。

为了实现线程间的通信，需要使用Object类声明的5个方法；

```Java
void  wait () //使当前线程释放对象锁，然后当前线程处于对象等待队列中的阻塞状态。
void  wait (long timeout) //同上，等待timeout毫秒
void  wait (long time out,int nanos) //同上，等待timeout毫秒 加 nanos 纳秒
void  notify () //当前线程唤醒此对象等待队列中的一个线程，该线程将进入就绪状态(start)
void  notifyAll () //当前线程唤醒此对象等待队列中的所有线程，这些线程将进入就绪状态(start)
```

 示例：

```Java
class Stack {
    // 堆栈指针初始值为0
    private int pointer = 0;
    // 堆栈有5个字符的空间
    private char[] data = new char[5];

    // 压栈方法，加上互斥锁
    public synchronized void push(char c) {
        // 堆栈已满，不能压栈
        while (pointer == data.length) {
            try {
                // 等待，直到有数据出栈
                this.wait();
            } catch (InterruptedException e) {
            }
        }
        // 通知其它线程把数据出栈
        this.notify();
        // 数据压栈
        data[pointer] = c;
        // 指针向上移动
        pointer++;
    }

    // 出栈方法，加上互斥锁
    public synchronized char pop() {
        // 堆栈无数据，不能出栈
        while (pointer == 0) {
            try {
                // 等待其它线程把数据压栈
                this.wait();
            } catch (InterruptedException e) {
            }
        }
        // 通知其它线程压栈
        this.notify();
        // 指针向下移动
        pointer--;
        // 数据出栈
        return data[pointer];
    }
}
```

```Java
public static void main(String args[]) {
    Stack stack = new Stack();
    // 下面的消费者和生产者所操作的是同一个堆栈对象stack
    // 生产者线程
    Thread producer = new Thread(() -> {
        char c;
        for (int i = 0; i < 10; i++) {
            // 随机产生10个字符
            c = (char) (Math.random() * 26 + 'A');
            // 把字符压栈
            stack.push(c);
            // 打印字符
            System.out.println("生产: " + c);
            try {
                // 每产生一个字符线程就睡眠
                Thread.sleep((int) (Math.random() * 1000));
            } catch (InterruptedException e) {
            }
        }

    });
    // 消费者线程
    Thread consumer = new Thread(() -> {
        char c;
        for (int i = 0; i < 10; i++) {
            // 从堆栈中读取字符
            c = stack.pop();
            // 打印字符
            System.out.println("消费: " + c);
            try {
                // 每读取一个字符线程就睡眠
                Thread.sleep((int) (Math.random() * 1000));
            } catch (InterruptedException e) {
            }
        }

    });
    producer.start(); // 启动生产者线程
    consumer.start(); // 启动消费者线程
}
```