- [Java 中的 Monitor 机制](https://segmentfault.com/a/1190000016417017)
- [深入理解多线程（四）—— Moniter的实现原理](https://www.hollischuang.com/archives/2030)

- [UNDERSTANDING THREADS, MONITORS AND LOCKS](http://www.javacreed.com/understanding-threads-monitors-and-locks/)



# Object level lock vs Class level lock

**Object level**：

```java
public class DemoClass{
    public synchronized void demoMethod(){}
}

public class DemoClass{
    public void demoMethod(){
        synchronized (this){
            //other thread safe code
        }
    }
}

public class DemoClass{
    private final Object lock = new Object();
    public void demoMethod(){
        synchronized (lock){
            //other thread safe code
        }
    }
}
```



**Class level**

```java
public class DemoClass{
    //Method is static
    public synchronized static void demoMethod(){}
}

public class DemoClass{
    public void demoMethod(){
        //Acquire lock on .class reference
        synchronized (DemoClass.class){
            //other thread safe code
        }
    }
}

public class DemoClass{
    private final static Object lock = new Object();
    public void demoMethod(){
        //Lock object is static
        synchronized (lock){
            //other thread safe code
        }
    }
}
```



1. Synchronization in Java guarantees that no two threads can execute a synchronized method, which requires same lock, simultaneously or concurrently.
2. `synchronized` keyword can be used only with methods and code blocks. These methods or blocks can be *static* or *non-static* both.
3. When ever a thread enters into Java `synchronized` method or block it acquires a lock and whenever it leaves synchronized method or block it releases the lock. Lock is released even if thread leaves synchronized method after completion or due to any Error or Exception.
4. Java `synchronized` keyword is **re-entrant** in nature it means if a synchronized method calls another synchronized method which requires same lock then current thread which is holding lock can enter into that method without acquiring lock.
5. Java synchronization will throw `NullPointerException` if object used in synchronized block is null. For example, in above code sample if lock is initialized as null, the “`synchronized (lock)`” will throw `NullPointerException`.
6. Synchronized methods in Java put a performance cost on your application. So use synchronization when it is absolutely required. Also, consider using synchronized code blocks for synchronizing only critical section of your code.
7. It’s possible that both static synchronized and non static synchronized method can run simultaneously or concurrently because they lock on different object.
8. According to the Java language specification you can not use `synchronized` keyword with constructor. It is illegal and result in compilation error.
9. Do not synchronize on non final field on synchronized block in Java. because reference of non final field may change any time and then different thread might synchronizing on different objects i.e. no synchronization at all.
10. Do not use String literals because they might be referenced else where in the application and can cause deadlock. String objects created with `new` keyword can be used safely. But as a best practice, create a new **private** scoped `Object` instance OR lock on the shared variable itself which we want to protect. [Thanks to Anu to point this out in comments.]



# Java Compare and Swap Example – CAS Algorithm

## 1. Optimistic and Pessimistic Locking

Traditional locking mechanisms, e.g. **using synchronized keyword in java, is said to be pessimistic technique** of locking or multi-threading. It asks you to first guarantee that no other thread will interfere in between certain operation (i.e. lock the object), and then only allow you access to any instance/method.

> It’s much like saying “please close the door first; otherwise some other crook will come in and rearrange your stuff”.

Though above approach is safe and it does work, but it **put a significant penalty on your application in terms of performance**. Reason is simple that waiting threads can not do anything unless they also get a chance and perform the guarded operation.

There exist one more approach which is more efficient in performance, and it **optimistic** in nature. In this approach, you proceed with an update, **being hopeful that you can complete it without interference**. This approach relies on collision detection to determine if there has been interference from other parties during the update, in which case the operation fails and can be retried (or not).

> The optimistic approach is like the old saying, “It is easier to obtain forgiveness than permission”, where “easier” here means “more efficient”.

**Compare and Swap** is a good example of such optimistic approach, which we are going to discuss next.

## 2. Compare and Swap Algorithm

This algorithm compares the contents of a memory location to a given value and, only if they are the same, modifies the contents of that memory location to a given new value. This is done as a single atomic operation. The atomicity guarantees that the new value is calculated based on up-to-date information; if the value had been updated by another thread in the meantime, the write would fail. The result of the operation must indicate whether it performed the substitution; this can be done either with a simple Boolean response (this variant is often called compare-and-set), or by returning the value read from the memory location (not the value written to it).

There are 3 parameters for a CAS operation:

1. A memory location V where value has to be replaced
2. Old value A which was read by thread last time
3. New value B which should be written over V

> CAS says “I think V should have the value A; if it does, put B there, otherwise don’t change it but tell me I was wrong.” CAS is an optimistic technique—it proceeds with the update in the hope of success, and can detect failure if another thread has updated the variable since it was last examined.



# How to work with wait(), notify() and notifyAll() in Java?

In general, a thread that uses the wait() method confirms that a condition does not exist (typically by checking a variable) and then calls the wait() method. When another thread establishes the condition (typically by setting the same variable), it calls the notify() method. The wait-and-notify mechanism does not specify what the specific condition/ variable value is. It is on developer’s hand to specify the condition to be checked before calling `wait()` or `notify()`.

## Interview questions on wait(), notify() and notifyAll() methods

#### 3.1. What happens when notify() is called and no thread is waiting?

In general practice, this will not be the case in most scenarios if these methods are used correctly. Though if the `notify()` method is called when no other thread is waiting, `notify()` simply returns and the notification is lost.

Since the *wait-and-notify mechanism* does not know the condition about which it is sending notification, it assumes that a notification goes unheard if no thread is waiting. A thread that later executes the **wait()** method has to wait for another notification to occur.

#### 3.2. Can there be a race condition during the period that the wait() method releases OR reacquires the lock?

The `wait()` method is tightly integrated with the lock mechanism. The object lock is not actually freed until the waiting thread is already in a state in which it can receive notifications. It means only when thread state is changed such that it is able to receive notifications, lock is held. The system prevents any race conditions from occurring in this mechanism.

Similarly, system ensures that lock should be held by object completely before moving the thread out of waiting state.

#### 3.3. If a thread receives a notification, is it guaranteed that the condition is set correctly?

Simply, no. Prior to calling the `wait()` method, a thread should always test the condition while holding the synchronization lock. Upon returning from the `wait()` method, the thread should always retest the condition to determine if it should wait again. This is because another thread can also test the condition and determine that a wait is not necessary — processing the valid data that was set by the notification thread.

This is a common case when multiple threads are involved in the notifications. More particularly, the threads that are processing the data can be thought of as consumers; they consume the data produced by other threads. There is no guarantee that when a consumer receives a notification that it has not been processed by another consumer.

As such, when a consumer wakes up, it cannot assume that the state it was waiting for is still valid. It may have been valid in the past, but the state may have been changed after the `notify()` method was called and before the consumer thread woke up. Waiting threads must provide the option to check the state and to return back to a waiting state in case the notification has already been handled. This is why we always put calls to the wait() method in a loop.

#### 3.4. What happens when more than one thread is waiting for notification? Which threads actually get the notification when the notify() method is called?

It depends on many factors.Java specification doesn’t define which thread gets notified. In runtime, which thread actually receives the notification varies based on several factors, including the implementation of the Java virtual machine and scheduling and timing issues during the execution of the program.

There is no way to determine, even on a single processor platform, which of multiple threads receives the notification.

Just like the `notify()` method, the `notifyAll()` method does not allow us to decide which thread gets the notification: they all get notified. When all the threads receive the notification, it is possible to work out a mechanism for the threads to choose among themselves which thread should continue and which thread(s) should call the `wait()` method again.

#### 3.5. Does the notifyAll() method really wake up all the threads?

Yes and no. All of the waiting threads wake up, but they still have to reacquire the object lock. So the threads do not run in parallel: they must each wait for the object lock to be freed. Thus, only one thread can run at a time, and only after the thread that called the notifyAll() method releases its lock.

#### 3.6. Why would you want to wake up all of the threads if only one is going to execute at all?

There are a few reasons. For example, there might be more than one condition to wait for. Since we cannot control which thread gets the notification, it is entirely possible that a notification wakes up a thread that is waiting for an entirely different condition.

By waking up all the threads, we can design the program so that the threads decide among themselves which thread should execute next. Another option could be when producers generate data that can satisfy more than one consumer. Since it may be difficult to determine how many consumers can be satisfied with the notification, an option is to notify them all, allowing the consumers to sort it out among themselves.



# Difference between sleep() and wait() in Java

The major difference is that `wait()` releases the lock or monitor while `sleep()` doesn’t releases the lock or monitor while waiting. `wait()` is used for inter-thread communication while `sleep()` is used to introduce pause on execution, generally.

**Thread.sleep()** sends the current thread into the “*Not Runnable*” state for some amount of time. The thread keeps the monitors it has acquired — i.e. if the thread is currently in a `synchronized` block or method no other thread can enter this block or method. If another thread calls `t.interrupt()`. it will wake up the sleeping thread.

While `sleep()` is a `static` method which means that it always affects the current thread (the one that is executing the sleep method). A common mistake is to call `t.sleep()` where `t` is a different thread; even then, it is the current thread that will sleep, not the `t` thread.

```java
synchronized(LOCK) {  
    Thread.sleep(1000); // LOCK is held
}

synchronized(LOCK) {  
    Thread.sleep(1000); // LOCK is held
}
```

## Java sleep() vs wait() – Summary

#### 1. Method called on

- `wait()` – Call on an object; current thread must synchronize on the lock object.
- `sleep()` – Call on a Thread; always currently executing thread.

#### 2. Synchronized

- `wait()` – when synchronized multiple threads access same Object one by one.
- `sleep()` – when synchronized multiple threads wait for sleep over of sleeping thread.

#### 3. Lock duration

- `wait()` – release the lock for other objects to have chance to execute.
- `sleep()` – keep lock for at least t times if timeout specified or somebody interrupt.

#### 4. wake up condition

- `wait()` – until call notify(), notifyAll() from object
- `sleep()` – until at least time expire or call interrupt().

#### 5. Usage

- `sleep()` – for time-synchronization
- `wait()` – for multi-thread-synchronization.



# Difference between Runnable vs Thread in Java

```java
public class DemoRunnable implements Runnable {
    public void run() {
        //Code
    }
}
 
//start new thread with a "new Thread(new demoRunnable()).start()" call

public class DemoThread extends Thread {
    public DemoThread() {
        super("DemoThread");
    }
    public void run() {
        //Code
    }
}
//start new thread with a "new demoThread().start()" call
```



There has been a good amount of debate on which is better way. Well, I also tried to find out and below is my learning.

1. Implementing `Runnable` is the preferred way to do it. Here, you’re not really specializing or modifying the thread’s behavior. You’re just giving the thread something to run. That means composition is the better way to go.

2. Java only supports single inheritance, so you can only extend one class.

3. Instantiating an interface gives a cleaner separation between your code and the implementation of threads.

4. Implementing `Runnable` makes your class more flexible. If you extend `Thread` then the action you’re doing is always going to be in a thread. However, if you implement `Runnable` it doesn’t have to be. You can run it in a thread, or pass it to some kind of executor service, or just pass it around as a task within a single threaded application.

5. If you are working on JDK 4 or lesser, then there is bug :

   [http://bugs.java.com/bugdatabase/view_bug.do;jsessionid=5869e03fee226ffffffffc40d4fa881a86e3:WuuT?bug_id=4533087](http://bugs.java.com/bugdatabase/view_bug.do;jsessionid=5869e03fee226ffffffffc40d4fa881a86e3:WuuT?bug_id=4533087%20terget=)

   It’s fixed in Java 1.5 but Sun doesn’t intend to fix it in 1.4.

   The issue is that at construction time, a `Thread` is added to a list of references in an internal thread table. It won’t get removed from that list until its `start()` method has completed. As long as that reference is there, it won’t get garbage collected.

   [the advantage is – when there are multiple threads then, memory usage would be more in case of extends Thread. Because, each of your threads contains unique object associated with it. Where as, memory usage would be less in case of implements Runnable. Because, many threads can share the same runnable instance.]

That’s all about **differences between Runnable interface and Thread class in java**. If you know something more, please put that in comments section and I will include in post content.



# Difference between lock and monitor

Short answer, locks provide necessary support for implementing monitors.

## Locks

**A lock is kind of data which is logically part of an object’s header on the heap memory.** Each object in a JVM has this lock (or mutex) that any program can use to coordinate multi-threaded access to the object. If any thread want to access instance variables of that object; then thread must “own” the object’s lock (set some flag in lock memory area). All other threads that attempt to access the object’s variables have to wait until the owning thread releases the object’s lock (unset the flag).

Once a thread owns a lock, it can request the same lock again multiple times, but then has to release the lock the same number of times before it is made available to other threads. If a thread requests a lock three times, for example, that thread will continue to own the lock until it has “released” it three times.

Please note that lock is acquired by a thread, when it explicitly ask for it. In Java, this is done with the synchronized keyword, or with `wait` and `notify`.

## Monitors

- [深入理解多线程（四）—— Moniter的实现原理](https://www.hollischuang.com/archives/2030)
- [Java 中的 Monitor 机制](https://segmentfault.com/a/1190000016417017)

**Monitor is a synchronization construct that allows threads to have both mutual exclusion (using locks) and cooperation** i.e. the ability to make threads wait for certain condition to be true (using **wait-set**).

In other words, along with data that implements a lock, every Java object is logically associated with data that implements a `wait-set`. Whereas locks help threads to work independently on shared data without interfering with one another, wait-sets help threads to cooperate with one another to work together towards a common goal e.g. all waiting threads will be moved to this wait-set and all will be notified once lock is released. **This wait-set helps in building monitors with additional help of lock (mutex).**

#### Mutual exclusion

Putting in very simple words, a monitor is like a building that contains one special room (object instance) that can be occupied by only one thread at a time. The room usually contains some data which needs to be protected from concurrent access. From the time a thread enters this room to the time it leaves, it has exclusive access to any data in the room. Entering the monitor building is called “entering the monitor.” Entering the special room inside the building is called “acquiring the monitor.” Occupying the room is called “owning the monitor,” and leaving the room is called “releasing the monitor.” Leaving the entire building is called “exiting the monitor.”

When a thread arrives to access protected data (enter the special room), it is first put in queue in building reception (entry-set). If no other thread is waiting (own the monitor), the thread acquires the lock and continues executing the protected code. When the thread finishes execution, it release the lock and exits the building (exiting the monitor).

If when a thread arrives and another thread is already owning the monitor, it must wait in reception queue (entry-set). When the current owner exits the monitor, the newly arrived thread must compete with any other threads also waiting in the entry-set. Only one thread will win the competition and own the lock.

**There is no role of wait-set feature.**

#### Cooperation

In general, mutual exclusion is important only when multiple threads are sharing data or some other resource. If two threads are not working with any common data or resource, they usually can’t interfere with each other and needn’t execute in a mutually exclusive way. Whereas mutual exclusion helps keep threads from interfering with one another while sharing data, cooperation helps threads to work together towards some common goal.

**Cooperation is important when one thread needs some data to be in a particular state and another thread is responsible for getting the data into that state e.g. producer/consumer problem** where read thread needs the buffer to be in a “not empty” state before it can read any data out of the buffer. If the read thread discovers that the buffer is empty, it must wait. The write thread is responsible for filling the buffer with data. Once the write thread has done some more writing, the read thread can do some more reading. It is also sometimes called a “**Wait and Notify**” OR “**Signal and Continue**” monitor because it retains ownership of the monitor and continues executing the monitor region (the continue) if needed. At some later time, the notifying thread releases the monitor and a waiting thread is resurrected to own the lock.

**This cooperation requires both i.e. entry-set and wait-set.** Below given diagram will help you in understand this cooperation.

![java-monitor](https://cdn2.howtodoinjava.com/wp-content/uploads/2015/10/java-monitor.gif)

Above figure shows the monitor as three rectangles. In the center, a large rectangle contains a single thread, the monitor’s owner. On the left, a small rectangle contains the entry set. On the right, another small rectangle contains the wait set.

Entry-sets are used in the Monitor to access a shared resource/data and Wait-sets are used in case there is a prerequisite corporation between Threads(one before another).So the former is used for concurrent access and the later for waiting-other-thread-processing.



# Difference between yield() and join()

A Java virtual machine is required to implement a **preemptive, priority-based scheduler** among its various threads. The priority value is important because the contract between the Java virtual machine and the underlying operating system is that the ***operating system must generally choose to run the Java thread with the highest priority***. 

Also note that **java does not mandate that its threads be time-sliced**, but most operating systems do so. There is often some confusion in terminology here: preemption is often confused with time-slicing. In fact, **preemption means only that a higher-priority thread runs instead of a lower-priority one**, but when threads have the same priority, they do not preempt each other. They are typically subject to time-slicing, but that is not a requirement of Java.

## Understanding thread priorities

1. Remember that all the threads carry normal priority when a priority is not specified.
2. Priorities can be specified from 1 to 10. 10 being the highest, 1 being the lowest priority and 5 being the normal priority.
3. Remember that the thread with highest priority will be given preference in execution. But there is no guarantee that it will be in running state the moment it starts.
4. Always the currently executing thread might have the higher priority when compared to the threads in the pool who are waiting for their chance.
5. It is the thread scheduler which decides what thread should be executed.
6. t.setPriority() can be used to set the priorities for the threads.
7. Remember that the priorities should be set before the threads start method is invoked.
8. You can use the constants, MIN_PRIORITY,MAX_PRIORITY and NORM_PRIORITY for setting priorities.



## yield() method

Theoretically, **to ‘yield’ means to let go, to give up, to surrender**. A yielding thread tells the virtual machine that it’s willing to let other threads be scheduled in its place. This indicates that it’s not doing something too critical. Note that *it’s only a hint*, though, and not guaranteed to have any effect at all.

yield() is defined as following in Thread.java.

```java
`/**``  ``* A hint to the scheduler that the current thread is willing to yield its current use of a processor. The scheduler is free to ignore``  ``* this hint. Yield is a heuristic attempt to improve relative progression between threads that would otherwise over-utilize a CPU.``  ``* Its use should be combined with detailed profiling and benchmarking to ensure that it actually has the desired effect.``  ``*/` `public` `static` `native` `void` `yield();`
```

Let’s list down important points from above definition:

- Yield is a Static method and Native too.

- Yield tells the currently executing thread to give a chance to the threads that have equal priority in the [**Thread Pool**](https://howtodoinjava.com/java-5/java-executor-framework-tutorial-and-best-practices/).[In thread-pool, `yield()` will try to suspend currently executing thread, and JVM will first try to choose a thread with equal priority inside pool, if none is found – it may choose the original thread or may choose any other thread of unequal priority.
  Again, It is on best effort basis. This execution order is not guaranteed as most of the cases of the cases in multi-threading.

  Yield method only suggest that current thread is ready to give up resources if some other thread want to take over. But JVM may choose to select that very thread everytime, irrespective of all reasoning. No guarantee at all.]

- There is no guarantee that Yield will make the currently executing thread to runnable state immediately.

- It can only make a thread from Running State to Runnable State, not in wait or blocked state.[The thread is in runnable state after invocation of start() method, but the thread scheduler has not selected it to be the running thread. The thread is in running state if the thread scheduler has selected it. The thread is in running state if the thread scheduler has selected it.]

## join() method

The join() method of a Thread instance can be **used to “join” the start of a thread’s execution to the end of another thread’s execution** so that a thread will not start running until another thread has ended. If join() is called on a Thread instance, the currently running thread will block until the Thread instance has finished executing.

```java
`//Waits for this thread to die.` `public` `final` `void` `join() ``throws` `InterruptedException`
```

***Giving a timeout within join(), will make the join() effect to be nullified after the specific timeout.*** When the timeout is reached, the main thread and taskThread are equally probable candidates to execute. However, as with sleep, join is dependent on the OS for timing, so you should not assume that join will wait exactly as long as you specify.



# Java Thread Pool

A **thread pool is a collection of pre-initialized threads**. Generally the size of collection is fixed, but it is not mandatory. It facilitates the execution of N number of tasks using same threads. If thread are more tasks than threads, then tasks need to wait in a queue like structure ([FIFO – First in first out](https://en.wikipedia.org/wiki/FIFO_and_LIFO_accounting#FIFO)).

When any thread completes it’s execution, it can pickup a new task from queue and execute it. When all tasks are completed the threads remain active and wait for more tasks in thread pool.

![Thread Pool](https://cdn1.howtodoinjava.com/wp-content/uploads/2015/03/Thread_pool.png)

A watcher keep watching queue (usually [BlockingQueue](https://howtodoinjava.com/java/multi-threading/how-to-use-blockingqueue-and-threadpoolexecutor-in-java/)) for any new tasks. As soon as tasks come, threads again start picking up tasks and execute them.

## How to create ThreadPoolExecutor

We can create following 5 types of thread pool executors with pre-built methods in `java.util.concurrent.Executors` interface.

1. Fixed thread pool executor– Creates a thread pool that reuses a fixed number of threads to execute any number of tasks. If additional tasks are submitted when all threads are active, they will wait in the queue until a thread is available. It is best fit for most off the real-life usecases.

   `ThreadPoolExecutor executor = (ThreadPoolExecutor) Executors.newFixedThreadPool(``10``);`

2. Cached thread pool executor– Creates a thread pool that creates new threads as needed, but will reuse previously constructed threads when they are available. DO NOT use this thread pool if tasks are long running. It can bring down the system if number of threads goes beyond what system can handle.

   `ThreadPoolExecutor executor = (ThreadPoolExecutor) Executors.newCachedThreadPool();`

3. Scheduled thread pool executor– Creates a thread pool that can schedule commands to run after a given delay, or to execute periodically.

   ```java
   ThreadPoolExecutor executor = (ThreadPoolExecutor) Executors.newScheduledThreadPool(10);
   ```

4. Single thread pool executor– Creates single thread to execute all tasks. Ute it when you have only one task to execute.

   `ThreadPoolExecutor executor = (ThreadPoolExecutor) Executors.newSingleThreadExecutor();`

5. Work stealing thread pool executor-Creates a thread pool that maintains enough threads to support the given parallelism level. Here parallelism level means the maximum number of threads which will be used to execute a given task, at single point of time, in multi-processor machines.

   `ThreadPoolExecutor executor = (ThreadPoolExecutor) Executors.newWorkStealingPool(``4``);`

**Summary**

1. The `ThreadPoolExecutor` class has four different constructors but, due to their complexity, the Java concurrency API provides the `Executors` class to construct executors and other related objects. Although we can create `ThreadPoolExecutor` directly using one of its constructors, it’s recommended to use the `Executors`class.
2. The cached thread pool, we have created above, creates new threads if needed to execute the new tasks, and reuses the existing ones if they have finished the execution of the task they were running, which are now available. The cached thread pool has, however, a disadvantage of constant lying threads for new tasks, so if you send too many tasks to this executor, you can overload the system. This can be overcome using fixed thread pool, which we will learn in next tutorial.
3. One critical aspect of the `ThreadPoolExecutor` class, and of the executors in general, is that you have to end it explicitly. If you don’t do this, the executor will continue its execution and the program won’t end. If the executor doesn’t have tasks to execute, it continues waiting for new tasks and it doesn’t end its execution. A Java application won’t end until all its non-daemon threads finish their execution, so, if you don’t terminate the executor, your application will never end.
4. To indicate to the executor that you want to finish it, you can use the `shutdown()` method of the `ThreadPoolExecutor` class. When the executor finishes the execution of all pending tasks, it finishes its execution. After you call the `shutdown()` method, if you try to send another task to the executor, it will be rejected and the executor will throw a `RejectedExecutionException` exception.
5. The `ThreadPoolExecutor` class provides a lot of methods to obtain information about its status. We used in the example the `getPoolSize()`, `getActiveCount()`, and `getCompletedTaskCount()` methods to obtain information about the size of the pool, the number of threads, and the number of completed tasks of the executor. You can also use the `getLargestPoolSize()` method that returns the maximum number of threads that has been in the pool at a time.
6. The ThreadPoolExecutor class also provides other methods related with the finalization of the executor. These methods are:
   - **shutdownNow()**: This method shut downs the executor immediately. It doesn’t execute the pending tasks. It returns a list with all these pending tasks. The tasks that are running when you call this method continue with their execution, but the method doesn’t wait for their finalization.
   - **isTerminated()**: This method returns true if you have called the `shutdown()` or `shutdownNow()` methods and the executor finishes the process of shutting it down.
   - **isShutdown()**: This method returns true if you have called the `shutdown()` method of the executor.
   - **awaitTermination(long timeout,TimeUnitunit)**: This method blocks the calling thread until the tasks of the executor have ended or the timeout occurs. The `TimeUnit` class is an enumeration with the following constants: `DAYS`, `HOURS`, `MICROSECONDS` etc.

## Java Callable Future

`Callable` interface has the `call()` method. In this method, we have to implement the logic of a task. The `Callable`interface is a parameterized interface, meaning we have to indicate the type of data the `call()` method will return.

`Future` interface has methods to obtain the result generated by a `Callable` object and to manage its state.

## Java executor framework 

`ScheduledThreadPoolExecutor` provides other methods to schedule periodic tasks. It is the scheduleWithFixedRate() method. It has the same parameters as the `scheduledAtFixedRate()` method, but there is a difference worth noticing. In the `scheduledAtFixedRate()` method, the third parameter determines the period of time between the starting of two executions. In the `scheduledWithFixedRate()`method, parameter determines the period of time between the end of an execution of the task and the beginning of the next execution.

1) `newFixedThreadPool()` method creates an executor with a maximum number of threads at any time. If you send more tasks than the number of threads, the remaining tasks will be blocked until there is a free thread to process them This method receives the maximum number of threads as a parameter you want to have in your executor. In your case, you have created an executor with four threads.

2) The `Executors` class also provides the `newSingleThreadExecutor()` method. This is an extreme case of a fixed-size thread executor. It **creates an executor with only one thread**, so it can only execute one task at a time.



## CountDownLatch

CountDownLatch was **introduced with JDK 1.5 along with other concurrent utilities like CyclicBarrier, Semaphore, ConcurrentHashMap and BlockingQueue** in java.util.concurrent package. This class **enables a java thread to wait until other set of threads completes** their tasks. e.g. Application’s main thread want to wait, till other service threads which are responsible for starting framework services have completed started all services.

CountDownLatch works by having a counter initialized with number of threads, which is decremented each time a thread complete its execution. When count reaches to zero, it means all threads have completed their execution, and thread waiting on latch resume the execution.

![CountdownLatch](https://cdn2.howtodoinjava.com/wp-content/uploads/CountdownLatch_example.png)

## Difference between Lock Interface and synchronized keyword

```java
Lock lock = new ReentrantLock();
 
lock.lock();
//critical section
lock.unlock();
```

The main differences between a Lock and a synchronized block are:

1) Having a timeout trying to get access to a `synchronized` block is not possible. Using [Lock.tryLock(long timeout, TimeUnit timeUnit)](https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/locks/Lock.html#tryLock%28long,%20java.util.concurrent.TimeUnit%29), it is possible.
2) The `synchronized` block must be fully contained within a single method. A Lock can have it’s calls to `lock()` and `unlock()` in separate methods.