---
layout: post
title: 阅读摘记
date:  2018-03-20 09:49:00 +0900
description: 阅读摘记
img: post-9.jpg # Add image post (optional)
tags: [Android]
author: # Add name author (optional)

#androidoriginal: true
needcomplete: true
---
**Kotlin和java的区别**

1：kotlin语法更简洁

2：定义变量不用写具体类型，不用输入分号作为结束符

3：会默认进行空值校验

4：可以编译成java字节码、javascript字节码、本地字节码

5：支持其它特性，如携程，扩展函数

**APP性能优化**

1：内存优化，通过可视化工具如android studio自带的工具进行内存分析，dump下来分析引用链，查找可能内存泄漏的地方

2：代码优化，代码审查，涉及数据或者算法的地方，优化算法降低时间空间复杂度，避免数据频繁装箱拆箱

3：布局优化，打开开发者选项里的绘制调试开关，看是否过度绘制

4：绘制优化，绘制ondraw函数调用比较频繁，避免做过多事情

5：控件优化，如recycleview，通过viewholder复用item，图片加载优化

6：大图优化，设置bitmapfactory。option选项injustdecodebounder为true，图片不加载入内存获取图片宽高，再设置insamplesize进行图片缩放，在设置injustdecodebounder为false加载图片

7：其它网络优化，电量优化

**APP内存泄漏**

1：内存泄漏主要有两种，第一种是指向内存块的引用对象访问不到，却一直指向内存块，导致内存无法回收（如内对象释放，其里面某成员变量指向某内存块，这个成员变量被静态对象引用着，导致无法释放，当对象被销毁时，指向该内存块的引用也无法使用了）。第二种，内存对象比较多，大部分对象生命周期比真正使用到的时间长太多

2：单例造成内存泄漏，单例持有类的上下文

3：非静态内部类默认持有外部类对象，当非静态内部类对象为静态时，当界面退出时，因静态类持有外部类引用，导致外部类无法释放，占用内存无法回收

4：其它如线程，handler，广播，rxjava等

**APP大小压缩**

1：启动代码混淆，可进行一部分压缩

2：图片使用两套图 xhdpi xxhdpi

3：图片看是否可以进一步压缩

4：Google Bundle打包方式

**APP屏幕适配**

1：通过工具生成360dp，480dp...多套适配屏幕的值

2：通过代码计算，如一屏横向放3张图，可通过代码计算设置

3：

**APP屏幕密度**

density=120时 屏幕实际分辨率为240px*400px （两个点对应一个像素）

density=160时 屏幕实际分辨率为320px*533px （三个点对应两个像素）

density=240时 屏幕实际分辨率为480px*800px （一个点对于一个像素）

apk的资源包中，当屏幕density=240时使用hdpi标签的资源

当屏幕density=160时，使用mdpi标签的资源

当屏幕density=120时，使用ldpi标签的资源。

DP=(density / 160)PX

https://blog.csdn.net/xxdw1992/article/details/81975073

https://blog.csdn.net/qq84549557/article/details/78880830


**JVM和DVM和ART区别**

**JVM内存管理机制**

**强引用，软引用，弱引用，虚引用**

**threadlocal作用**

主要多个线程使用同一个变量时，会互相造成干扰，那么通过这个类，会给每个类复制一份变量的副本，副本隔离，互不干扰。



**Android内存管理机制**

**synchronized理解**

**volatile理解**

**线程池创建方式**

**HaseMap理解**

**常用排序算法原理及实际复杂度**

**常用数据结构**

**Android系统架构**

**Android自定义VIEW**

**Android Filter**

**Android Handler机制**

**Android Binder机制**

**Android AMS WMS工作原理**

**Android Activity启动过程**
