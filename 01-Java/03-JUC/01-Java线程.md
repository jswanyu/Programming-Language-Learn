参考资料：

黑马 JUC视频

《实战：Java高并发程序设计》





#  一、进程与线程

> 进程: 资源分配的最小单位
>
> - 进程是线程的容器, 一个进程中包含多个线程, 真正执行任务的是线程
>
> 线程: 资源调度的最小单位



## 进程

- 程序由指令和数据组成，但这些指令要运行，数据要读写，就必须将指令加载至 CPU，数据加载至内存。在指令运行过程中还需要用到磁盘、网络等设备。**进程就是用来加载指令、管理内存、管理 IO 的**。
- **当一个程序被运行，从磁盘加载这个程序的代码至内存，这时就开启了一个进程**。
- **进程就可以视为程序的一个实例**。大部分程序可以同时运行多个实例进程（例如记事本、画图、浏览器 等），也有的程序只能启动一个实例进程（例如网易云音乐、360 安全卫士等）

## 线程

- 一个进程之内可以分为**一到多个线程**。
- 一个线程就是一个指令流，将指令流中的一条条指令以一定的顺序交给 CPU 执行 。
- Java 中，线程作为最小调度单位，进程作为资源分配的最小单位。 在 windows 中进程是不活动的，只是作为线程的容器

## 二者对比

- **进程基本上相互独立**的，而**线程存在于进程内，是进程的一个子集**
- **进程拥有共享的资源，如内存空间等，供其内部的线程共享**
- 进程间通信较为复杂 
    - 同一台计算机的进程通信称为 IPC（Inter-process communication）
    - 不同计算机之间的进程通信，需要通过网络，并遵守共同的协议，例如 HTTP
- 线程通信相对简单，因为它们共享进程内的内存，一个例子是多个线程可以访问同一个共享变量 
- 线程更轻量，线程上下文切换成本一般上要比进程上下文切换低



<font color=red>进程和线程的切换</font>



# 二、并发与并行

## 并发(concurrent)

**是一个CPU核在不同的时间轮流执行不同线程中的指令**。

虽然叫并发，但实际还是串行执行, CPU的时间片切换非常快，给人一种同时运行的感觉。微观串行, 宏观并行
（操作系统中有一个组件叫做任务调度器，将 cpu 的时间片（windows下时间片最小约为 15 毫秒）分给不同的程序使用，只是由于cpu 在线程间（时间片很短）的切换非常快，给人的感觉是同时运行的 。）



## 并行

**多核CPU的每个核在同一时刻处理不同的线程。**

并行中也可能存在并发，比如说2核cpu, 同时执行4个线程. 理论上同时可以有2个线程是并行执行的. 此时还是存在并发， 因为2个cpu也会同时切换不同的线程执行任务罢了



## 二者对比

引用 Rob Pike 的一段描述：

- 并发（concurrent）是同一时间**应对**（dealing with）多件事情的能力
- 并行（parallel）是同一时间**动手做**（doing）多件事情的能力



# 三、同步与异步

以调用方角度来讲，如果

- 需要等待结果返回，才能继续运行就是**同步**
- 不需要等待结果返回，就能继续运行就是**异步**

## 设计

多线程可以让方法执行变为异步的（即不要巴巴干等着）比如说读取磁盘文件时，假设读取操作花费了 5 秒钟，如 果没有线程调度机制，这 5 秒 cpu 什么都做不了，其它代码都得暂停…

## 举例

比如在项目中，视频文件需要转换格式等操作比较费时，这时开一个新线程处理视频转换，避免阻塞主线程

tomcat 的异步 servlet 也是类似的目的，让用户线程处理耗时较长的操作，避免阻塞

tomcat 的工作线程 ui 程序中，开线程进行其他操作，避免阻塞 ui 线程

## 结论

* 单核 cpu 下，多线程不能实际提高程序运行效率，只是为了能够在不同的任务之间切换，不同线程轮流使用 cpu ，不至于一个线程总占用 cpu，别的线程没法干活

* 多核 cpu 可以并行跑多个线程，但能否提高程序运行效率还是要分情况的

    - 有些任务，经过精心设计，将任务拆分，并行执行，当然可以提高程序的运行效率。但不是所有计算任务都能拆分（参考后文的<font color=red>阿姆达尔定律</font>）

    - 也不是所有任务都需要拆分，任务的目的如果不同，谈拆分和效率没啥意义

* IO 操作不占用 cpu，只是我们一般拷贝文件使用的是【阻塞 IO】，这时相当于线程虽然不用 cpu，但需要一 直等待 IO 结束，没能充分利用线程。所以才有后面的【非阻塞 IO】和【异步 IO】优化





# 四、线程的创建

Java中创建一个线程（非主线程）有如下几种方法

## 方法一：通过继承Thread创建线程

1. 创建线程对象
2. 启动线程

```java
@Slf4j(topic = "c.CreateThread")
public class CreateThread {
    public static void main(String[] args) {
        // 创建线程对象
        Thread myThread = new Thread("myThread"){
            // 这里是匿名内部类的写法，本质上是继承Thread
            @Override
            public void run() {
                log.debug("myThread is running");
            }
        };
        // 主线程
        log.debug("main thread is running");
        // 启动线程
        myThread.start();
    }
}
```

> 17:08:16.562 [main] DEBUG c.CreateThread - main thread is running
>
> 17:08:16.562 [myThread] DEBUG c.CreateThread - myThread is running

使用继承方式的好处是:

* 在run()方法内获取当前线程直接使用this就可以了，无须使用Thread.currentThread（）方法；

不好的地方是:

* Java不支持多继承，如果继承了Thread类，那么就不能再继承其他类。
* 任务与代码没有分离，当多个线程执行一样的任务时需要多份任务代码



## 方法二：使用Runnable配合Thread（推荐）

把【线程】和【任务】（要执行的代码）分开

1. 创建任务对象
2. 创建线程，传入任务对象
3. 启动线程

```java
@Slf4j(topic = "c.CreateThread2")
public class CreateThread2 {
    public static void main(String[] args) {
        Runnable task = new Runnable(){
            @Override
            public void run(){
                log.debug("myThread2 is running");
            }
        };
        Thread myThread2 = new Thread(task, "myThread2");
        myThread2.start(); //18:18:09.634 [myThread2] DEBUG c.CreateThread2 - myThread2 is running
    }
}
```

JDK8之后，对于匿名内部类，可以使用 lambda 精简代码

```java
public static void main(String[] args) {
    //任务对象
    Runnable task = () -> log.debug("myThread2 is running"); //任务有多行语句也可以加上大括号
    //线程对象
    Thread myThread2 = new Thread(task, "myThread2");
    //启动线程
    myThread2.start();
}
```

也可以进一步简写，将任务对象也用lambda 形式，写到线程的第一个参数里

```java
public static void main(String[] args) {
    Thread myThread2 = new Thread(() -> log.debug("myThread2 is running"), "myThread2");
    myThread2.start();
}
```

如果后续用不到myThread2，直接启动

```java
public static void main(String[] args) {
    new Thread(() -> log.debug("myThread2 is running"), "myThread2").start();
}
```



#### 原理之 Thread 与 Runnable 的关系

分析 Thread 的源码，理清它与 Runnable 的关系：

Thread的构造方法第一个参数就是Runnable 类型的参数，传到最后就是调用Runnable 的run方法



**小结**

- 方法1 是把线程和任务合并在了一起
- 方法2 是把线程和任务分开了
- 用 Runnable 更容易与线程池等高级 API 配合 用 Runnable 让任务类脱离了 Thread 继承体系，更灵活



## 方法三：使用FutureTask与Thread结合

方法二的任务对象Runnable是没有返回值的，想要带返回值，就需要用新的对象，FutureTask

使用FutureTask可以用泛型指定线程的返回值类型，需要搭配Callable来使用

```java
@Slf4j
public class CreateThread3 {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        // 创建任务对象
        FutureTask<Integer> futuretask = new FutureTask<>(new Callable<Integer>() {
            @Override
            public Integer call() throws Exception {
                log.debug("myThread3 is running");
                Thread.sleep(2000);
                return 2;
            }
        });
        log.debug("main thread is running");

        // 创建线程对象
        Thread myThread3 = new Thread(futuretask, "myThread3");
        myThread3.start();

        // 主线程阻塞，同步等待 task 执行完毕的结果
        log.debug("阻塞了{}s",futuretask.get());
    }
}
```

> 19:14:02.294 [main] DEBUG CreateThread3 - main thread is running
> 19:14:02.296 [myThread3] DEBUG CreateThread3 - myThread3 is running
> 19:14:04.297 [main] DEBUG CreateThread3 - 阻塞了2s



# 五、线程运行原理

多个Java线程同时运行是交替运行的，谁先谁后是由操作系统的任务调度器控制

查看进程线程的方法：暂时省略



## 1. 栈与栈帧

每个线程启动后，虚拟机就会为其分配一块栈内存。

* 每个栈由多个栈帧（Frame）组成，对应着每次方法调用时所占用的内存
* 每个线程只能有一个活动栈帧，对应着当前正在执行的那个方法（视频03.011图解栈帧讲的挺好）
* 线程拥有独立的栈内存，彼此之间不受影响



## 2. 线程上下文切换（Thread Context Switch）

所谓的上下文切换其实也就是线程切换，即cpu 不再执行当前的线程，转而执行另一个线程的代码。一般会是因为i以下几种情况：

- 分给线程的 cpu 时间片用完
- 垃圾回收 
- 有更高优先级的线程需要运行
- 线程自己调用了 sleep、yield、wait、join、park、synchronized、lock 等方法

当线程切换发生时，需要由操作系统保存当前线程的状态，并恢复另一个线程的状态，Java 中对应的概念 就是**程序计数器**（Program Counter Register），它的作用是记住下一条 jvm 指令的执行地址，是线程私有的

- 状态包括程序计数器、虚拟机栈中每个栈帧的信息，如局部变量、操作数栈、返回地址等
- **线程切换频繁发生会影响性能**



# 六、线程常见方法

## 1. start与run

新线程启动时**必须使用start方法**，如果直接使用**run方法，会是主线程在运行run方法，并没有启动新的线程**。使用 start 是启动新的线程，通过新的线程间接执行 run 中的代码

```java
public static void main(String[] args) {
    Thread t1 = new Thread("t1") {
        @Override
        public void run() {
			log.debug("running ...");
        }
    };
    t1.run(); //19:39:18 [main] c.TestStart - running ...
}
```



**start方法只能调用一次，多次调用会抛出异常**



## 2. sleep与yield

**sleep** (使线程阻塞)

* 调用 sleep()方法会让当前线程从 **Running 进入 Timed Waiting 状态（阻塞）**，可通过state()方法查看线程状态。

    * 处于阻塞状态的线程，CPU不会给其分配时间片

    * 建议用 **TimeUnit 的 sleep** 代替 Thread 的 sleep 来获得更好的可读性 。如：

        ```java
        //休眠一秒
        TimeUnit.SECONDS.sleep(1);
        //休眠一分钟
        TimeUnit.MINUTES.sleep(1);
        ```

* 其它线程可以使用 **interrupt** 方法打断正在睡眠的线程，`myThread.interrupt()`，这时 sleep 方法会抛出 InterruptedException

* 睡眠结束后的线程未必会立刻得到执行，CPU可能正在执行其他线程，需要等待任务调度器分配时间片给它



**yield （让出当前线程）**

* 调用 yield 会让当前线程从 **Running 进入 Runnable 就绪状态**（仍然有可能被执行，不同于阻塞状态，cpu分配时间片不会考虑阻塞状态），然后调度执行其它线程
* 具体的实现依赖于操作系统的任务调度器，cpu太闲的话，你让出去，任务调度器也会去执行，和阻塞还是完全不同的



## 3. setPriority 设置线程优先级

### 线程优先级

- 线程优先级会提示（hint）调度器优先调度该线程，但它仅仅是一个提示，调度器可以忽略它
- 如果 cpu 比较忙，那么优先级高的线程会获得更多的时间片，但 cpu 闲时，优先级几乎没作用

```java
thread1.setPriority(Thread.MAX_PRIORITY); //设置为优先级最高
```



yield和设置优先级均只是一个提示，真正的运行还是得看任务调度器



## 4. join() 等待

用于等待某个线程结束。如在主线程中调用ti.join()，则是主线程等待t1线程结束

join()还有带参数的形式，表示最多等待的时间。如果一个线程被阻塞了10s，join的参数是2s，那么最多等待2s；如果一个线程被阻塞了1s，join的参数是2s，那么只用等待1s

```java
Thread thread = new Thread();
//等待thread线程执行结束
thread.join();
//最多等待1000ms,如果1000ms内线程执行完毕，则会直接执行下面的语句，不会等够1000ms
thread.join(1000);
```



## 5. interrupt() 打断

interrupt() 用于打断线程，每个线程会有一个标志位`isInterrupted()`，用于记录打断的状态，默认是false

```java
//用于查看打断标记，返回值被boolean类型
t1.isInterrupted();
//更推荐使用当前线程的方法
Thread.currentThread().isInterrupted();
//无法手动设置标志位状态！！！
Thread.currentThread().isInterrupted()=true; //会报错
```

（1）如果是打断正处于sleep，wait，join 的线程（这几个方法都会让线程进入阻塞状态），会清空打断状态，打断状态为false

```java
private static void test1() throws InterruptedException {
    Thread t1 = new Thread(()->{
        sleep(1);
    }, "t1");
    t1.start();
    sleep(0.5);
    t1.interrupt();
    log.debug(" 打断状态: {}", t1.isInterrupted());
}
```

```java
java.lang.InterruptedException: sleep interrupted
	...
21:18:10.374 [main] c.TestInterrupt - 打断状态: false
```

还需要指出，线程有另一个方法是`interrupted()` ，和`isInterrupted()`类似，但他不会清空打断标记

（2）打断正常运行的线程, 不会清空打断状态，打断标记为true，**但不会停止**，会继续执行。如果要让线程在被打断后停下来，需要**使用打断标记来判断**。这是一种更优雅的处理方式

```java
public static void main(String[] args) throws InterruptedException {
    Thread t1 = new Thread(()->{
        while (true){
            boolean interrupted = Thread.currentThread().isInterrupted();
            if (interrupted){
                log.debug("被打断了，退出循环");
                break;
            }
        }
    },"t1");

    t1.start();
    TimeUnit.SECONDS.sleep(1);
    log.debug("interrupt");
    t1.interrupt();
}
```

```
23:50:39.051 [main] DEBUG ThreadInterruptTest - interrupt
23:50:39.053 [t1] DEBUG ThreadInterruptTest - 打断状态：true
```

### **interrupt方法的应用**——两阶段终止模式

当我们在执行线程一时，想要终止线程二，这是就需要使用interrupt方法来**优雅**的停止线程二。这里的【优雅】指的是给 T2 一个料理后事的机会。

![image-20220104162511467](https://jswanyu-1309100582.cos.ap-shanghai.myqcloud.com/picgo/%E4%B8%A4%E6%AE%B5%E7%BB%88%E6%AD%A2%E6%A8%A1%E5%BC%8F.png)

```java
@Slf4j
public class TwoPhaseTermination {
    public static void main(String[] args) throws InterruptedException {
        TPTInteruption tpt = new TPTInteruption();
        tpt.start();
        Thread.sleep(5000);
        log.debug("主线程打断监控线程");
        tpt.stop();
    }
}

@Slf4j
class TPTInteruption{
    private Thread monitor;

    // 启动监控线程
    public void start(){
        // 创建一个监控线程
        monitor = new Thread(() -> {
            while (true){
                if (Thread.currentThread().isInterrupted()){
                    // 如果是正常打断，则料理后事然后结束
                    log.debug("料理后事");
                    break;
                }
                // 每隔1s监控一次
                try {
                    Thread.sleep(2000);
                    log.debug("执行监控");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                    // 睡眠时捕获到打断异常，重新设置打断标记，让其为真，好退出循环
                    Thread.currentThread().interrupt();
                }
            }
        },"监控线程");

        monitor.start();
    }

    // 停止线程
    public void stop(){
        monitor.interrupt();
    }
}
```



### 其他：打断park线程，遇到再说



## 6. 不推荐的方法

还有一些不推荐使用的方法，这些方法已过时，容易破坏同步代码块，造成线程死锁

stop() --  停止线程运行
suspend() --  挂起（暂停）线程运行
resume() --  恢复线程运行





# 七、主线程和守护线程



默认情况下，Java 进程需要等待所有线程都运行结束，才会结束。有一种特殊的线程叫做守护线程，只要其它非守
护线程运行结束了，即使守护线程的代码没有执行完，也会强制结束。

```java
//在线程开启前将线程设置为守护线程, 默认为false
monitor.setDaemon(true);
monitor.start();
```

注意：

* 垃圾回收器线程就是一种守护线程
* Tomcat 中的 Acceptor 和 Poller 线程都是守护线程，所以 Tomcat 接收到 shutdown 命令后，不会等待它们处理完当前请求



# 八、线程状态

## 1. 五种状态

这是从 操作系统 层面来描述的

![image-20220112213855310](https://jswanyu-1309100582.cos.ap-shanghai.myqcloud.com/picgo/%E7%BA%BF%E7%A8%8B%E7%8A%B6%E6%80%81.png)

- 【初始状态】仅是在语言层面创建了线程对象，还未与操作系统线程关联（例如线程调用了start方法）
- 【可运行状态】（就绪状态）指该线程已经被创建（与操作系统线程关联），可以由 CPU 调度执行
- 【运行状态】指获取了 CPU 时间片运行中的状态
    - 当 CPU 时间片用完，会从【运行状态】转换至【可运行状态】，会导致线程的上下文切换
- 【阻塞状态】
    - 如果调用了阻塞 API，如 BIO 读写文件，这时该线程实际不会用到 CPU，会导致线程上下文切换，进入 【阻塞状态】
    - 等 BIO 操作完毕，会由操作系统唤醒阻塞的线程，转换至【可运行状态】
    - 与【可运行状态】的区别是，对【阻塞状态】的线程来说只要它们一直不唤醒，调度器就一直不会考虑调度它们
- 【终止状态】表示线程已经执行完毕，生命周期已经结束，不会再转换为其它状态



## 2. 六种状态

这是从 Java API 层面来描述的，根据 Thread.State 枚举，分为六种状态

![image-20220112215155531](https://jswanyu-1309100582.cos.ap-shanghai.myqcloud.com/picgo/%E7%BA%BF%E7%A8%8B%E7%8A%B6%E6%80%812.png)

- **NEW** 线程刚被创建，但是还没有调用 start() 方法
- **RUNNABLE** 当调用了 start() 方法之后，注意，Java API 层面的 RUNNABLE 状态涵盖了操作系统层面的 【可运行状态】、【运行状态】和【阻塞状态】（由于 BIO 导致的线程阻塞，在 Java 里无法区分，仍然认为 是可运行）
- **BLOCKED ， WAITING ， TIMED_WAITING** 都是 **Java API 层面**对【阻塞状态】的细分，如sleep就位TIMED_WAITING， join为WAITING状态。后面会在状态转换一节详述。
- **TERMINATED** 当线程代码运行结束





















