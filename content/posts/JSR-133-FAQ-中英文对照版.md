---
title: "JSR 133 FAQ 中英文对照版"
date: 2022-04-24T00:03:01+08:00
draft: true
---

### What is a memory model, anyway?
In multiprocessor systems, processors generally have one or more layers of memory cache, which improves performance both by speeding access to data (because the data is closer to the processor) and reducing traffic on the shared memory bus (because many memory operations can be satisfied by local caches.) Memory caches can improve performance tremendously, but they present a host of new challenges. What, for example, happens when two processors examine the same memory location at the same time? Under what conditions will they see the same value?

在多处理器系统中，处理器通常会有一或更多层缓存用以提高访问数据的速度(因缓存更靠近处理器)及减少共享内存总线上的通信量
(在本地缓存中已能执行很多操作)，从而提高性能。缓存能够显著地提高性能的同时带来了许多新的挑战。例如当两个处理器同时检查内存的同一块地址时会发生
什么，什么条件下这两个处理器得到相同的值？

At the processor level, a memory model defines necessary and sufficient conditions for knowing that writes to memory by other processors are visible to the current processor, and writes by the current processor are visible to other processors. Some processors exhibit a strong memory model, where all processors see exactly the same value for any given memory location at all times. Other processors exhibit a weaker memory model, where special instructions, called memory barriers, are required to flush or invalidate the local processor cache in order to see writes made by other processors or make writes by this processor visible to others. These memory barriers are usually performed when lock and unlock actions are taken; they are invisible to programmers in a high level language.

内存模型在处理器级别定义了其它处理器的写操作对当前处理器是否可见的必要和充分条件。一些处理器展现出强大的内存模型，使得所有处理器都能
在任意给定时刻在任意的内存位置读取完全相同的值。其它处理器表现出更弱的内存模型，它们需要通过称为内存屏障的特别指令来刷新缓存，或者是
使得当前的缓存无效，这样才能看见由其它处理器对数据做出的最新修改或是使得当前处理器的修改能被其它处理器所见。这些内存屏障指令通常在
加锁或者解锁时执行，并且对高级语言的使用者是透明的。

It can sometimes be easier to write programs for strong memory models, because of the reduced need for memory barriers. However, even on some of the strongest memory models, memory barriers are often necessary; quite frequently their placement is counterintuitive. Recent trends in processor design have encouraged weaker memory models, because the relaxations they make for cache consistency allow for greater scalability across multiple processors and larger amounts of memory.

在为强大的内存模型编程时，因为内存屏障使用需求减少，所以有时编程会更加容易。但即便如此，在最强大的内存模型中编程时也经常必须使用内存屏障，
而且它们的使用频繁地反直觉。近来处理器设计的趋势是鼓励使用较弱的内存模型，因为较弱的内存模型对缓存一致性的要求更少，所以能带来更好的扩展性，
适配更大的内存。

The issue of when a write becomes visible to another thread is compounded by the compiler's reordering of code. For example, the compiler might decide that it is more efficient to move a write operation later in the program; as long as this code motion does not change the program's semantics, it is free to do so.  If a compiler defers an operation, another thread will not see it until it is performed; this mirrors the effect of caching.

Moreover, writes to memory can be moved earlier in a program; in this case, other threads might see a write before it actually "occurs" in the program.  All of this flexibility is by design -- by giving the compiler, runtime, or hardware the flexibility to execute operations in the optimal order, within the bounds of the memory model, we can achieve higher performance.

A simple example of this can be seen in the following code:
```
Class Reordering {
  int x = 0, y = 0;
  public void writer() {
    x = 1;
    y = 2;
  }

  public void reader() {
    int r1 = y;
    int r2 = x;
  }
}
```

Let's say that this code is executed in two threads concurrently, and the read of y sees the value 2. Because this write came after the write to x, the programmer might assume that the read of x must see the value 1. However, the writes may have been reordered. If this takes place, then the write to y could happen, the reads of both variables could follow, and then the write to x could take place. The result would be that r1 has the value 2, but r2 has the value 0.

假设 reader 和 writer 在两个线程中并发执行，而 reader 在读 y 时看到了值 2，因为 y 的写操作在(代码上的) x 的写操作之后，程序员可能会假设
对 x 的读取必定会看到值 1. 但是这些写操作也许被重排序了。

The Java Memory Model describes what behaviors are legal in multithreaded code, and how threads may interact through memory. It describes the relationship between variables in a program and the low-level details of storing and retrieving them to and from memory or registers in a real computer system. It does this in a way that can be implemented correctly using a wide variety of hardware and a wide variety of compiler optimizations.

Java includes several language constructs, including volatile, final, and synchronized, which are intended to help the programmer describe a program's concurrency requirements to the compiler. The Java Memory Model defines the behavior of volatile and synchronized, and, more importantly, ensures that a correctly synchronized Java program runs correctly on all processor architectures.

### Do other languages, like C++, have a memory model?
Most other programming languages, such as C and C++, were not designed with direct support for multithreading. The protections that these languages offer against the kinds of reorderings that take place in compilers and architectures are heavily dependent on the guarantees provided by the threading libraries used (such as pthreads), the compiler used, and the platform on which the code is run.

### What is JSR 133 about?

### What is meant by reordering?


### What was wrong with the old memory model?

### What do you mean by “incorrectly synchronized”?

### What does synchronization do?
