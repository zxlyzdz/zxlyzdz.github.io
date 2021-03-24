---
layout: post
title: 多线程
date: 2021-03-24
Author: 龙龙
categories: 
tags: [Java]
comments: true
---



### 1. 线程简介

**进程**：在我们的电脑中运行的程序就是进程，比如QQ，播放器，IDE等等。进程之间是相互独立存在的。

**线程**：进程想要执行任务就需要依赖线程。换句话说，就是进程中的最小执行单位就是线程，并且一个进程中至少有一个线程。线程是CPU调度和执行的单位。

**多线程**：指的是程序(一个进程)运行时产生了多个线程。

* 并行与并发
* 并行：多个CPU或者多台机器同时执行一段处理逻辑，这是真正的同时。
* 并发：通过CPU调度算法，让用户看上去是在同时执行，实际上从CPU操作层面不是真正的同时。

很多多线程都是模拟出来的，真正的多线程是指有多个CPU，即多核。如果是模拟出来的多线程，在一个CPU的情况下，在同一个时间点，CPU只能执行一个代码，因为切换的很快，所以就有了同时执行的错觉。

* 线程就是独立的执行路径
* 在程序运行时，即使没有自己创建线程，后台也会有多个线程，如主线程，gc线程
* main()称之为主线程，为系统的入口，用于执行整个程序
* 在一个进程中，如果开辟了多个线程，线程的运行由调度器安排调度，调度器是与操作系统紧密相关的，先后顺序是不能人为的干预的。
* 对同一份资源操作时，会存在资源抢夺问题，需要加入并发控制
* 线程会带来额外的开销，如CPU调度时间，并发控制开销
* 每个线程在自己的工作内存交互，内存控制不当会造成数据不一致

### 2. 线程实现[重点]

线程创建方式有三种：

* 继承Thread类
* 实现Runnable接口
* 实现Callable接口

更推荐使用实现Runnable接口的方式实现多线程

* 继承 Thread 类
  * 子类继承 Thread 类具备多线程能力
  * 启动线程：子类对象.start()
  * **不建议使用：避免 OOP 单继承局限性**

* 实现 Runnable 接口
  * 实现接口 Runnable 具有多线程能力
  * 启动线程：传入目标对象 + Thread对象.start()
  * **推荐使用：避免单继承局限性，灵活方便，方便同一个对象被多个线程使用**

**具体实现**

1. 继承Thread类

   * 自定义线程类继承 Thread 类
   * 重写 run()  方法
   * 创建线程对象，调用 start() 方法启动线程

   ```java
   package demo01;	
   
   // 创建线程方式1：继承Thread类，重写Run()方法，调用 start() 启动线程
   // 总结：线程是同时进行的，但是县城开启之后不一定会立即执行，由CPI调度执行
   public class TestThread1 extends Thread {
       @Override
       public void run() {
           // run 方法线程体
           for (int i = 0; i < 5; i++) {
               System.out.println("我在看代码 ----- "+i);
           }
       }
   
       public static void main(String[] args) {
           // 创建自定义线程对象
           TestThread1 testThread1 = new TestThread1();
           // 调用 start() 方法开启线程
           testThread1.start();
   
           // main 线程，主线程
           for (int i = 0; i < 2000; i++) {
               System.out.println("我在学习多线程 " + i);
           }
       }
   }
   ```

   

   ![image-20210323155923684](C:\Users\周仙龙\AppData\Roaming\Typora\typora-user-images\image-20210323155923684.png)

2. 实现Runnable接口

   * 定义 MyRunnable 类实现Runnable接口

   * 实现Run方法

   * 创建线程对象，调用 start() 方法启动线程

     ```java
     package demo01;
     
     // 创建线程方式2：实现Runnable接口，重写 run() 方法，执行线程需要丢入 runnable 接口实现类，调用 start() 方法
     
     public class TestThread3 implements Runnable  {
         @Override
         public void run() {
             // run 方法线程体
             for (int i = 0; i < 5; i++) {
                 System.out.println("我在看代码 ----- "+i);
             }
         }
     
         public static void main(String[] args) {
             // 创建 Runnable 接口的实现类对象
             TestThread3 testThread3 = new TestThread3();
     
             // 创建线程对象，通过线程对象来开启我们的线程。代理
             Thread thread = new Thread(testThread3);
     
             // 启动线程
             thread.start();
     
             // main 线程，主线程
             for (int i = 0; i < 2000; i++) {
                 System.out.println("我在学习多线程 " + i);
             }
         }
     }
     
     ```

     

     ![image-20210323184151226](C:\Users\周仙龙\AppData\Roaming\Typora\typora-user-images\image-20210323184151226.png)

3. 实现Callable接口

   * 实现 Callable 接口，需要返回类型

   * 重写 call 方法，需要抛出异常

   * 创建目标对象

   * 创建执行服务：ExecutorService ser = Executors.newFixedThreadPool(1);

   * 提交执行：Future<Boolean>result1 = ser.submit(t1);

   * 获取结果：boolean r1 = result1.get()

   * 关闭服务：ser.shutDownNow();

     ```java
     package demo02;
     
     import org.apache.commons.io.FileUtils;
     
     import java.io.File;
     import java.io.IOException;
     import java.net.URL;
     import java.util.concurrent.*;
     
     // 线程创建方式3：实现 callable 接口
     /*
     callable 的好处：
     1. 可以定义返回值
     2. 可以抛出异常
      */
     public class TestCallable implements Callable<Boolean> {
     
         private String url; // 网络图片地址
         private String name; // 保存的文件名
     
         // 构造方法
         public TestCallable(String url, String name) {
             this.url = url;
             this.name = name;
         }
     
         // 下载图片线程的线程体
         @Override
         public Boolean call() {
             WebDownloader webDownloader = new WebDownloader();
             webDownloader.downloader(url, name);
             System.out.println("下载文件名为"+name);
             return true;
         }
     
         // 主函数
         public static void main(String[] args) throws ExecutionException, InterruptedException {
             TestCallable t1 = new TestCallable("https://i0.hdslb.com/bfs/vc/c1e19150b5d1e413958d45e0e62f012e3ee200af.png", "1.png");
             TestCallable t2 = new TestCallable("https://i0.hdslb.com/bfs/vc/c1e19150b5d1e413958d45e0e62f012e3ee200af.png", "2.png");
             TestCallable t3 = new TestCallable("https://i0.hdslb.com/bfs/vc/c1e19150b5d1e413958d45e0e62f012e3ee200af.png", "3.png");
     
             // 创建执行服务
             ExecutorService service = Executors.newFixedThreadPool(3);
     
             // 提交执行
             Future<Boolean> r1 = service.submit(t1);
             Future<Boolean> r2 = service.submit(t2);
             Future<Boolean> r3 = service.submit(t3);
     
             // 获取结果
             boolean rs1 = r1.get();
             boolean rs2 = r2.get();
             boolean rs3 = r3.get();
             
             // 关闭服务
             service.shutdownNow();
         }
     }
     
     
     // 下载器
     class WebDownloader{
         // 下载方法
         public void downloader(String url, String name) {
             try {
                 FileUtils.copyURLToFile(new URL(url), new File(name));
             } catch (IOException e) {
                 e.printStackTrace();
                 System.out.println("IO异常，downloader方法出现问题");
             }
         }
     }
     
     ```

     ![image-20210323191232922](C:\Users\周仙龙\AppData\Roaming\Typora\typora-user-images\image-20210323191232922.png)

**lambda表达式**

* 避免匿名内部类定义过多
* 其实质是函数式编程的概念

**为什么要使用lambda表达式**

* 避免匿名内部类定义过多
* 可以让代码看起来更简洁
* 去掉了一堆没有意义的代码，只留下核心的逻辑

**函数式接口**

* 定义：

  * 任何接口，如果只包含唯一一个抽象方法，那么它就是一个函数式接口，比如 Runnable 接口，只有一个抽象方法 run()，所以它就是函数式接口
  * 对于函数式接口，我们可以通过 lambda 表达式来创建接口的对象
  * 理解函数式接口是学习 Java8 lambda 表达式的关键所在

  ```java
  package lambda;
  
  public class TestLambda2 {
      // 3. 静态内部类
      static class Love1 implements ILove {
          @Override
          public void love(int a) {
              System.out.println("I love you....."+a);
          }
      }
  
      public static void main(String[] args) {
          /*
          ILove love = new Love();
          love.love(2);
  
          love = new Love1();
          love.love(3);
  
  
          // 4. 局部内部类
          class Love2 implements ILove {
              @Override
              public void love(int a) {
                  System.out.println("I love you....."+a);
              }
          }
  
          love = new Love2();
          love.love(4);
  
          // 5. 匿名内部类
          love = new ILove() {
              @Override
              public void love(int a) {
                  System.out.println("I love you...."+a);
              }
          };
          love.love(5);
           */
  
          // 6. lambda表达式
          ILove love = (int a) -> {
              System.out.println("I love you...." +a);
          };
          // 简化参数类型
          love = (a) -> {
              System.out.println("I love you...." +a);
          };
          // 简化括号
          love = a -> {
              System.out.println("I love you...." +a);
          };
          // 去掉花括号
          love = (a) -> System.out.println("I love you...." +a);
  
          // 总结：
          // 1. lambda 表达式只能在有一行代码的情况下才能简化掉花括号，如果有多行，就需要用代码块包裹
          // 2. 前提：接口是函数式接口
          // 3. 多个参数也可以去掉参数类型，必须加上括号
  
  
          love.love(6);
  
      }
  }
  
  // 1. 定义一个接口
  interface ILove {
      void love(int a);
  }
  
  // 2. 实现类
  /*
  class Love implements ILove {
      @Override
      public void love(int a) {
          System.out.println("I love you....."+a);
      }
  }
   */
  ```

  从内部类一步一步简化到 lambda 表达式的过程

### 3. 线程状态

![image-20210324122418277](C:\Users\周仙龙\AppData\Roaming\Typora\typora-user-images\image-20210324122418277.png)

* 创建状态
  * Thread t = new Thread()
  *  线程对象一旦创建就进入到创建状态
* 就绪状态
  * 当调用 start() 方法，线程立即进入就绪状态，但是不意味着立即调度执行

* 运行状态
  * 当 CPU 开始调度之后，线程进入运行状态。这时，线程才真正执行线程体的代码块	

* 阻塞状态
  * 当调用 sleep, wait 或同步锁定时，线程进入阻塞状态，就是代码不往下执行，阻塞事件解除后，重新进入就绪状态，等待 CPU 调度执行。
* 死亡状态
  * 线程中断或者结束，一旦进入死亡状态，就不能再次启动

**线程方法**

![image-20210324123000436](C:\Users\周仙龙\AppData\Roaming\Typora\typora-user-images\image-20210324123000436.png)

**停止线程**

* 不推荐使用 JDK 提供的 stop(), destroy() 方法。
* 推荐让线程自己停下来
* 建议使用一个标志位进行终止变量，当 flag = false，则终止线程运行

```java
package state;

// 测试 stop
// 1. 建议线程正常停止 ---> 利用次数，不建议死循环
// 2. 建议使用标志位 ---> 设置一个标志位
// 3. 不建议使用 stop(), destroy() 方法
public class TestStop implements Runnable{
    // 1. 设置一个标志位
    private boolean flag = true;

    @Override
    public void run() {
        int i = 0;
        while(flag) {
            System.out.println("线程正在运行"+(i++));
        }
    }

    // 2. 设置一个公开的方法停止线程，转换标志位
    public void stop() {
        this.flag = false;
    }

    // 主线程
    public static void main(String[] args) {
        TestStop testStop = new TestStop();
        new Thread(testStop).start();

        for (int i = 0; i < 1000; i++) {
            System.out.println("main " + i);
            if (i == 900) {
                // 调用 stop() 方法切换标志位，让线程停止
                testStop.stop();
                System.out.println("线程该停止了....");
            }
        }
    }
}
```

**线程休眠**

* sleep(时间) 指定当前线程阻塞的毫秒数
* sleep() 存在异常 InterruptedException
* sleep() 时间达到后进入就绪状态
* sleep() 可以模拟网络延时，倒计时等
* 每一个对象都有一个锁，sleep() 不会释放锁

```java
package state;

import java.text.SimpleDateFormat;
import java.util.Date;

// 模拟倒计时
public class TestSleep2 {
    public static void tenDown() throws InterruptedException {
        int number = 10;
        while (true) {
            Thread.sleep(1000);
            System.out.println(number--);
            if (number < 0) {
                break;
            }
        }
    }

    public static void main(String[] args) {
        //try {
        //    tenDown();
        //} catch (InterruptedException e) {
        //    e.printStackTrace();
        //}

        // 打印当前时间
        Date startTime = new Date(System.currentTimeMillis()); // 获取系统当前时间
        while (true) {
            try {
                Thread.sleep(1000);
                // 打印时间
                System.out.println(new SimpleDateFormat("HH:mm:ss").format(startTime));
                startTime = new Date(System.currentTimeMillis()); // 更新当前时间
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

```

**线程礼让**

* 礼让线程，让当前正在执行的线程暂停，但不阻塞

* 将线程从运行状态变为就绪状态

* **让 CPU 重新调度，礼让不一定成功！看 CPU 心情**

  ```java
  package state;
  
  // 线程礼让
  // 礼让不一定成功，看 CPU 心情
  public class TestYield {
      public static void main(String[] args) {
          MyYield myYield = new MyYield();
          new Thread(myYield, "a").start();
          new Thread(myYield, "b").start();
      }
  }
  
  // 线程类
  class MyYield implements Runnable {
      @Override
      public void run() {
          System.out.println(Thread.currentThread().getName()+"线程开始执行");
          // 线程礼让
          Thread.yield();
          System.out.println(Thread.currentThread().getName()+"线程停止执行");
      }
  }
  ```

**线程强制执行**

* Join 合并线程，待线程执行完成后，再执行其他线程，其他线程阻塞

* 可以想象成插队 

  ```java
  package state;
  
  // 线程强行执行，测试 join 方法
  public class TestJoin implements Runnable{
      @Override
      public void run() {
          for (int i = 0; i < 100; i++) {
              System.out.println("线程 vip 来了"+i);
          }
      }
  
      public static void main(String[] args) throws InterruptedException {
          // 启动线程
          TestJoin testJoin = new TestJoin();
          Thread thread = new Thread(testJoin);
          thread.start();
  
  
          // 主线程
          for (int i = 0; i < 1000; i++) {
              if (i == 200) {
                  thread.join(); // 插队
              }
              System.out.println("main "+i);
          }
      }
  }
  ```

**线程状态观测**

* Thread.state
* 线程状态。线程可以处于以下状态之一
  * NEW：尚未启动的线程处于此状态
  * RUNNABLE：再 Java 虚拟机中执行的线程处于此状态
  * BLOCKED：被阻塞等待监视器锁定的线程处于此状态
  * WAITTING：正在等待另一个线程执行特定动作的线程处于此状态
  * TIMED_WAITTING：正在等待另一个线程执行动作达到指定等待时间的线程处于此状态
  * TERMINATED：已退出的线程处于此状态 

* 一个线程可以在给定时间点处于一个状态，这些状态是不反映任何操作系统线程状态的虚拟机状态

```java
package state;

// 观察测试线程的状态
public class TestState {
    public static void main(String[] args) throws InterruptedException {
        // 创建线程
        Thread thread = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            System.out.println("/////");
        });

        // 观察状态
        Thread.State state = thread.getState();
        System.out.println(state); // NEW

        // 启动线程
        thread.start();
        state = thread.getState();
        System.out.println(state); // RUNNABLE

        // 监听线程
        while (state != Thread.State.TERMINATED) {  // 只要线程不终止，就一直输出状态
            Thread.sleep(1000);
            state = thread.getState(); // 更新线程状态
            System.out.println(state); // 输出状态
        }

    }
}
```



**线程优先级**

* Java 提供一个线程调度器来监控程序中启动后进入就绪状态的所有线程，线程调度器按照优先级决定应该调度哪个线程来执行
* 线程的优先级用数字表示，范围从 1 ~ 10
  * Thread.MIN_PRIORITY = 1;
  * Thread.MAX_PRIORITY = 10;
  * Thread.NORM_PRIORITY = 5;

* 使用以下方式改变或者获取优先级
  * getPriority.setPriority(int num)

* 优先级的设定建议在 start() 方法前
* 优先级低只是意味着获得调度的概率低，并不是优先级低就不会被调用了，这都是看 CPU 的调度

```java
package state;

// 测试线程的优先级
public class TestPriority {
    public static void main(String[] args) {
        // 主线程默认优先级
        System.out.println(Thread.currentThread().getName()+"-->"+Thread.currentThread().getPriority());

        // 创建实现类对象
        MyPriority myPriority = new MyPriority();

        // 创建线程
        Thread t1 = new Thread(myPriority);
        Thread t2 = new Thread(myPriority);
        Thread t3 = new Thread(myPriority);
        Thread t4 = new Thread(myPriority);
        Thread t5 = new Thread(myPriority);
        Thread t6 = new Thread(myPriority);

        // 先设置优先级，再启动
        t1.start();

        t2.setPriority(1);
        t2.start();

        t3.setPriority(4);
        t3.start();

        // 最大优先级
        t4.setPriority(Thread.MAX_PRIORITY);
        t4.start();

        //t5.setPriority(-1);
        //t5.start();
        //
        //t6.setPriority(11);
        //t6.start();
    }
}

// 创建线程实现类
class MyPriority implements Runnable{
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName()+"-->"+Thread.currentThread().getPriority());
    }
}
```

**守护(daemon)线程**

* 线程分为 **用户线程** 和 **守护线程**
* 虚拟机必须确保用户线程执行完毕
* 虚拟机不用等待守护线程执行完毕
* 如，后台记录操作日志，监控内存，垃圾回收等待

```java
package state;

// 测试守护线程
// 上帝守护你
public class TestDaemon {
    public static void main(String[] args) {
        God god = new God();
        You you = new You();


        // 让上帝变为守护线程
        Thread thread = new Thread();
        thread.setDaemon(true); // 默认是 false，表示是用户线程，正常的线程都是用户线程
        thread.start();

        // 你 用户线程启动
        new Thread(you).start();
    }
}

// 上帝
class God implements Runnable {
    @Override
    public void run() {
        while (true) {
            System.out.println("上帝保护你");
        }
    }
}

// You
class You implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 36500; i++) {
            System.out.println("你一生都活得很开心。。。");
        }
        System.out.println("=====goodbye! world.... ");
    }
}
```

### 4. 线程同步[重点]

**并发**

* 并发：**同一个对象**被**多个线程**同时操作

**并发问题**

```
package demo01;

// 多个线程同时操作同一个对象
// 买火车票的例子

// 发现问题：多个线程操作同一个资源的情况下，线程不安全，数据紊乱
public class TestThread4 implements Runnable{
    // 火车票数量
    private int ticketNums = 10;

    @Override
    public void run() {
        while(true) {
            // 循环终止条件
            if (ticketNums < 0) {
                break;
            }

            // 模拟延时
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            System.out.println(Thread.currentThread().getName()+"拿到了第"+(ticketNums--)+"张票");
        }
    }

    // 主线程
    public static void main(String[] args) {
        TestThread4 testThread4 = new TestThread4();

        new Thread(testThread4, "小明").start();
        new Thread(testThread4, "老师").start();
        new Thread(testThread4, "黄牛").start();
    }
}

```

![image-20210323185518851](C:\Users\周仙龙\AppData\Roaming\Typora\typora-user-images\image-20210323185518851.png)

**线程同步**

* 现实生活中，我们会遇到“同一个资源，多个人想使用”的问题，比如，食堂排队打饭，每个人都想吃饭，最天然的解决办法是，排队，一个一个来。
* 处理多线程问题时，多个线程访问同一个对象，并且某些线程还想修改这个对象，这时候我们就需要线程同步。线程同步其实就是一种等待机制，多个需要同时访问此对象的进程进入这个**对象的等待池**形成队列，等待前面线程使用完毕，下一个现场再使用。

**队列和锁**

* 由于同一进程的多个线程在共享同一块存储空间，在带来方便的同时，也带来了访问冲突问题，为了保证数据在方法中被访问时的正确性，在访问时加入 **锁机制 synchronized**，当一个线程获得对象的排他锁，独占资源，其他线程必须等待，使用后释放锁即可。存在以下问题：
  * 一个线程持有锁会导致其他所有需要此锁的线程挂起
  * 在多线程竞争下，加速，释放锁会导致比较多的上下文切换和调度延时，引起性能问题
  * 如果一个优先级高的线程等待一个优先级低的线程释放锁，会导致优先级倒置，引起性能问题

**线程不安全的实例**

```java
package syn;

// 不安全的买票
public class UnSafeBuyTicket {
    public static void main(String[] args) {
        BuyTicket station = new BuyTicket();
        new Thread(station, "苦逼的我").start();
        new Thread(station, "厉害的你").start();
        new Thread(station, "可恶额黄牛党").start();
    }
}

class BuyTicket implements Runnable {
    // 票
    private int tickets = 10;
    boolean flag = true; // 标志位

    @Override
    public void run() {
        // 买票
        while (flag) {
            buy();
        }
    }

    private void buy() {
        // 判断是否有票
        if (tickets <= 0) {
            flag = false;
            return;
        }

        // 模拟延时
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // 买票
        System.out.println(Thread.currentThread().getName()+"拿到"+(tickets--));
    }
}
```

```java
package syn;

import java.util.ArrayList;
import java.util.List;

// 线程不安全的集合
public class UnSateList {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        for (int i = 0; i < 10000; i++) {
            new Thread(() -> {                 
                list.add(Thread.currentThread().getName());
            }).start();
        }
        // 模拟延时
        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(list.size());
    }
}
```



**同步方法**

* 由于我们可以通过 private 关键字来保证数据对象只能被方法访问，所以我们只需要针对方法提出一套机制，这套机制就是 synchronized 关键字，它包括两种用法：
  * synchronized 方法和 synchronized 块
* synchronized 方法控制对 "对象" 的访问，每个对象对应一把锁，每个 synchronized 方法都必须获得调用该方法的对象的锁才能执行，否则线程会阻塞，方法一旦执行，就独占该锁，知道该方法返回才释放锁，后面被阻塞的线程才能获得这个锁，继续实行。
* 缺陷：若将一个大的方法声明为 synchronized 将会印象效率

**同步块**

* 同步块：synchronized (Obj ) {}
* Obj 称之为 **同步监视器**
  * Obj 可以为任何对象，但是推荐使用共享资源作为同步监视器
  * 同步方法中无需指定同步监视器，因为同步方法的同步监视器就是 this，就是这个对象本身，或者是 class

* 同步监视的执行过程
  1. 第一个线程访问，锁定同步监视器，执行其中代码
  2. 第二个线程访问，发现同步监视器被锁定，无法访问
  3. 第一个线程访问完毕，解锁同步监视器
  4. 第二个线程访问，发现同步监视器没有锁，然后锁定并访问

```java
package syn;

// 不安全的买票
public class UnSafeBuyTicket {
    public static void main(String[] args) {
        BuyTicket station = new BuyTicket();
        new Thread(station, "苦逼的我").start();
        new Thread(station, "厉害的你").start();
        new Thread(station, "可恶额黄牛党").start();
    }
}

class BuyTicket implements Runnable {
    // 票
    private int tickets = 10;
    boolean flag = true; // 标志位

    @Override
    public void run() {
        // 买票
        while (flag) {
            buy();
        }
    }

    // synchronized 同步方法
    // 锁的是 this
    private synchronized void buy() {
        // 判断是否有票
        if (tickets <= 0) {
            flag = false;
            return;
        }

        // 模拟延时
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // 买票
        System.out.println(Thread.currentThread().getName()+"拿到"+(tickets--));
    }
}
```

```java
package syn;

import java.util.ArrayList;
import java.util.List;

// 线程不安全的集合
public class UnSateList {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        for (int i = 0; i < 10000; i++) {
            new Thread(() -> {
                synchronized (list) {
                    list.add(Thread.currentThread().getName());
                }
            }).start();
        }
        // 模拟延时
        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.printl n(list.size());
    }
}
```

**JUC中线程安全的list**

```java
package syn;

import java.util.concurrent.CopyOnWriteArrayList;

// 测试JUC安全类型的集合
public class TestJUC {
    public static void main(String[] args) {
        CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList<String>();
        for (int i = 0; i < 10000; i++) {
            new Thread(() -> {
                list.add(Thread.currentThread().getName());
            }).start();
        }

        // 模拟延时
        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        // 打印大小
        System.out.println(list.size());
    }
}
```

**死锁**

* 多个线程各自占有一些共有资源，并且互相等待其他线程占有的资源才能运行，而导致两个或者多个线程都在等待对方释放资源，都停止执行的情形。某一个同步块同时拥有“**两个以上对象的锁**”时，就可能会发生“死锁”的问题。

```java
package thread;

// 死锁：多个线程互相抱着对方需要的资源，然后形成僵持
public class DeadLock {
    public static void main(String[] args) {
        MakeUp q1 = new MakeUp(0, "灰姑娘");
        MakeUp q2 = new MakeUp(1, "白雪公主");
        q1.start();
        q2.start();
    }
}

// 口红
class Lipstick {

}

// 镜子
class Mirror {

}

// 化妆
class MakeUp extends Thread {

    // 创建镜子和口红对象
    // 需要的资源只有一份
    static Lipstick lipstick = new Lipstick();
    static Mirror mirror = new Mirror();
    // 选择
    int choice;
    String girlName; // 使用化妆品的人

    // 构造器
    MakeUp (int choice, String girlName) {
        this.choice = choice;
        this.girlName = girlName;
    }


    @Override
    public void run() {
        // 化妆
        try {
            makeUp();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    // 化妆，互相持有对方的锁，就是需要拿到对方的资源
    private void makeUp() throws InterruptedException {
        if (choice == 0) {
            // 获得口红的锁
            synchronized (lipstick) {
                System.out.println(this.getName()+"获得口红的锁");
                Thread.sleep(1000);  // 休息一秒
                synchronized (mirror) {
                    System.out.println(this.getName()+"获得镜子的锁");
                }
            }
        } else {
            // 获得镜子的锁
            synchronized (mirror) {
                System.out.println(this.getName()+"获得镜子的锁");
                Thread.sleep(2000); // 休息两秒
                synchronized (lipstick) {
                    System.out.println(this.getName()+"获得口红的锁");
                }
            }
        }
    }
}

```

**死锁避免方法**

* 产生死锁的四个条件
  1. 互斥条件：一个资源每次只能被一个进程使用
  2. 请求和保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放
  3. 不剥夺条件：进程已获得的资源，在未使用完之前，不能强行剥夺
  4. 循环等待条件：若干进程之间形成一种头尾相接的循环等待资源关系
* 上面列出了死锁的四个必要条件，我们只需要想办法破坏其中的一个或多个条件就可以避免死锁的发生

**Lock（锁）**

* 从 JDK5.0 开始，Java 提供了更强大的线程同步机制——通过显式定义同步锁对象来实现同步。同步锁使用 Lock 对象充当。
* java.util.concurrent.locks.Lock 接口时控制多个线程对共享资源进行访问的工具。锁提供了对共享资源的独占访问，每次只能有一个线程对 Lock 对象加锁，线程开始访问共享资源之前应获得 Lock 对象
* 可重入锁 ReentrantLock 类实现了 Lock 接口，它拥有与 synchronized 相同的并发性和内存语义，在实现线程安全的控制中，比较常用的是 ReentrantLock，可以显式加锁，释放锁

```java
package gaoji;

import java.util.concurrent.locks.ReentrantLock;

// 测试Lock锁
public class TestLock {
    public static void main(String[] args) {
        TestLock2 testLock2 = new TestLock2();
        new Thread(testLock2).start();
        new Thread(testLock2).start();
        new Thread(testLock2).start();
    }
}

class TestLock2 implements Runnable {

    // 票数
    int tickets = 10;

    // 定义 Lock 锁
    private final ReentrantLock lock = new ReentrantLock();

    @Override
    public void run() {
        while(true) {
            // 加锁
            lock.lock();
            try {	
                // 退出循环条件
                if (tickets < 0) {
                    break;
                }
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(tickets--);
            } finally {
                // 解锁
                lock.unlock();
            }
        }
    }
}

```

**synchronized 与 lock 的对比**

* Lock 是显式锁（手动开启和关闭锁），synchronized 是隐式锁，出了作用域自动释放
* Lock 只有代码块锁，synchronized 有代码块锁和方法锁
* 使用 Lock 锁，JVM 将花费较少的时间来调度线程，性能更好。并且具有更好的扩展性（提供更多的子类）
* 优先使用顺序：
  * Lock > 同步代码块（已经进入方法体，分配了相应资源） > 同步方法（在方法体之外）

### 5. 线程通信问题

**应用场景：生产者和消费者问题**

* 假设仓库中只能存放一件产品，生产者将生产出来的产品放入仓库，消费者将仓库中产品取走消费
* 如果仓库中没有产品，则生产者将产品放入仓库，否则停止生产并等待，直到仓库中的产品被消费者取走为止
* 如果仓库中方有产品，则消费者可以将产品取走消费，否则停止消费并等待，直到仓库中再次放入产品为止

**分析：这是一个线程同步问题，生产者和消费者共享用同一个资源，并且生产者和消费者之间相互依赖，互为条件**

* 对于生产者，没有生产产品之前，要通知消费者等待，而生产了产品之后，又需要马上通知消费者取走消费
* 对于消费者，在消费之后，要通知生产者已经结束消费，需要生产新的产品以供消费
* 在生产者消费者问题中，仅有 synchronized 是不够的
  * synchronized 可阻止并发更新同一个共享资源，实现了同步
  * synchronized 不能用来实现不同线程之间的消息传递（通信）

**Java 提供了几个方法解决线程之间的通信问题**

![image-20210324191609341](C:\Users\周仙龙\AppData\Roaming\Typora\typora-user-images\image-20210324191609341.png)

​		**注意：均是 Object 类的方法，都只能在同步方法或者同步代码块中使用，否则会抛出异常IIIegalMonitorStateException**

**解决方式1：管程法**

* 并发协作模型“生产者/消费者模式” --> 管程法
* 生产者：负责生产数据的模块（可能是方法，对象，线程，进程）
* 消费者：负责处理数据的模块（可能是方法，对象，线程，进程）
* 缓冲区：消费者不能直接使用生产者的数据，他们之间有个“缓冲区”

**生产者将生产好的数据放入缓冲区，消费者从缓冲区拿出数据**

```java
package pc;

// 测试：生产者/消费者模型 -> 利用缓冲区解决：管程法

// 生产者，消费者，产品，缓冲区
public class TestPC {

    public static void main(String[] args) {
        SynContainer container = new SynContainer();

        new Productor(container).start();
        new Consumer(container).start();
    }

}

// 生产者
class Productor extends Thread {
    SynContainer container; // 创建属性

    // 构造器
    public Productor(SynContainer container) {
        this.container = container;
    }

    // 生产
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            container.push(new Chicken(i));
            System.out.println("生产了"+i+"只鸡");
        }
    }
}

// 消费者
class Consumer extends Thread {
    SynContainer container; // 创建属性

    // 构造器
    public Consumer(SynContainer container) {
        this.container = container;
    }


    // 消费
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println("消费了-->"+container.pop().id +"只鸡");
        }
    }
}

// 产品
class Chicken {
    int id; // 产品编号
    // 构造器
    public Chicken(int id) {
        this.id = id;
    }
}

// 缓冲区
class SynContainer {

    // 容器大小
    Chicken[] chickens = new Chicken[10];
    // 容器中鸡的数量
    int count;

    // 生产者放入产品
    public synchronized void push(Chicken chicken) {
        // 如果容器满了，就需要等待消费者消费
        if (count == chickens.length) {
            // 生产者等待
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        // 如果没有满，生产者生产产品
        chickens[count] = chicken;
        count++;

        // 通知消费者消费
        this.notifyAll();
    }

    // 消费者消费产品
    public synchronized Chicken pop() {
        // 判断能否消费
        if (count == 0) {
            // 消费者等待
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        // 如果可以消费
        count--;
        Chicken chicken = chickens[count];

        // 吃完了，通知生产者生产
        this.notifyAll();

        return chicken;
    }
}
```

**解决方式2：信号灯法**

* 设置一个标识符，来监听是否等待

```java
package pc;

// 测试生产者/消费者问题2：信号灯法，标志位解决
public class TestPC2 {
    public static void main(String[] args) {
        Tv tv = new Tv();
        new Player(tv).start();
        new Watcher(tv).start();
    }
}

// 生产者 -- 演员
class Player extends Thread {
    // 属性
    Tv tv;

    // 构造器
    public Player(Tv tv) {
        this.tv = tv;
    }

    @Override
    public void run() {
        for (int i = 0; i < 20; i++) {
            if ( (i & 1) == 0) {
                this.tv.play("快乐大本营表演");
            } else {
                this.tv.play("抖音：记录美好生活");
            }
        }
    }
}

// 消费者 -- 观众
class Watcher extends Thread {
    // 属性
    Tv tv;

    // 构造器
    public Watcher(Tv tv) {
        this.tv = tv;
    }

    @Override
    public void run() {
        for (int i = 0; i < 20; i++) {
            tv.watch();
        }
    }
}

// 产品 -- 节目
class Tv {
    // 演员表演，观众等待
    // 观众观看，演员等待
    String name; // 表演的节目
    boolean flag = true; // 标志位

    // 表演
    public synchronized void play(String name) {
        // flag 为 true，表明存在了节目，等待观看
        if (!flag) {
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        this.name = name;
        System.out.println("演员表演了：" + name);
        // 通知观众观看
        this.notifyAll(); // 通知唤醒
        this.flag = !this.flag;
    }

    // 观看
    public synchronized void watch() {
        // flag 为 false，表明不存在了节目，等待表演
        if (flag) {
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("观看了：" + this.name);
        // 通知演员表演
        this.notifyAll();
        this.flag = !this.flag;
    }
}
```

### 6. 高级主题

**线程池**

* 背景：经常创建和销毁、使用量特别大的资源，比如并发情况下的线程，对性能影响很大
* 思路：提前创建好多个线程，放入线程池中，使用时直接获取，使用完放回池中。可以避免频繁创建线程、实现重复利用。
* 好处：
  * 提高响应速度（减少了创建新线程的时间）
  * 降低资源消耗（重复利用线程池中线程，不需要每次都创建）
  * 便于线程管理

* 参数
  * corePoolSize：核心池的大小
  * maximumPoolSize：最大线程数
  * keepAliveTime：线程没有任务时最多保持多长时间后会终止

**使用线程池**

* JDK 5.0 起提供了线程池相关API：ExecutorService 和 Executors
* ExecutorService：真正的线程池接口。常见子类 ThreadPoolExecutor
  * void execute(Runnable command)：执行任务/命令，没有返回值，一般用来执行 Runnable
  * <T> Future <T> submit(Callable<T> task)：执行任务，有返回值，一般用来执行 Callable
  * void shutDown()：关闭连接池
* Executors：工具类、线程池的工厂类，用于创建并返回不同类型的线程池 

**第一种：使用 Runnable 创建线程池**

```java
package gaoji;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

// 使用 Runnable 测试线程池
public class ThreadPool {
    public static void main(String[] args) {
        // 1. 创建服务，创建线程池
        ExecutorService service = Executors.newFixedThreadPool(10);

        // 执行
        service.execute(new MyThread());
        service.execute(new MyThread());
        service.execute(new MyThread());
        service.execute(new MyThread());


        // 关闭连接
        service.shutdown();
    }
}

// 自定义线程
class MyThread implements Runnable {
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }
}
```

**第二种：使用 Callable 创建线程池**

```java
package gaoji;

import java.util.concurrent.*;

// 使用 callable 创建线程池
public class ThreadPool2 {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        // 1. 创建服务，创建线程池
        ExecutorService service = Executors.newFixedThreadPool(3);

        // 提交执行
        Future<Boolean> f1 = service.submit(new MyCallable());
        Future<Boolean> f2 = service.submit(new MyCallable());
        Future<Boolean> f3 = service.submit(new MyCallable());

        // 获取结果
        boolean r1 = f1.get();
        boolean r2 = f2.get();
        boolean r3= f3.get();

        // 打印结果
        System.out.println(r1);
        System.out.println(r2);
        System.out.println(r3);

        // 关闭线程池
        service.shutdown();
    }
}

// callable 接口实现类
class MyCallable implements Callable<Boolean> {
    @Override
    public Boolean call() throws Exception {
        System.out.println(Thread.currentThread().getName());
        return true;
    }
}
```

