This is the *third* article in the series of "*Become a Java GC Expert*". In the first issue [Understanding Java Garbage Collection](https://www.cubrid.org/blog/understanding-java-garbage-collection/) we have learned about the processes for different GC algorithms, about how GC works, what Young and Old Generation is, what you should know about the 5 types of GC in the new JDK 7, and what the performance implications are for each of these GC types.

In the second article [How to Monitor Java Garbage Collection](https://www.cubrid.org/blog/how-to-monitor-java-garbage-collection/) I have explained how [JVM](https://www.cubrid.org/blog/understanding-jvm-internals/) actually runs the Garbage Collection in the real time, how we can monitor GC, and which tools we can use to make this process faster and more effective.

In this third article based on real cases as our examples **I will show some of the best options you can use for GC tuning**. I have written this article under the assumption that you have already understood the previous articles in this series. Therefore, for your further understanding, if you haven't already read the two previous articles, please do so before reading this one.

## Is GC Tuning Required?

Or more precisely **is GC tuning required for Java-based services**? I should say GC tuning is *not* always required for all Java-based services. This means a Java-based system in operation has the following options and actions:

 

- The memory size has been specified using `-Xms` and `–Xmx` options.
- The `-server` option is included.
- Logs such as *Timeout log* are not left in the system.

 

**In other words, if you have not set the memory size and too many Timeout logs are printed, you need to perform GC tuning on your system.**

But, there is one thing to keep in mind: **GC tuning is the last task to be done.**

Think about the fundamental cause of GC tuning. The Garbage Collector clears an object created in Java. The number of objects necessary to be cleared by the garbage collector as well as the number of GCs to be executed depend on the number of objects which have been created. Therefore, to control the GC performed by your system, you should, first, **decrease the number of objects created**.

There is a saying, "many a little makes a mickle." We need to take care of small things, or they will add up and become something big which is difficult to manage.

 

- We need to use and make `StringBuilder` or `StringBuffer` a way of life instead of `String`.
- And it is better to accumulate as few logs as possible.

 

However, we know that there are some cases we cannot help. We have seen that XML and JSON parsing use the most memory. Even though we use `String` as little as possible and process logs as well as we can, a huge temporary memory is used for parsing XML or JSON, some 10-100 MB. However, it is difficult not to use XML and JSON. Just understand that it takes too much memory.

If application memory usage improves after repeated tunings, you can start GC tuning. I classify the purposes of GC tuning into two.

 

1. One is to **minimize the number of objects passed to the old area**;
2. and the other is to **decrease Full GC execution time**.

 

### Minimizing Number of Objects Passed to Old Area

Generational GC is the GC provided by Oracle JVM, excluding the G1 GC which can be used from JDK 7 and higher versions. In other words, an object is created in the Eden area and transferred from and to the Survivor area. After that, the objects left are sent to the Old area. Some objects are created in the Eden area and directly passed to the Old area because of their large size. GC in the Old area takes relatively more time than the GC in the New area. Therefore, decreasing the number of objects passed to the Old area can decrease the full GC in frequency. Decreasing the number of objects passed to the Old area may be misunderstood as choosing to leave the object in the New area. However, this is impossible. Instead, you can **adjust the size of the New area**.

### Decreasing Full GC Time

The execution time of Full GC is relatively longer than that of Minor GC. Therefore, if it takes too much time to execute Full GC (1 second or more), timeout may occur in several connected parts.

 

- If you try to decrease the Old area size to decrease Full GC execution time, `OutOfMemoryError` may occur or the number of Full GCs may increase.
- Alternatively, if you try to decrease the number of Full GC by increasing the Old area size, the execution time will be increased.

 

Therefore, you need to **set the Old area size to a "proper" value**.

## Options Affecting the GC Performance

As I have mentioned at the end of [Understanding Java Garbage Collection](https://www.cubrid.org/blog/understanding-java-garbage-collection/), do not think that "**Somebody's got a great performance when he used GC options. Why don't we use that option as he did?**" The reason is that **the size of objects created and their lifetime is different from one Web service to another**.

Simply consider, if a task is performed under the conditions of A, B, C, D and E, and the same task is performed under the conditions of only A and B, then which one will be done quicker? From a common-sense standpoint, the answer would be *the task which is performed under conditions of A and B*.

Java GC options are the same. Setting several options does not enhance the speed of executing GC. Rather, it *may* make it slower. The **basic principle of GC tuning** is to **apply the different GC options to two or more servers and compare them**, and then add those options to the server for which the server has demonstrated enhanced performance or better GC time. Keep this in mind.

The following table shows options related to memory size among the GC options that can affect performance.

**Table 1: JVM Options to Be Checked for GC Tuning.**

 

| Classification | Option              | Description                          |
| -------------- | ------------------- | ------------------------------------ |
| Heap area size | `-Xms`              | Heap area size when starting JVM     |
|                | `-Xmx`              | Maximum heap area size               |
| New area size  | `-XX:NewRatio`      | Ratio of New area and Old area       |
|                | `-XX:NewSize`       | New area size                        |
|                | `-XX:SurvivorRatio` | Ratio of Eden area and Survivor area |

I frequently use `-Xms`, `-Xmx`, and `-XX:NewRatio` options for GC tuning.  `-Xms` and `-Xmx` option are particularly required. How you set the `NewRatio` option makes a significant difference on GC performance.

Some people ask **how to set the Perm area size**? You can set the Perm area size with the `-XX:PermSize` and `-XX:MaxPermSize` options but only when `OutOfMemoryError` occurs and the cause is the Perm area size.

Another option that may affect the GC performance is the [GC type](https://www.cubrid.org/blog/understanding-java-garbage-collection/). The following table shows available options by GC type (based on JDK 6.0).

**Table 2: Available Options by GC Type.**

| Classification         | Option                                                       | Remarks                                            |
| ---------------------- | ------------------------------------------------------------ | -------------------------------------------------- |
| Serial GC              | -XX:+UseSerialGC                                             |                                                    |
| Parallel GC            | -XX:+UseParallelGC -XX:ParallelGCThreads=value               |                                                    |
| Parallel Compacting GC | -XX:+UseParallelOldGC                                        |                                                    |
| CMS GC                 | -XX:+UseConcMarkSweepGC -XX:+UseParNewGC -XX:+CMSParallelRemarkEnabled -XX:CMSInitiatingOccupancyFraction=value -XX:+UseCMSInitiatingOccupancyOnly |                                                    |
| G1                     | -XX:+UnlockExperimentalVMOptions -XX:+UseG1GC                | In JDK 6, these two options must be used together. |

Except G1 GC, the GC type is changed by setting the option at the first line of each GC type. The most general GC type that does not intrude is Serial GC. It is optimized for client systems.

There are a lot of options that affect GC performance. But you can get significant effect by setting the options mentioned above. Remember that setting too many options does not promise enhanced GC execution time.

## Procedure of GC Tuning

The procedure of GC tuning is similar to the general performance improvement procedure. The following is the GC tuning procedure that I use.

### 1. Monitoring GC status

You need to monitor the GC status to check the GC status of the system in operation. Please see various GC monitoring methods in [How to Monitor Java Garbage Collection](https://www.cubrid.org/blog/how-to-monitor-java-garbage-collection/).

### 2. Deciding whether to tune GC after analyzing the monitoring result

After checking the GC status, you should analyze the monitoring result and decide whether to tune GC or not. If the analysis shows that the time taken to execute GC is just 0.1-0.3 seconds. you don't need to waste your time on tuning the GC. However, **if the GC execution time is 1-3 seconds, or more than 10 seconds, GC tuning is necessary**.

But, if you have allocated about 10GB Java memory and it is impossible to decrease the memory size, there is no way to tune GC. Before tuning GC, you need to think about why you need to allocate large memory size. If you have allocated the memory of 1 GB or 2 GB and `OutOfMemoryError` occurs, you should execute heap dump to verify and remove the cause.

> **Note:**
>
>  
>
> Heap dump is a file of the memory that is used to check the objects and data in the Java memory. This file can be created by using the **jmap** command included in the JDK. While creating the file, the Java process stops. Therefore, do not create this file while the system is operating.
>
>  
>
> Search on the Internet the detailed description on heap dump. For Korean readers, see my book I published last year: [The story of troubleshooting for Java developers and system operators](http://book.naver.com/bookdb/book_detail.nhn?bid=6654751) (Sangmin Lee, Hanbit Media, 2011, 416 pages).

### 3. Setting GC type/memory size

If you have decided on GC tuning, select the GC type and set the memory size. At this time, if you have several servers, it is important to check the difference of each GC option by setting different GC options for each server.

### 4. Analyzing results

Start analyzing the results after collecting data for at least 24 hours after setting GC options. If you are lucky, you will find the most suitable GC options for the system. If you are not, you should analyze the logs and check how the memory has been allocated. Then you need to find the optimum options for the system by changing the GC type/memory size.

### 5. If the result is satisfactory, apply the option to all servers and terminate GC tuning.

If the GC tuning result is satisfactory, apply the option to all the servers and terminate GC tuning.

In the following section, you will see the tasks to be done in each stage.

## Monitoring GC Status and Analyzing Results

The best way to check the GC status of the Web Application Server (WAS) in operation is to use the **jstat** command. I have explained the jstat command in [How To Monitor Java Garbage Collection](https://www.cubrid.org/blog/how-to-monitor-java-garbage-collection/), so I will describe the data to check in this article.

The following example shows a JVM for which GC tuning has not been done (however, it is not the operation server).

 

```
`$ jstat -gcutil 21719 1s``S0  S1  E  O  P  YGC  YGCT  FGC  FGCT GCT``48.66 0.00 48.10 49.70 77.45 3428 172.623 3 59.050 231.673``48.66 0.00 48.10 49.70 77.45 3428 172.623 3 59.050 231.673`
```

 

 

Here, check the values of YGC and YGCT. Divide YGCT by YGC. Then you get 0.050 seconds (50 ms). It means that it takes average 50 ms to execute GC in the Young area. With that result, you don't need to care about GC for the Young area.

And now, check the values of FGCT and FGC. Divide FGCT by FGC. Then you get 19.68 seconds. It means that it takes average 19.68 seconds to execute GC. It may take 19.68 seconds to execute GC three times. Otherwise, it takes 1 second to execute GC two times and 58 seconds for once. In both cases, GC tuning is required.

You can easily check GC status by using the **jstat** command; however, the best way to analyze GC is by generating logs with the `–verbosegc` option. For a detailed description on how to generate and tools to analyze logs, I have explained it the previous article. **HPJMeter** is my favorite among tools that are used to analyze the `-verbosegc` log. It is easy to use and analyze. With HPJmeter you can easily check the distribution of GC execution times and the frequency of GC occurrence.

If the GC execution time meets all of the following conditions, GC tuning is not required.

 

- Minor GC is processed quickly (within 50 ms).
- Minor GC is not frequently executed (about 10 seconds).
- Full GC is processed quickly (within 1 second).
- Full GC is not frequently executed (once per 10 minutes).

 

The values in parentheses are not the absolute values; they vary according to the service status. Some services may be satisfied with 0.9 seconds of Full GC processing speed, but some may not. Therefore, check the values and decide whether to execute GC tuning or not by considering each service.

There is one thing you should be careful of when you check the GC status; do not check the time of Minor GC and Full GC only. You must **check the number of GC executions**, as well. If the New area size is too small, Minor GC will be too frequently executed (sometimes once or more per 1 second). In addition, the number of objects passed to the Old area increases, causing increased Full GC executions. Therefore, apply the `–gccapacity` option in the stat command to check how much the area is occupied.

## Setting GC Type/Memory Size

### Setting GC Type

There are five GC types for Oracle JVM. However, if not JDK 7, one among Parallel GC, Parallel Compacting GC and CMS GC should be selected. There is no principle or rule to decide which one to select.

If so, **how can we select one?** The most recommended way is to apply all three. However, one thing is clear - CMS GC is faster than other Parallel GCs. At this time, if so, just apply CMS GC. However, CMS GC is not always faster. Generally, Full GC of CMS GC is fast, however, when concurrent mode failure occurs, it is slower than other Parallel GCs.

#### Concurrent mode failure

Let's take a deeper look into the concurrent mode failure.

The biggest difference between Parallel GC and CMS GC is the compaction task. The compaction task is to remove memory fragmentation by compacting memory in order to remove the empty space between allocated memory areas.

In the Parallel GC type, the compaction is executed whenever Full GC is executed, taking too much time. However, after executing Full GC, memory can be allocated in a faster way since the next memory can be allocated sequentially.

On the contrary, CMS GC does not accompany compaction. Therefore, the CMS GC is executed faster. However, when compaction is not executed, some empty spaces are generated in the memory as before executing Disk Defragmenter. Therefore, there may be no space for large objects. For example, 300 MB is left in the Old area, but some 10 MB objects cannot be sequentially saved in the area. In this case, "Concurrent mode failure" warning occurs and compaction is executed. However, if CMS GC is used, it takes a longer time to execute compaction than other Parallel GCs. And, it may cause another problem. For a more detailed description on concurrent mode failure, see [Understanding CMS GC Logs](https://blogs.oracle.com/poonam/entry/understanding_cms_gc_logs), written by Oracle engineers.

In conclusion, you should find the best GC type for your system.

Each system requires its proper GC type, so you need to find the best GC type for your system. If you are running six servers, I recommend you to set the same options for each of two servers, add the `-verbosegc` option, and then analyze the result.

### Setting Memory Size

The following shows the relationship between the memory size, the number of GC execution, and the GC execution time.

 

- Large memory size
  - decreases the number of GC executions.
  - increases the GC execution time.
- Small memory size
  - decreases the GC execution time.
  - increases the number of GC executions.

 

There is no "right" answer to set the memory size to small or large. 10 GB is OK if the server resource is good and Full GC can be completed within 1 second even when the memory has been set to 10 GB. But most servers are not in the status. When the memory is set to 10 GB, it takes about 10 ~ 30 seconds to execute Full GC. Of course, the time may vary according the object size.

If so, **how we should set the memory size?** Generally, I recommend 500 MB. But note that it does not mean that you should set the WAS memory with the `–Xms500m` and `–Xmx500m` options. Based on the current status before GC tuning, check the memory size left after Full GC. If there is about 300 MB left after Full GC, it is good to set the memory to 1 GB (300 MB (for default usage) + 500 MB (minimum for the Old area) + 200 MB (for free memory)). That means you should set the memory space with more than 500 MB for the Old area. Therefore, if you have three operation servers, set one server to 1 GB, one to 1.5 GB, and one to 2 GB, and then check the result.

Theoretically, GC will be done fast in the order of 1 GB > 1.5 GB > 2 GB, so 1 GB will be the fastest to execute GC. However, it cannot be guaranteed that it takes 1 second to execute Full GC with 1 GB and 2 seconds with 2 GB. The time depends on the server performance and the object size. Therefore, the best way to create the measurement data is to set as many as possible and monitor them.

You should set one more thing for setting the memory size: `NewRatio`. `NewRatio` is the ratio of the New area and the Old area. If `XX:NewRatio=1`, New area:Old area is 1:1. For 1 GB, New area:Old area is 500MB: 500MB. If `NewRatio` is 2, New area:Old area is 1:2. Therefore, as the value gets larger, the Old area size gets larger and the New area size gets smaller.

It may not be an important thing, but `NewRatio` value significantly affects the entire GC performance. If the New area size is small, much memory is passed to the Old area, causing frequent Full GC and taking a long time to handle it.

You may simply think that `NewRatio 1` would be the best; however, it may not be so. When `NewRatio` is set to 2 or 3, the entire GC status may be better. And I have seen such cases.

**What is the fastest way to complete GC tuning?** Comparing the results from performance tests is the fastest way to get the result. To set different options for each server and monitor the status, it is recommended to check the data after at least one or two days. However, you should prepare for giving the same load with the operation situation when you execute GC tuning through performance test. And the request ratio such as the URL that gives the load must be identical to that of the operation situation. However, giving accurate load is not easy for the professional performance tester and takes too long time for preparing. Therefore, it is more convenient and easier to apply the options to operation and wait for the result even though it takes a longer time.

## Analyzing GC Tuning Results

After applying the GC option and setting the `-verbosegc` option, check whether the logs are accumulated as desired with the `tail` command. If the option is not exactly set and no log is accumulated, you will waste your time. If logs are accumulated as desired, check the result after collecting data for one or two days. The easiest way is to move logs to the local PC and analyze the data by using **HPJMeter**.

In the analysis, focus on the following. The priority is determined by me. The most important item to decide the GC option is Full GC execution time.

 

- Full GC execution time
- Minor GC execution time
- Full GC execution interval
- Minor GC execution interval
- Entire Full GC execution time
- Entire Minor GC execution time
- Entire GC execution time
- Full GC execution times
- Minor GC execution timesl

 

It is a very lucky case to find the most appropriate GC option, and in most cases, it's not. Be careful when executing GC tuning because `OutOfMemoryError` may occur if you try to complete GC tuning all at once.

## Examples of Tuning

So far, we have theoretically discussed GC tuning without any examples. Now we will take a look at the examples of GC tuning.

### Example 1

The following example is GC tuning for **Service S**. For the newly developed Service S, it took too much time to execute Full GC.

See the result of `jstat –gcutil`.

 

```bsh
S0 S1 E O P YGC YGCT FGC FGCT GCT12.16 0.00 5.18 63.78 20.32 54 2.047 5 6.946 8.993
```

 

 

Information to the left **Perm** area is not important for the initial GC tuning. At this time, the values from the right YGC are important.

The average value taken to execute Minor GC and Full GC once is calculated as below.

**Table 3: Average Time Taken to Execute Minor GC and Full GC for Service S.**

| GC Type  | GC Execution Times | GC Execution Time | Average  |
| -------- | ------------------ | ----------------- | -------- |
| Minor GC | 54                 | 2.047             | 37 s     |
| Full GC  | 5                  | 6.946             | 1,389 ms |

**37 ms** is not bad for Minor GC. However, **1.389 seconds** for Full GC means that timeout may frequently occur when GC occurs in the system of which DB Timeout is set to 1 second. In this case, the system requires GC tuning.

First, you should check how the memory is used before starting GC tuning. Use the `jstat –gccapacity` option to check the memory usage. The result checked from this server is as follows.

 

```bsh
NGCMN NGCMX NGC S0C S1C EC OGCMN OGCMX OGC OC PGCMN PGCMX PGC PC YGC FGC212992.0 212992.0 212992.0 21248.0 21248.0 170496.0 1884160.0 1884160.0 1884160.0 1884160.0 262144.0 262144.0 262144.0 262144.0 54 5
```

 

 

The key values are as follows.

 

- New area usage size: 212,992 KB
- Old area usage size: 1,884,160 KB

 

Therefore, the totally allocated memory size is 2 GB, excluding the Perm area, and New area:Old area is 1:9. To check the status in a more detailed way than **jstat**, the `-verbosegc` log has been added and three options were set for the three instances as shown below. No other option has been added.

- NewRatio=2
- NewRatio=3
- NewRatio=4

 

After one day, the GC log of the system has been checked. Fortunately, no Full GC has occurred in this system after `NewRatio` has been set.

**Why?** The reason is that most of the objects created from the system are destroyed soon, so the objects are not passed to the Old area but destroyed in the New area.

In this status, it is not necessary to change other options. Just select the best value for `NewRatio`. So, **how can we determine the best value?** To get it, analyze the average response time of Minor GC for each `NewRatio`.

The average response time of Minor GC for each option is as follows:

- NewRatio=2: 45 ms
- NewRatio=3: 34 ms
- NewRatio=4: 30 ms

 

We have concluded that NewRatio=4 is the best option since the GC time is the shortest even though the New area size is the smallest. After applying the GC option, the server has no Full GC.

For your information, the following is the result of executing `jstat –gcutil` some days after the JVM of the service had started.

 

```bsh
S0 S1 E O P YGC YGCT FGC FGCT GCT8.61 0.00 30.67 24.62 22.38 2424 30.219 0 0.000 30.219
```

 

 

You many think that GC has not frequently occurred since the server has few requests. However, Full GC has not been executed while Minor GC has been executed 2,424 times.

### Example 2

This example is for Service A. We found that the JVM had not operated for a long time (8 seconds or more) periodically in the Application Performance Manager (APM) in the company. So we executed GC tuning. We were searching for the reason and found that it took a long time to execute Full GC, so we decided to execute GC tuning.

As the starting stage of GC tuning, we added the `-verbosegc` option and the result is as follows.

![duration_graph_before_gc_tuning.jpeg](https://www.cubrid.org/files/attach/images/1730/731/001/f49ae67f710a1fe19690ed17ff377527.jpeg)

**Figure 1: Duration Graph before GC Tuning.**

The above graph, which shows the duration, is one of the graphs that the HPJMeter automatically provides after analysis. The **X-axis** shows the time after the JVM has started and the **Y-axis** shows the response time of each GC. The green dots, the CMS, indicates the Full GC result, and the blue bots, Parallel Scavenge, indicates the Minor GC result.

Previous I said that CMS GC would be the fastest. But the above result show that there were some cases which took up to 15 seconds. **What has caused such result?** Please remember what I said before: CMS gets slower when compaction is executed. In addition, the memory of the service has been set by using `–Xms1g` and `–Xmx4g` and the memory allocated was 4 GB.

So I changed the GC type from CMS GC to Parallel GC. I changed the memory size to 2 GB and then set the `NewRatio` to 3. The result of `jstat –gcutil` after a few hours is as follows.

 

```bsh
S0 S1 E O P YGC YGCT FGC FGCT GCT0.00 30.48 3.31 26.54 37.01 226 11.131 4 11.758 22.890
```

 

 

The Full GC time was faster, 3 seconds per one time, compared to 15 seconds for 4 GB. However, 3 seconds is still not so fast. So I created six cases as follows.

- Case 1: `-XX:+UseParallelGC -Xms1536m -Xmx1536m -XX:NewRatio=2`
- Case 2: `-XX:+UseParallelGC -Xms1536m -Xmx1536m -XX:NewRatio=3`
- Case 3: `-XX:+UseParallelGC -Xms1g -Xmx1g -XX:NewRatio=3`
- Case 4: `-XX:+UseParallelOldGC -Xms1536m -Xmx1536m -XX:NewRatio=2`
- Case 5: `-XX:+UseParallelOldGC -Xms1536m -Xmx1536m -XX:NewRatio=3`
- Case 6: `-XX:+UseParallelOldGC -Xms1g -Xmx1g -XX:NewRatio=3`

**Which one would be the fastest?** The result showed that the smaller the memory size was, the better the result was. The following figure shows the duration graph of Case 6, which showed the highest GC improvement. The slowest response time was 1.7 seconds and the average had been changed to within 1 second, showing the improved result.

![duration_graph_after_applying_case_6.png](https://www.cubrid.org/files/attach/images/1730/731/001/117f4cc137a4b953815933dcd51f481b.png)

**Figure 2: Duration Graph after Applying Case 6.**

With the result, I changed all GC options of the service to Case 6. However, this change causes `OutOfMemoryError` at night each day. It is difficult to detail the reason here, but in short, batch data processing made a lack of JVM memory. The related problems are being cleared now.

It is very dangerous to analyze the GC logs accumulated for a short time and to apply the result to all servers as executing GC tuning. Keep in mind that GC tuning can be executed without failure **only** when you analyze the service operation as well as the GC logs.

We have reviewed two GC tuning examples to see how GC tuning is executed. As I mentioned, the GC option set in the examples can be identically set for the server which has the same CPU, OS version and JDK version with the service that executes the same functions. However, do not apply the option I did to your services in operation, since they may not work for you.

## Conclusion

I execute GC tuning based on my experiences without executing heap dump and analyzing the memory in detail. Precise memory status analysis may draw the better GC tuning results. However, that kind of analysis may be helpful when the memory is used in the constant and routine pattern. But, if the service is heavily used and there are a lot of memory usage patterns, GC tuning based on reliable previous experience may be recommendable.

I have executed the performance test by setting the G1 GC option to some servers, but have not applied to any operation server yet. The G1 GC option shows a faster result than any other GC types. However, it requires to upgrade to JDK 7. In addition, stability is still not guaranteed. Nobody knows if there is any critical bug or not. So the time is not yet ripe for applying the option.

After JDK 7 is stabilized (this does not mean that it is not stable) and WAS is optimized for JDK 7, enabling stable application of G1 GC *may* finally work as expected and some day we *may* not need the GC tuning.

For more detail on GC tuning, search on [Slideshare.com](http://www.slideshare.net/) for related materials. The most recommendable material is [Everything I Ever Learned About JVM Performance Tuning @Twitter](http://www.slideshare.net/aszegedi/everything-i-ever-learned-about-jvm-performance-tuning-twitter), written by Attila Szegedi, a Twitter engineer. Please take the time to read it.