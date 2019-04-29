---
title: "程序，进程，线程"
toc: true
toc_sticky: true
toc_label: "目录"
categories:
  - Operating System
tags:
  - Operating System
  - Program
  - Process
  - Thread
---



## 程序（Program）

程序是一个被计算机执行的指令集合，用于执行某个特定的任务。

众所周知，程序一般是由程序员用某种特定的编程语言编写的，这些编程语言往往是高级语言，但是计算机其实是不懂高级语言的，它只认识一些特定的机器码。

### 一个程序是如何被翻译和启动的？

一个程序从高级代码到被执行阶段的流程如下图：

![](/assets/images/program-process-thread/15564346843425.jpg)

* **高级语言**：指的是程序员编写的代码，例如 Objective-C，Java，C++ 等。
* **汇编语言**：汇编语言是一种低级语言，在不同的设备中，汇编语言对应着不同的机器语言指令集。
* **目标文件**：目标文件是指存放目标代码的计算机文件，目标代码一般是机器代码或者接近机器语言的代码。
* **可执行文件**：可以由操作系统加载执行的文件，例如 .ipa，.apk，.exe 等。

上面的流程可以简单概括为：高级语言经过编译器，被编译器编译成汇编语言；然后汇编语言再由汇编器汇编成目标文件；连接器再将所有目标文件链接成一个可执行文件；最后再由操作系统通过加载器，将可执行文件加载到内存中，同时创建一个进程来跑这个程序。

## 进程

进程是程序的实例，它是操作系统创建的用来执行程序的单元。

一个进程的结构如下图：

![](/assets/images/program-process-thread/15564410722393.jpg)

其中寄存器（Register）是 CPU 的一部分，它包含一些进程需要的指令，地址及数据；程序计数器（Program Counter）是用来记录和追踪计算机在程序序列中的位置；栈和堆是操作系统分配给进程的，其中栈的作用是保存活动的子程序的信息；进程还拥有一部分内存来存放程序的代码。

每个进程都拥有独立的内存空间，所以进程之间是相互独立的。进程之间无法直接共享数据，从一个进程切换到另外一个进程需要消耗一定的资源，例如保存和加载寄存器的时间，内存映射等。

## 线程

线程是进程里的执行单元，一个进程可以拥有一条或多条线程。

单线程和多线程的结构如下图：

![](/assets/images/program-process-thread/15564468564031.jpg)

从图中可以看出，每条线程都有自己单独的栈，但是每条线程都共享着同一个堆。线程和进程都是共享着同一块内存空间。

## 线程 vs. 进程

* 进程之间是相互独立的，线程相当于进程的子集。
* 进程需要比线程维护更多的状态信息，同一进程里的多条线程能共享进程的状态、内存和资源。
* 进程之间有相互独立的地址空间，线程和进程共享同一块地址空间。
* 进程间需要通信必须依赖于系统提供的进程间通讯机制。
* 线程间的上下文切换比进程间的上下文切换快。

## 参考

* [Computer program](https://en.wikipedia.org/wiki/Computer_program)
* [Process (computing)](https://en.wikipedia.org/wiki/Process_(computing))
* [Thread (computing)](https://en.wikipedia.org/wiki/Thread_(computing))
* [What’s the Diff: Programs, Processes, and Threads](https://www.backblaze.com/blog/whats-the-diff-programs-processes-and-threads/)