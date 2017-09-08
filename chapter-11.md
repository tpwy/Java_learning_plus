# 第十一章 多线程

## 11.1 线程的概念

+ 多线程编程：将一个程序任务分成几个可以同时并发执行的子任务
+ 程序(Program)：程序是含有指令和数据的文件，被存储在磁盘或其他的数据存储设备中。即程序是静态的代码
+ 进程(Process): 进程是程序的一次执行过程，是系统运行程序的基本单位，因此进程是动态的
+ 系统运行一个程序即是一个进程从创建、运行到消亡的过程。也就是说，一个进程就是一个执行中的程序
+ 每个进程之间是独立的，除非利用某些通讯管道来进行通信，或是通过操作系统产生交互作用，否则基本各进程之间不知道彼此的存在
+ 多任务(Multi task)：多任务指一个系统中可以运行多个进程，即有多个独立运行的任务，每一个任务对应一个进程。
+ 每个进程都有一段专用的内存区域，即使多次启动同一段程序也会产生不同的进程
+ 所谓同时运行的进程，其实是值操作系统将系统资源分配给各个进程，每个进程在CPU交替运行
+ 每个进程占有不同的内存空间，内存消耗大。这使得系统在不同程序之间切换时开销很大，进程之间的通信速度慢
+ 线程是比进程更小的执行单位。与进程不同的是，同类的多个线程共享同一块内存空间和一组系统资源。因此也被称为轻量级进程(light-weight process)
+ 对于每个线程来说，它都有自身的产生、运行和消亡的过程，也是一个动态的概念
+ 操作系统并没有将多个线程看作多个独立的应用去实现线程的调度和管理以及资源分配

### 11.1.1 线程的状态与生命周期

+ 每个Java程序都有一个默认的主线程，对于应用程序来说其主线程是main()方法执行的线程
+ 要想实现多线程，必须在主线程中创建新的线程对象
+ Java语言使用Thread类及其子类的对象来表示线程，新线程在它的一个完整的生命周期内通常需要经历五个状态。通过线程的控制和调度可使线程在这几种状态间转化

1. 新建状态(newborn)
    + 当一个Thread类或其子类的对象被声明并创建，但还未被执行的这段时间，处于一种特殊的新建状态中
    + 此时线程对象已经被分配了内存空间和其他资源，并已被初始化，但该线程未被调度。当该线程被调度，就变成就绪状态
2. 就绪状态(runnable)
    + 就绪状态也称为可运行状态。处于新建状态的线程被启动后，将进入线程队列排队等待CPU时间片，此时它已具备了运行的条件，即处于就绪状态
    + 一旦轮到它享用CPU资源时，就可以脱离创建它的主线程开始自己的声明周期
    + 原来处于阻塞状态的线程被接触阻塞后也将进入就绪状态
3. 运行状态(running)
    + 当就绪状态的线程被调度并获得CPU资源时，便进入运行状态。该状态表示线程正在运行，已经拥有了对CPU的控制权
    + 每一个Thread类及其子类的对象都有一个重要的run()方法，该方法定义了这一类线程的操作和功能
    + 当线程对象被调度执行是，它将自动调用本对象的run()方法开始执行知道执行完毕
    + 除非线程主动让出CPU的控制权或者CPU的控制权被优先级更高的线程抢占
    + 处于运行状态的线程在下列情况下将让出CPU的控制权：
        + 线程运行完毕
        + 比当前线程优先级更高的线程处于就绪状态
        + 线程主动睡眠一段时间
        + 线程在等待某一资源
4. 阻塞状态(blocked)
    + 正在执行的线程如果在某些特殊情况下，将出让CPU并暂时中止自己的执行，线程处于这种不可运行的状态这称为阻塞状态
    + 处于阻塞情况下即使CPU空闲也不能执行线程
    + 下面几种情况下可以使得一个线程进入阻塞状态
        1. 调用sleep()和yield()方法
        2. 为了等待一个条件变量，线程调用wait()方法
        3. 该线程与另一个线程join()在一起
    + 线程被阻塞时不能进入排队队列，只有引起阻塞的原因消失时才可以转入就绪状态，并从原来的暂停处继续运行
    + 处于睡眠状态的线程通常需要由某些事件才能唤醒，至于由什么时间唤醒该线程，取决于其阻塞的原因
5. 消亡状态(dead)
    + 处于消亡状态的线程不具有继续运行的能力
    + 导致线程消亡的原因
        1. 正常运行的线程完成了它的全部工作，即执行完了run()方法然后退出
        2. 当程序因故停止运行时，进程中的所有线程将被强行终止
    + 当线程处于消亡状态，并且没有该线程对象的引用时，垃圾回收其会从内存中删除该线程对象

### 11.1.2 线程的调度与优先级

#### 调度

+ 调度就是指各个线程之间分配CPU资源，多个线程的并发执行实际上是通过一个调度来进行的
+ 线程调度有两种模型: 分时模型和抢占模型

分时模型：

+ CPU资源按照时间片分配，获得CPU资源的线程只能在指定时间片内执行，一旦时间片使用完毕，就必须把CPU让给另一个处于就绪状态线程
+ 在分时模型中，线程本身不会让出CPU

抢占模型：

+ 当前活动的线程一旦获得执行权，将一直执行下去直到执行完成或由于某种原因主动放弃执行权
+ Java语言支持的就是抢占式调度模型
+ 为了使低优先级的线程有机会运行，高优先级的线程应用不时地主动进入"睡眠'状态，而暂时让出CPU

#### 优先级

+ 在多线程系统中每个线程都被赋予一个执行优先级，这个优先级决定了线程被CPU执行的优先顺序
+ 对于优先级完全相同的线程，按照"先来先用"的原则调度
+ Java语言中程序的优先级从低到高以整数1~10表示，共10级。
+ Thread类有3个关于线程优先即的静态变量
    + MIN_PRIORITY——最小优先级，通常为1
    + MAX_PRIORITY——最高优先级，通常为10
    + NORM_PRIORITY——普通优先级，默认值为5
+ 对应新建的线程，系统遵循一下原则为其制定优先级:
    1. 新建线程将继承创建它的父线程的优先级。父线程指执行创建新线程对象语句所在的线程
    2. 一般情况下主线程具有普通优先级
+ 可以调用线程对象的setPriority()方法改变线程的优先级

## 11.2 Java 的Thread线程类和Runnable接口

Java语言中实现多线程的两个方法
1. 继承java.lang中的Thread类
2. 用户在定义自己的类中实现Runnable接口

### 11.2.1 利用Thread类的子类来创建线程

Thread类的构造方法

| 构造方法 | 功能说明 |
|-|-|
| public Thread() | 创建一个线程对象，此线程对象的名称是"Thread-n"的形式，其中n是一个整数。使用这个构造方法，必须创建Thread类的一个子类并覆盖其run()方法 |
| public Thread(String name) | 创建一个线程对象，参数name指定了线程的名称 |
| public Thread(Runnable target) | 参数target的run()方法将被线程对象调用来作为执行代码 |
| public Thread(Runnable target, String name) |

Thread类的常用方法

| 方法 | 功能说明 |
|-|-|
| public static Thread currentThread() | 返回当前正在运行的线程对象 |
| public final String getName() | 返回线程名称 |
| public void start() | 使得线程由新建状态变为就绪状态，如果已经是就绪状态，则产生IllegalStateException异常 |
| public void run() | 线程应执行的任务 |
| public final boolean isAlive() | 判断当前线程是否正在运行 |
| public void interrupt() | 中断当前线程 |
| public static boolean isInterrupted() | 判断当前线程是否被中断 |
| public final void join() | 暂停当前线程的执行，等待调用该方法的线程结束后再继续执行本线程 |
| public final int getPriority() | 返回线程的优先级 | 
| public final void setPriority(int newPriority) | 设置线程优先级。如果当前优先级不能修改这个线程，则产生SecurityException异常；如果参数不在所要求的优先级范围内，则产生IllegalArgumentException异常 |
| public static void sleep(long mills) | 为当前线程指定睡眠时间，参数mills单位是毫秒。若该线程已经被别的线程中断，则产生InterruptException异常 |
| public static void yield() | 暂停当前线程的执行，但线程仍处于就绪状态而非阻塞状态。该方法只给同优先级线程运行的机会 |

### 11.2.2 用Runnable接口来创建线程

+ 如果类本身已经继承了某个父类，由于Java不允许多重继承，无法再继承Thread类。这时就需要创建一个类来实现Runnable接口。
+ Runnable接口只有一个方法run()，并没有任何对线程的支持，因此还必须创建Thread类的实例，这一点通过Thread类的构造方法实现
+ Runnable接口间接解决了多重继承问题，并且更适合于多个线程处理同一资源

两种创建线程对象的方式：
1. 直接继承Thread类
    + 优点：编写简单，直接操作线程
    + 缺点：不能再继承其他类
2. 使用Runnable接口
    + 可以将Thread类与所要处理的任务的类分开，形成清晰的模型
    + 可以从其他类继承，实现多重继承的功能

+ 若直接使用Thread类，在类中this指当前线程
+ 若使用Runnable接口，必须使用Thread.currentThread()方法才能获得当前线程

### 11.2.3 线程之间的数据共享

+ 当多个线程的执行代码来自同一个类的run()方法是，则称它们共享相同的代码
+ 当它们访问相同的对象时，则称它们共享相同的数据

```
public class App11_5 {
    public static void main(String[] args)
    {
        ThreadSale t = new ThreadSale();
        Thread t1 = new Thread(t, "num1");
        Thread t2 = new Thread(t, "num2");
        Thread t3 = new Thread(t, "num3");
        t1.start();
        t2.start();
        t3.start();
    }
}

class ThreadSale implements Runnable
{
    private int tickets = 10;
    public void run()
    {
        while (true)
        {
            if (tickets > 0)
                System.out.println(Thread.currentThread().getName() + " 售票机售票第 " + tickets-- +" 号");
            else
                System.exit(0);
        }
    }
}
```

## 11.3 多线程的同步控制

+ 当线程运行时不需要外部的数据或方法，因此不必关心其他线程的状态或行为。我们称这样的线程为独立的、不同步的或是异步执行的
+ 当存在多个线程之间共享数据时，若线程仍以异步方式访问共享数据，有时是不安全的或不符合逻辑的
+ 因此，当一个线程对共享数据进行操作时，应使之成为一个"原子操作"。即在没有完成相关操作之前，不允许其他线程打断它，否则将破坏数据的完整性，得到错误的处理结果，这就是线程的同步
+ 线程的互斥和同步的区别是，互斥指两个或多个线程不能同时运行，而同步则是两个或多个线程的运行有先后次序的约束
+ 共享是指线程之间对内存数据的共享，因为线程共同拥有对内存空间中数据的处理权力，这样会导致因为多个线程同时处理数据而使数据出现不一致。所有提出同步来解决问题
+ 同步与并发多线程并不矛盾，这里同步指的是处理数据的线程不能处理其他线程当前还没有处理完的数据，但是可以处理其他数据。
+ 在并发程序设计中，对多线程共享的资源或数据称为临界资源或同步资源，而把每个线程中访问临界资源的那一段代码称为临界代码或临界区。
+ 临界区必须互斥地使用，为了使临界代码对临界资源的访问成为一个不可被中断的原子操作，Java利用对象"互斥锁"机制来实现线程之间的互斥操作
+ 每个对象都有且只有一个"互斥锁"，为了保证互斥，Java语言中使用`synchronized`关键字来识别同步资源，这里的资源可以是对象、方法或一段代码

```
// 同步语句
Synchronized (对象) // 对象是需要锁定的临界资源
{
    临界代码段
}

// 同步方法
public 返回类型 方法名()
{
    synchronized(this)
    {
        方法体
    }
}

// 当被synchronized限定的代码块执行完，就自动释放互斥锁
```

对`synchronized`的进一步说明
1. `synchronized`锁定的通常是临界代码
2. `synchronized`代码块中的代码数量越少越好，包含范围越小越好，否则就失去了多线程并发执行的很多优势
3. 若两个或多个线程锁定的不是同一个对象，则它们的`synchronized`代码块可以互相交替穿插进行
4. 所有的非`synchronized`代码块或方法仍可以自由调用
5. 获得互斥锁的线程可以调用多个对象的所有`synchronized`代码块和方法
6. 一个对象的互斥锁只能被一个线程所拥有
7. 只有当一个线程调用完所调用对象的所有`synchronized`代码块和方法后，才会释放互斥锁
8. 临界代码中的共享变量应定义为`private`型
9. 由于8，只能用临界代码中的方法访问共享变量，因此锁定的对象通常是this
10. 一定要保证所有对临界代码中共享变量的访问和操作都在`synchronized`代码块中进行
11. 通常共享变量都是私有静态变量
12. 对于一个`static`型方法，要么整个方法都是`synchronized`，要么整个方法都不是`synchronized`
13. 如果`synchronized`用于类声明中，则类中的所有方法都是`synchronized`

## 11.4 线程之间的通信

多线程的执行往往需要相互配合，为了更有效地协调不同线程的工作，需要在线程间建立沟通渠道，通过线程间的通信来解决同步问题，而不仅仅是依靠互斥

Object类中用于线程通信的常用方法

| 方法 | 功能说明 |
|-|-|
| public final void wait() | 如果一个正在执行同步代码的线程A执行了wait()调用(在对象x上)，该线程暂停执行而进入对象x的等待队列中，并释放对象x的互斥锁。 |
| public void notify() | 唤醒正在等待该对象互斥锁的第一个线程 |
| public void notifyAll() | 唤醒正在等待中该对象互斥锁的所有线程，具有最高优先级的线程首先被唤醒执行 |

注意：对于一个线程，若基于对象x调用wait()或notify()方法，该线程必须已经获得对象x的互斥锁。即这几个方案只能在同步代码块中调用

sleep()和wait()的区别在于：wait()方法在放弃CPU资源的同时交出资源的控制权，而sleep()方法则无法做到这点

```
public class App11_8 {
    public static void main(String[] args)
    {
        Tickets one = new Tickets(10);
        Producer pro = new Producer(one);
        Consumer cus =  new Consumer(one);
        pro.start();
        cus.start();
    }
}

class Tickets
{
    private int size;
    int number = 0;
    boolean available = false;
    public int getSize()
    {
        return this.size;
    }
    public Tickets(int size)
    {
        this.size = size;
    }
    public synchronized void put()
    {
        if (available)
            try {wait();}
            catch (Exception e) {}
        System.out.println("存入第【" + (++number) + "】号票");
        available = true;
        notify();
    }
    public synchronized void sell()
    {
        if (!available)
            try {wait();}
            catch (Exception e) {}
        System.out.println("售出第【" + number + "】号票");
        available = false;
        notify();
        if (number == size)
            number = size + 1;
    }
}

class Producer extends Thread
{
    Tickets t = null;
    public Producer(Tickets t)
    {
        this.t = t;
    }
    public void run()
    {
        while (t.number < t.getSize() ) {
            t.put();
        }
    }
}

class Consumer extends Thread
{
    Tickets t = null;
    public Consumer(Tickets t)
    {
        this.t = t;
    }
    public void run()
    {
        while (t.number <= t.getSize())
            t.sell();
    }
}
```