---
title: "Java自带的一些并发工具类"
date: 2023-06-03T14:49:26+08:00
draft: true
toc: false
images:
tags:
  - java
---



Java提供了许多有用的多线程工具类，主要有

1. CountDownLatch: 计数器，主要用于线程之间的同步
2. CyclicBarrier: 屏障， 用于多个线程互相等待
3. Semaphore: 信号量，用户控制线程数量，实现资源池的功能
4. Executor 和 ExecutorService: 线程池，用于线程生命周期管理
5. Future和FutureTask: 用于future调用的返回结果
6. ReentrantLock 和 Condition: 用于线程同步，替代synchronized
7. Atomic 包下的类：用于线程安全的原子操作
8. ThreadLocal: 用于线程独有的变量副本
9. ForkJoinPool: 用于并行计算，分而治之
