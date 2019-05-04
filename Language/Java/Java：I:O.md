# How Java I/O Works Internally at Lower Level?

[How Java I/O Works Internally at Lower Level?](<https://howtodoinjava.com/java/io/how-java-io-works-internally-at-lower-level/>)

## Buffer Handling and Kernel vs User Space

Buffers, and how buffers are handled, are the basis of all I/O. **The very term “input/output” means nothing more than moving data in and out of buffers.** Just keep this in your mind all the time. Usually, processes perform I/O by requesting of the operating system that data to be drained from a buffer (*write operation*) or that a buffer be filled with data (*read operation*). That’s whole summary of I/O concepts. The machinery inside the operating system that performs these transfers can be incredibly complex, but conceptually, it’s very straightforward and we are going to discuss a small part of it in this post.

![data buffering at os level](https://cdn1.howtodoinjava.com/wp-content/uploads/2014/12/data-buffering-at-os-level.png)

The image above shows a simplified ‘logical’ diagram of how block data moves from an external source, such as a hard disk, to a memory area inside a running process (e.g. RAM). First of all, the process requests that its buffer be filled by making the `read()` system call. This call results in the kernel issuing a command to the disk controller hardware to fetch the data from disk. The disk controller writes the data directly into a kernel memory buffer by **DMA(Direct Memery Access)** without further assistance from the main CPU. Once the disk controller finishes filling the buffer, the kernel copies the data from the temporary buffer in kernel space to the buffer specified by the process; when it requested the `read()` operation.

One thing to notice is that the kernel tries to cache and/or prefetch data, so the data being requested by the process may already be available in kernel space. If so, the data requested by the process is copied out. If the data isn’t available, the process is suspended while the kernel goes about bringing the data into memory.

## Virtual Memory

You already must have heard of virtual memory multiple times already. Let me put some thoughts on it.

All modern operating systems make use of virtual memory. Virtual memory means that artificial, or virtual, addresses are used in place of physical (hardware RAM) memory addresses. Virtual memory brings two important advantages:

1) More than one virtual address can refer to the same physical memory location.
2) A virtual memory space can be larger than the actual hardware memory available.

In above section, copying from kernel space to the final user buffer must seem like extra work. Why not tell the disk controller to send it directly to the buffer in user space? Well, it is done using virtual memory and it’s advantage number 1 above.

By mapping a kernel space address to the same physical address as a virtual address in user space, the DMA hardware (which can access only physical memory addresses) can fill a buffer that is simultaneously visible to both the kernel and a user space process.

![virtual memory](https://cdn2.howtodoinjava.com/wp-content/uploads/2014/12/virtual-memory-architechture.png)

This eliminates copies between kernel and user space, but requires the kernel and user buffers to share the same page alignment. Buffers must also be a multiple of the block size used by the disk controller (usually 512 byte disk sectors). Operating systems divide their memory address spaces into pages, which are fixed-size groups of bytes. These memory pages are always multiples of the disk block size and are usually powers of 2 (which simplifies addressing). Typical memory page sizes are 1,024, 2,048, and 4,096 bytes. The virtual and physical memory page sizes are always the same.

# Java Standard IO vs. Java NIO

## **Identifying differences between IO and NIO**

##### **1) IO streams versus NIO blocks**

The most important distinction between the original I/O library (found in java.io.*) and NIO has to do with how data is packaged and transmitted. As previously mentioned, original I/O deals with data in streams, whereas NIO deals with data in blocks.

A stream-oriented I/O system deals with data one or more bytes at a time. An input stream produces one byte of data, and an output stream consumes one byte of data. It is very easy to create filters for streamed data. It is also relatively simply to chain several filters together so that each one does its part in what amounts to a single, sophisticated processing mechanism. Important thing is that bytes are not cached anywhere. Furthermore, you cannot move forth and back in the data in a stream. If you need to move forth and back in the data read from a stream, you must cache it in a buffer first.

A block-oriented I/O system deals with data in blocks. Each operation produces or consumes a block of data in one step. Processing data by the block can be much faster than processing it by the (streamed) byte. You can move forth and back in the buffer as you need to. This gives you a bit more flexibility during processing. However, you also need to check if the buffer contains all the data you need in order to fully process it. And, you need to make sure that when reading more data into the buffer, you do not overwrite data in the buffer you have not yet processed. But block-oriented I/O lacks some of the elegance and simplicity of stream-oriented I/O.

**Read more:** [**3 ways to read files using Java NIO**](https://howtodoinjava.com/java-7/nio/3-ways-to-read-files-using-java-nio/)

##### **2) Synchronous vs. Asynchronous IO**

Java IO’s various streams are blocking or synchronous. That means, that when a thread invokes a read() or write(), that thread is blocked until there is some data to read, or the data is fully written. The thread will be in blocked state for this period. This has been cited as a good solid reason for bringing multi-threading in modern languages.

In asynchronous IO, a thread can request that some data be written to a channel, but not wait for it to be fully written. The thread can then go on and do something else in the mean time. Usually these threads spend their idle time on when not blocked in IO calls, is usually performing IO on other channels in the meantime. That is, a single thread can now manage multiple channels of input and output.

Synchronous programs often have to resort to polling, or to the creation of many, many threads, to deal with lots of connections. With asynchronous I/O, you can listen for I/O events on an arbitrary number of channels, without polling and without extra threads.

The central object in asynchronous I/O is called the Selector. A Selector is where you register your interest in various I/O events, and it is the object that tells you when those events occur. So, the first thing we need to do is create a Selector:

```
`Selector selector = Selector.open();`
```

Later on, we will call the register() method on various channel objects, in order to register our interest in I/O events happening inside those objects. The first argument to register() is always the Selector.

**Read more: ** https://howtodoinjava.com/java-7/nio/how-to-define-path-in-java-nio/

## **Summary**

NIO allows you to manage multiple channels using only a single (or fewer) threads, but the cost is that parsing the data might be somewhat more complicated than when reading data from a blocking stream using standard IO.

If you need to manage thousands of open connections simultaneously, which each only send a little data, for instance a chat server, implementing the server in NIO is probably an advantage. Similarly, if you need to keep a lot of open connections to other computers, e.g. in a P2P network, using a single thread to manage all of your outbound connections might be an advantage.

If you have fewer connections with very high bandwidth, sending a lot of data at a time, standard IO server implementation should be your choice.



# Write to File

## Write File using BufferedWritter

t is advisable to wrap a `BufferedWriter` around any `Writer` whose `write()` operations may be costly, such as `FileWriter` and `OutputStreamWriter`.

As it buffers before writing, so it result in **less IO operations**, so it improve the performance.

```java
public static void usingBufferedWritter() throws IOException{
    String fileContent = "Hello Learner !! Welcome to howtodoinjava.com.";
     
    BufferedWriter writer = new BufferedWriter(new FileWriter("c:/temp/samplefile1.txt"));
    writer.write(fileContent);
    writer.close();
}
```



## Write File using FileWriter/PrintWriter

`FileWriter` the most clean way to write files. Syntax is self explanatory and easy to read and understand. `FileWriter` writes directly into file (***less performance***) and should be used only when number of writes are less.

```java
`public` `static` `void` `usingFileWriter() ``throws` `IOException``{``    ``String fileContent = ``"Hello Learner !! Welcome to howtodoinjava.com."``;``    ` `    ``FileWriter fileWriter = ``new` `FileWriter(``"c:/temp/samplefile2.txt"``);``    ``fileWriter.write(fileContent);``    ``fileWriter.close();``}`
```

## Write File using FileOutputStream

Use `FileOutputStream` to **write binary data to a file**. `FileOutputStream` is meant for writing streams of raw bytes such as image data. For writing streams of characters, consider using `FileWriter`.

```java
public static void usingFileOutputStream() throws IOException{
    String fileContent = "Hello Learner !! Welcome to howtodoinjava.com.";
     
    FileOutputStream outputStream = new FileOutputStream("c:/temp/samplefile4.txt");
    byte[] strToBytes = fileContent.getBytes();
    outputStream.write(strToBytes);
  
    outputStream.close();
}
```

## Write File using DataOutputStream

`DataOutputStream` lets an application **write primitive Java data types** to an output stream in a portable way. An application can then use a data input stream to read the data back in.

```java
public static void usingDataOutputStream() throws IOException{
    String fileContent = "Hello Learner !! Welcome to howtodoinjava.com.";
     
    FileOutputStream outputStream = new FileOutputStream("c:/temp/samplefile5.txt");
    DataOutputStream dataOutStream = new DataOutputStream(new BufferedOutputStream(outputStream));
    dataOutStream.writeUTF(fileContent);
  
    dataOutStream.close();
}
```

## Write File using FileChannel

`FileChannel` can be used for reading, writing, mapping, and manipulating a file. If you are dealing with large files, `FileChannel` can be faster than standard IO.

File channels are safe for use by multiple concurrent threads.

```java
public static void usingFileChannel() throws IOException{
    String fileContent = "Hello Learner !! Welcome to howtodoinjava.com.";
     
    RandomAccessFile stream = new RandomAccessFile("c:/temp/samplefile6.txt", "rw");
    FileChannel channel = stream.getChannel();
    byte[] strBytes = fileContent.getBytes();
    ByteBuffer buffer = ByteBuffer.allocate(strBytes.length);
    buffer.put(strBytes);
    buffer.flip();
    channel.write(buffer);
    stream.close();
    channel.close();
}
```

## Summary

1. If we try to write to a file that doesn’t exist, the file will be created first and no exception will be thrown (except using `Path` method).
2. Always close the output stream after writing the file content to release all resources. It will also help in not corrupting the file.
3. Use `PrintWriter` is used to write formatted text.
4. Use `FileOutputStream` to write binary data.
5. Use `DataOutputStream` to write primitive data types.
6. Use `FileChannel` to write larger files.



# Read File

use `Files.readAllBytes()`, `Files.lines()` (to **read line by line**) and `FileReader` & `BufferedReader` to **read text file to String**.









