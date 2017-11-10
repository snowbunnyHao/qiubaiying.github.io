---
layout:     post
title:      clock()函数在多线程环境下不要使用
subtitle:   CPU时间 挂钟时间 以及各个函数的应用
date:       2017-11-10
author:     snowbunny
header-img: img/bg_1.jpg
catalog: true
tags:
    - OS
    - Linux
---


# 前言

前几天在做OS的一个课程project，题目比较简单，就是实现多线程环境下两个4096x4096的矩阵相乘。
我是在Linux下用pthread实现的，使用c库里的clock()函数计时。

# 经过

结果我就发现，计算出来的时间往往比真实的时间多很多。
于是我就去查了clock()的资料，发现这样说：
This function returns the calling process’ current CPU time. If the CPU time is not available or cannot be represented, clock returns the value (clock_t)(-1).

其中提到了CPU Time，在Wikipedia上这么定义：
CPU time (or process time) is the amount of time for which a central processing unit (CPU) was used for processing instructions of a computer program or operating system
然后顺便查了一下另外一个概念：
Wall-clock time is the time that a clock on the wall (or a stopwatch in hand) would measure as having elapsed between the start of the process and 'now'.因此挂钟时间应该是指的真实经过的时间，包括其他进程使用的时间片（time slice）和本进程耗费在阻塞（如等待I/O操作完成）上的时间。


# 经验
clock()计算了所有CPU上的时钟滴答数，因此在多个线程并发时计算就会出现问题。
所以在多线程环境下还是不要使用clock()函数，可以使用gettimeofday()等函数



