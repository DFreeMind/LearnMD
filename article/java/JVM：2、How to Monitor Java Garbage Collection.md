This is the second article in the series of "*Become a Java GC Expert*". In the first issue [Understanding Java Garbage Collection](https://www.cubrid.org/blog/understanding-java-garbage-collection/) we have learned about the processes for different GC algorithms, about how GC works, what Young and Old Generation is, what you should know about the 5 types of GC in the new JDK 7, and what the performance implications are for each of these GC types.

In this article, I will explain **how [JVM](https://www.cubrid.org/blog/understanding-jvm-internals/) is actually running Garbage Collection in the real time**.

## What is GC Monitoring? 

**Garbage Collection Monitoring** refers to the *process of figuring out how JVM is running GC*. For example, we can find out:

1. when an object in young has moved to old and by how much,
2. or when [stop-the-world](https://www.cubrid.org/blog/understanding-java-garbage-collection/#stop-the-world) has occurred and for how long.

GC monitoring is carried out *to see if JVM is running GC efficiently*, and *to check if additional GC tuning is necessary*. Based on this information, the application can be edited or GC method can be changed (**GC tuning**).

## How to Monitor GC?

There are different ways to monitor GC, but the only difference is how the GC operation information is shown. GC is done by JVM, and since the GC monitoring tools disclose the GC information provided by JVM, you will get the same results no matter how you monitor GC. Therefore, you do not need to learn all methods to monitor GC, but since it only requires a little amount of time to learn each GC monitoring method, knowing a few of them can help you use the right one for different situations and environments.

The tools or JVM options listed below cannot be used universally regardless of the HVM vendor. This is because there is no need for a "standard" for disclosing GC information. In this example we will use **HotSpot JVM** (Oracle JVM). Since NHN is using Oracle (Sun) JVM, there should be no difficulties in applying the tools or JVM options that we are explaining here.

First, the GC monitoring methods can be separated into **CUI** and **GUI** depending on the access interface. The typical CUI GC monitoring method involves using a separate CUI application called "**jstat**", or selecting a JVM option called "**verbosegc**" when running JVM.

GUI GC monitoring is done by using a separate GUI application, and three most commonly used applications would be "jconsole", "jvisualvm" and "Visual GC".

Let's learn more about each method.

## jstat

**jstat** is a monitoring tool in HotSpot JVM. Other monitoring tools for HotSpot JVM are **jps** and **jstatd**. Sometimes, you need all three tools to monitor a Java application.

**jstat** does not provide only the GC operation information display. It also provides class loader operation information or Just-in-Time compiler operation information. Among all the information jstat can provide, in this article we will only cover its functionality to *monitor* GC operating information.

**jstat** is located in $JDK_HOME/bin, so if *java* or *javac* can run without setting a separate directory from the command line, so can jstat.

You can try running the following in the command line.

```bsh
$> jstat –gc  $<vmid$> 1000 S0C       S1C       S0U    S1U      EC         EU          OC         OU         PC         PU         YGC     YGCT    FGC      FGCT     GCT3008.0   3072.0    0.0     1511.1   343360.0   46383.0     699072.0   283690.2   75392.0    41064.3    2540    18.454    4      1.133    19.5883008.0   3072.0    0.0     1511.1   343360.0   47530.9     699072.0   283690.2   75392.0    41064.3    2540    18.454    4      1.133    19.5883008.0   3072.0    0.0     1511.1   343360.0   47793.0     699072.0   283690.2   75392.0    41064.3    2540    18.454    4      1.133    19.588 $>
```

Just like in the example, the real type data will be output along with the following columns:
**S0C   S1C   S0U   S1U   EC   EU   OC   OU   PC**.

**vmid** (Virtual Machine ID), as its name implies, is the **ID** for the VM. Java applications running either on a local machine or on a remote machine can be specified using vmid. The vmid for Java application running on a local machine is called **lvmid** (Local vmid), and usually is PID. To find out the lvmid, you can write the PID value using a **ps** command or Windows task manager, but we suggest **jps** because PID and lvmid does not always match. **jps** stands for Java PS. jps shows *vmids* and main method information. Just like ps shows PIDs and process names.

Find out the vmid of the Java application that you want to monitor by using jps, then use it as a parameter in jstat. If you use jps alone, only bootstrap information will show when several WAS instances are running in one equipment. We suggest that you use **ps -ef | grep java** command along with **jps**.

GC performance data needs constant observation, therefore when running jstat, try to output the GC monitoring information on a regular basis. 

For example, running "**jstat –gc  1000**" (or 1s) will display the GC monitoring data on the console every 1 second. "**jstat –gc  1000 10**" will display the GC monitoring information once every 1 second for 10 times in total.

There are many options other than **-gc**, among which GC related ones are listed below.

| Option Name    | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| gc             | It shows the current size for each heap area and its current usage (Ede, survivor, old, etc.), total number of GC performed, and the accumulated time for GC operations. |
| gccapactiy     | It shows the minimum size (ms) and maximum size (mx) of each heap area, current size, and the number of GC performed for each area. (Does not show current usage and accumulated time for GC operations.) |
| gccause        | It shows the "information provided by -gcutil" + reason for the last GC and the reason for the current GC. |
| gcnew          | Shows the GC performance data for the new area.              |
| gcnewcapacity  | Shows statistics for the size of new area.                   |
| gcold          | Shows the GC performance data for the old area.              |
| gcoldcapacity  | Shows statistics for the size of old area.                   |
| gcpermcapacity | Shows statistics for the permanent area.                     |
| gcutil         | Shows the usage for each heap area in percentage. Also shows the total number of GC performed and the accumulated time for GC operations. |

Only looking at frequency, you will probably use **-gcutil** (or -gccause), **-gc** and **-gccapacity** the most in that order.

- **-gcutil** is used to check the usage of heap areas, the number of GC performed, and the total accumulated time for GC operations,
- while **-gccapacity** option and others can be used to check the actual size allocated.

You can see the following output by using the **-gc** option:

```bsh
S0C      S1C    …   GCT1248.0   896.0  …   1.2461248.0   896.0  …   1.246…        …      …   …
```

Different jstat options show different types of columns, which are listed below. Each column information will be displayed when you use the "jstat option" listed on the right.

| Column | Description                                                  | Jstat Option                                                 |
| ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| S0C    | Displays the current size of Survivor0 area in KB            | -gc -gccapacity -gcnew -gcnewcapacity                        |
| S1C    | Displays the current size of Survivor1 area in KB            | -gc -gccapacity -gcnew -gcnewcapacity                        |
| S0U    | Displays the current usage of Survivor0 area in KB           | -gc -gcnew                                                   |
| S1U    | Displays the current usage of Survivor1 area in KB           | -gc -gcnew                                                   |
| EC     | Displays the current size of Eden area in KB                 | -gc -gccapacity -gcnew -gcnewcapacity                        |
| EU     | Displays the current usage of Eden area in KB                | -gc -gcnew                                                   |
| OC     | Displays the current size of old area in KB                  | -gc -gccapacity -gcold -gcoldcapacity                        |
| OU     | Displays the current usage of old area in KB                 | -gc -gcold                                                   |
| PC     | Displays the current size of permanent area in KB            | -gc -gccapacity -gcold -gcoldcapacity -gcpermcapacity        |
| PU     | Displays the current usage of permanent area in KB           | -gc -gcold                                                   |
| YGC    | The number of GC event occurred in young area                | -gc -gccapacity -gcnew -gcnewcapacity -gcold -gcoldcapacity -gcpermcapacity -gcutil -gccause |
| YGCT   | The accumulated time for GC operations for Yong area         | -gc -gcnew -gcutil -gccause                                  |
| FGC    | The number of full GC event occurred                         | -gc -gccapacity -gcnew -gcnewcapacity -gcold -gcoldcapacity -gcpermcapacity -gcutil -gccause |
| FGCT   | The accumulated time for full GC operations                  | -gc -gcold -gcoldcapacity -gcpermcapacity -gcutil -gccause   |
| GCT    | The total accumulated time for GC operations                 | -gc -gcold -gcoldcapacity -gcpermcapacity -gcutil -gccause   |
| NGCMN  | The minimum size of new area in KB                           | -gccapacity -gcnewcapacity                                   |
| NGCMX  | The maximum size of max area in KB                           | -gccapacity -gcnewcapacity                                   |
| NGC    | The current size of new area in KB                           | -gccapacity -gcnewcapacity                                   |
| OGCMN  | The minimum size of old area in KB                           | -gccapacity -gcoldcapacity                                   |
| OGCMX  | The maximum size of old area in KB                           | -gccapacity -gcoldcapacity                                   |
| OGC    | The current size of old area in KB                           | -gccapacity -gcoldcapacity                                   |
| PGCMN  | The minimum size of permanent area in KB                     | -gccapacity -gcpermcapacity                                  |
| PGCMX  | The maximum size of permanent area in KB                     | -gccapacity -gcpermcapacity                                  |
| PGC    | The current size of permanent generation area in KB          | -gccapacity -gcpermcapacity                                  |
| PC     | The current size of permanent area in KB                     | -gccapacity -gcpermcapacity                                  |
| PU     | The current usage of permanent area in KB                    | -gc -gcold                                                   |
| LGCC   | The cause for the last GC occurrence                         | -gccause                                                     |
| GCC    | The cause for the current GC occurrence                      | -gccause                                                     |
| TT     | Tenuring threshold. If copied this amount of times in young area (S0 ->S1, S1->S0), they are then moved to old area. | -gcnew                                                       |
| MTT    | Maximum Tenuring threshold. If copied this amount of times inside young arae, then they are moved to old area. | -gcnew                                                       |
| DSS    | Adequate size of survivor in KB                              | -gcnew                                                       |

The advantage of **jstat** is that it can always monitor the GC operation data of Java applications running on local/remote machine, as long as a console can be used. From these items, the following result is output when **–gcutil** is used. At the time of GC tuning, pay careful attention to **YGC, YGCT, FGC, FGCT** and **GCT**.

```bsh
S0      S1       E        O        P        YGC    YGCT     FGC    FGCT     GCT0.00    66.44    54.12    10.58    86.63    217    0.928     2     0.067    0.9950.00    66.44    54.12    10.58    86.63    217    0.928     2     0.067    0.9950.00    66.44    54.12    10.58    86.63    217    0.928     2     0.067    0.995
```

These items are important because they show how much time was spent in running GC.

In this example, **YGC** is 217 and **YGCT** is 0.928. So, after calculating the arithmetical average, you can see that it required about *4 ms* (0.004 seconds) for each young GC. Likewise, the average full GC time us *33ms*.

But the arithmetical average often does not help analyzing the actual GC problem. This is due to the severe deviations in GC operation time. (In other words, if the average time is *0.067 seconds* for a full GC, one GC may have lasted 1 ms while the other one lasted *57 ms*.) In order to check the individual GC time instead of the arithmetical average time, it is better to use **-verbosegc**.

## -verbosegc

**-verbosegc** is one of the JVM options specified when running a Java application. While *jstat* can monitor any JVM application that has not specified any options, **-verbosegc** needs to be specified in the beginning, so it could be seen as an unnecessary option (since jstat can be used instead). However, as **-verbosegc** displays easy to understand output results whenever a GC occurs, it is very helpful for monitoring rough GC information.

|                    | jstat                                                        | -verbosegc                                                   |
| ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Monitoring Target  | Java application running on a machine that can log in to a terminal, or a remote Java application that can connect to the network by using jstatd | Only when -verbogc was specified as a JVM starting option    |
| Output information | Heap status (usage, maximum size, number of times for GC/time, etc.) | Size of ew and old area before/after GC, and GC operation time |
| Output Time        | Every designated time                                        | Whenever GC occurs                                           |
| Whenever useful    | When trying to observe the changes of the size of heap area  | When trying to see the effect of a single GC                 |

The followings are other options that can be used with **-verbosegc**.

- -XX:+PrintGCDetails
- -XX:+PrintGCTimeStamps
- -XX:+PrintHeapAtGC 
- -XX:+PrintGCDateStamps (from JDK 6 update 4)

If only **-verbosegc** is used, then **-XX:+PrintGCDetails** is applied by default. Additional options for **–verbosgc** are not exclusive and can be mixed and used together.

When using **-verbosegc**, you can see the results in the following format whenever a minor GC occurs.

```bsh
[GC [<collector>: <starting occupancy1> -> <ending occupancy1>, <pause time1> secs] <starting occupancy3> -> <ending occupancy3>, <pause time3> secs]
```

| Collector           | Name of Collector Used for minor gc                          |
| ------------------- | ------------------------------------------------------------ |
| starting occupancy1 | The size of young area before GC                             |
| ending occupancy1   | The size of young area after GC                              |
| pause time1         | The time when the Java application stopped running for minor GC |
| starting occupancy3 | The total size of heap area before GC                        |
| ending occupancy3   | The total size of heap area after GC                         |
| pause time3         | The time when the Java application stopped running for overall heap GC, including major GC |

This is an example of **-verbosegc** output for **minor GC**:

```bsh
S0    S1     E      O      P        YGC    YGCT    FGC    FGCT     GCT0.00  66.44  54.12  10.58  86.63    217    0.928     2    0.067    0.9950.00  66.44  54.12  10.58  86.63    217    0.928     2    0.067    0.9950.00  66.44  54.12  10.58  86.63    217    0.928     2    0.067    0.995
```

This is the example of output results after an **Full GC** occurred.

```bsh
[Full GC [Tenured: 3485K->4095K(4096K), 0.1745373 secs] 61244K->7418K(63104K), [Perm : 10756K->10756K(12288K)], 0.1762129 secs] [Times: user=0.19 sys=0.00, real=0.19 secs]
```

If a [CMS collector](https://www.cubrid.org/blog/understanding-java-garbage-collection/#cms-gc) is used, then the following CMS information can be provided as well.

As **-verbosegc** option outputs a log every time a GC event occurs, it is easy to see the changes of the heap usage rates caused by GC operation.

## (Java) VisualVM  + Visual GC

Java Visual VM is a GUI profiling/monitoring tool provided by Oracle JDK.

[![Figure 1: VisualVM Screenshot.](https://www.cubrid.org/files/attach/images/1747/748/001/e387fd9fbccb654f3d8db696fb215403.png)](https://www.cubrid.org/files/attach/images/220547/126/316/visual-vm.png)

**Figure 1: VisualVM Screenshot.**

Instead of the version that is included with JDK, you can download Visual VM directly from its website. For the sake of convenience, the version included with JDK will be referred to as Java VisualVM (jvisualvm), and the version available from the website will be referred to as Visual VM (visualvm). The features of the two are not exactly identical, as there are slight differences, such as when installing plug-ins. Personally, I prefer the Visual VM version, which can be downloaded from the website.

After running Visual VM, if you select the application that you wish to monitor from the window on the left side, you can find the "*Monitoring*" tab there. You can get the basic information about GC and Heap from this Monitoring tab. 

Though the basic GC status is also available through the basic features of VisualVM, you cannot access detailed information that is available from either **jstat** or **-verbosegc** option. 

If you want the detailed information provided by jstat, then it is recommended to install the Visual GC plug-in. 

Visual GC can be accessed in real time from the *Tools* menu.

![Figure 2:&nbsp;Viusal GC Installation Screenshot.](https://www.cubrid.org/files/attach/images/1747/748/001/28c621040950b1010adca98e19baad62.png)

**Figure 2: Viusal GC Installation Screenshot.**

By using Visual GC, you can see the information provided by running **jstatd** in a more intuitive way.  

[![Figure 3: Visual GC execution screenshot.](https://www.cubrid.org/files/attach/images/1747/748/001/1b37381a6b243d19cc73843dd134b86e.png)](https://www.cubrid.org/files/attach/images/220547/126/316/visual-gc-execution.png)

**Figure 3: Visual GC execution screenshot.**

## HPJMeter

[HPJMeter](https://h20392.www2.hp.com/portal/swdepot/displayProductInfo.do?productNumber=HPJMETER) is convenient for analyzing **-verbosegc** output results. If Visual GC can be considered as the GUI equivalent of *jstat*, then HPJMeter would be the GUI equivalent of *-verbosgc*. Of course, GC analysis is just one of the many features provided by HPJMeter. HPJMeter is a performance monitoring tool developed by HP. It can be used in HP-UX, as well as Linux and MS Windows.

Originally, a tool called **HPTune** used to provide the GUI analysis feature for **-verbosegc**. However, since the HPTune feature has been integrated into HPJMeter since version 3.0, there is no need to download HPTune separately.

When executing an application, the **-verbosegc** output results will be redirected to a separate file.

You can open the redirected file with HPJMeter, which allows faster and easier GC performance data analysis through the intuitive GUI.

[![Figure 4: HPJMeter.](https://www.cubrid.org/files/attach/images/1747/748/001/99ed9348c60cdcdfb00857ce8a7187ed.png)](https://www.cubrid.org/files/attach/images/220547/126/316/hpjmeter.png)

**Figure 4: HPJMeter.**

## What is the Next Article About?

In this article I focused on *how to monitor GC operation information*, as the preparation stage for GC tuning. From my personal experience, I suggest using **jstat** to monitor GC operation, and if you feel that it takes too lmuch time to execute GC, then try **-verbosegc** option to analyze GC. The general GC tuning process is *to analyze the results after applying the changed GC options* after the **-verbosegc** option has been applied based on the analysis. In the next article, we will see the best options for executing GC tuning by using real cases as our examples.