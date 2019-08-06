---
layout: post
title: Tencent
date:  2019-08-06 18:08:00 +0900
description: Tencent
img: post-3.jpg # Add image post (optional)
tags: [原理]
author: # Add name author (optional)
theory: true
---


**热修复**




**增量更新**

1: 使用bsdiff生成差异包PATCH.patch

2: 在手机上合并base包和差异包，生成新版本的安装包

3： 安装新的安装包


**volatile关键字和synchronized的区别**

它所修饰的变量不保留拷贝，直接访问主内存中的。

在Java内存模型中，有main memory，每个线程也有自己的memory (例如寄存器)。为了性能，一个线程会在自己的memory中保持要访问的变量的副本。这样就会出现同一个变 量在某个瞬间，在一个线程的memory中的值可能与另外一个线程memory中的值，或者main memory中的值不一致的情况。 一个变量声明为volatile，就意味着这个变量是随时会被其他线程修改的，因此不能将它cache在线程memory中

保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的

禁止进行指令重排序。<br>
volatile 本质是在告诉 jvm 当前变量在寄存器（工作内存）中的值是不确定的，需要从主存中读取


synchronized修饰一个方法或者代码块，表示同一时刻只能有一个线程访问，其它线程访问到时处于阻塞状态

**synchronized原理**

https://www.jianshu.com/p/e62fa839aa41
https://www.cnblogs.com/mingyao123/p/7424911.html


加上synchronized同步代码块，会在代码块头部加上monitorenter，代码块尾部加上monitorexit，每个对象都有monitor对象与之关联，如果monitor进入数为0，则有线程进入时，则计数会加一，线程会获取monitor的所有权，其它线程进入时则要处于等待状态

同步代码块：monitorenter指令插入到同步代码块的开始位置，monitorexit指令插入到同步代码块的结束位置，JVM需要保证每一个monitorenter都有一个monitorexit与之相对应。任何对象都有一个monitor与之相关联，当且一个monitor被持有之后，他将处于锁定状态。线程执行到monitorenter指令时，将会尝试获取对象所对应的monitor所有权，即尝试获取对象的锁

在JVM中，对象在内存中的布局分为三块区域：对象头、实例变量和填充数据。

1： monitorenter：每个对象都是一个监视器锁（monitor）。当monitor被占用时就会处于锁定状态，线程执行monitorenter指令时尝试获取monitor的所有权，过程如下：

如果monitor的进入数为0，则该线程进入monitor，然后将进入数设置为1，该线程即为monitor的所有者；

如果线程已经占有该monitor，只是重新进入，则进入monitor的进入数加1；

如果其他线程已经占用了monitor，则该线程进入阻塞状态，直到monitor的进入数为0，再重新尝试获取monitor的所有权；

2：monitorexit：执行monitorexit的线程必须是objectref所对应的monitor的所有者。指令执行时，monitor的进入数减1，如果减1后进入数为0，那线程退出monitor，不再是这个monitor的所有者。其他被这个monitor阻塞的线程可以尝试去获取这个 monitor 的所有权。

monitorexit指令出现了两次，第1次为同步正常退出释放锁；第2次为发生异步退出释放锁；


**什么是内存可见性**

一个线程修改了共享变量的值，其它线程也能看到最新修改的值
