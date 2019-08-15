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

**APP布局优化**

hierarchy viewer，Lint tool 布局调优工具

1：使用include标签减少复用布局而产生的布局嵌套。提取布局间的公共部分，通过提高布局的复用性从而减少测量 & 绘制时间

2：merge用于辅助include标签，可将include布局里面顶层布局替换掉，减少布局层级

3：ViewStub在需要的时候加载显示

4：首先使用相对布局， 一般情况下用LinearLayout的时候总会比RelativeLayout多一个View的层级

5：嵌套LinearLayout中尽量不要使用weight，因为weight会重新测量两次

6：View视图的显示隐藏，尽量使用invisible，因为gone,不占用空间，视图会重新测量绘制；而invisible视图不会重新绘制，但仍然占用空间位置

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

1：JVM是java虚拟机，基于栈，解析的是class字节码

2：DVM是Android虚拟机，基于寄存器，其需要多一步将class字节码转换成dex，允许里面运行多个进程，其存取速度比栈快的多，栈要更多的指令分派和内存访问次数。

3：Android 5.0后ART取代了DVM虚拟机，ART直接执行的是本地机器码，安装过程中就编译成了本地机器码，DVM需要通过JIT即时编译器将将DEX转换成本地机器码执行。


**JVM内存管理机制**

1：JVM内存回收区域为堆，虚拟机栈本地方法栈，方法区

2：首先找到垃圾，通过引用计数法和可达性分析法，引用计数存在相互引用问题导致无法回收，可达性分析法一般以堆上存活对象作为GC Root根对象根据引用链往下查找，根对象不可达的对象是可以回收的对象

3：回收垃圾，有标记法，易造成内存碎片化，复制法空间占用大，效率高，标记整理法，和分代法。分代法分为新生代和老年代，新生代分为Eden和两个Survivor，比例为8:1:1，因为研究发现，新生代Eden区和第一个survivor区90%的对象都会回收，当第一次GC时会把eden区里存活的对象拷贝到第一个survivor区，当再次GC时，通过复制算法，将Eden和第一个survivor区的存活对象拷贝到第二个survivor区。当第二个survivor区满了的时候，会将这些存活对象拷贝到老年代中。如果人为触发GC发现Survivor空间不够时，会担保进入老年代。

**Android内存管理机制**

DVM内存管理：

基于JVM回收机制，运用更好的算法帮助执行，默认用的标记擦除回收算法，

ART内存管理：

主要基于Mark-Sweep GC和Compacting GC


**强引用，软引用，弱引用，虚引用**

1：强引用，new对象直接创建的

2：软引用，内存不够时进行回收

3：弱引用，GC时进行回收不管内存是否够用

4：虚引用，被回收时收到通知，一个对象是否有虚引用的存在，完全不会对其生存时间构成影响，也无法通过虚引用来取得一个对象实例。为一个对象设置虚引用关联的唯一目的就是能在这个对象被收集器回收时收到一个系统通知。

**threadlocal作用**

主要多个线程使用同一个变量时，会互相造成干扰，那么通过这个类，会给每个类复制一份变量的副本，副本隔离，互不干扰。

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

**Android Surfaceview双缓冲**

**Android Webview 用法及优化**

**Android Viewpager懒加载**

**JAVA 类加载过程**

**JAVA 线程状态**

**JAVA final，finally，finallize区别**

final:其修饰的常量不可更改，方法无法重载，类无法继承

finally：任何执行try 或者catch中的return语句之前，（如果有finally）都会先执行finally语句。
如果finally中有return语句，那么程序就return了，所以finally中的return是一定会被return的，
编译器把finally中的return实现为一个warning

finallize：对象在内存回收之前，可重写该函数，将对象重新挂上引用链，如果再次GC时finallize方法不会再执行，对象将被回收

**JAVA 接口和抽象类区别**
