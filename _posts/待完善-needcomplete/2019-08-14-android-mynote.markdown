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

1：加锁，主要为了解决线程并发导致抢占资源问题

2：synchronized代码修饰的代码段，查看字节码会加上monitorenter和monitorexit，线程要进入时，需要获取monitor对象，同时计数加1，当其它线程要进入时，因为计数不为0，它就要等待，直到当前执行的线程执行到monitorexit退出，monitor计数变为0.它才能获得所有权

**volatile理解**

每个线程都有自己的工作内存及所有线程对应的主存，为提高效率，线程会拷贝变量的副本，操作完成后再同步回主存。变量的值对所有线程是透明的，相当于告诉线程从主存去获取该变量值，防止多线程并发时对该变量操作产生问题

**线程池创建方式**

1：newFixedThreadPool,创建固定大小的线程池，LinkedBlockingQuene作为阻塞队列，线程池中没有任务也不会释放线程

2：newCachedThreadPool,创建缓存线程池，线程超过1分钟没有任务会释放，最大可创建2^31个线程，内部使用SynchronousQueue作为阻塞队列

3：newSingleThreadExecutor,创建只有一个线程的线程池，当该线程没有正常完成任务，会启动另一个线程继续完成任务，内部使用LinkedBlockingQueue作为阻塞队列

4：newScheduledThreadPool,创建定时任务的线程

ThreadPoolExecutor类构造器：

1：核心线程数

2：最大线程数

3：阻塞队列处理方式

4：线程工厂

5：线程拒绝处理时是使用策略

**线程池状态**

1：新建

2：可运行

3：运行中

4：阻塞

5：消亡

**HaseMap理解**

1：纵向是数组，横向是链表+红黑树，通过key进行hashcode运算，它是根据key的内存地址得到一个整形值，然后对其与数组长度进行取余得到数据在数组中的位置。然后通过equal与key在链表上进行比较，如果存在则替换，否则追加。当链表长度超过8时JDK1.8上会转换成红黑树

2：一般字符串常量或者整形作为key，因为不可变。也可以自定义对象作为key，但是需要重写hashcode和equal，因为当两个对象在堆上是对应两个地址空间时，按默认的hashcode运算，这两个对象hashcode是不相同的，所以要重写hashcode让其相同。同样equal默认跟==效果一样，值和内存地址都要
相同，那么此时我们需要重写它，让其只比较某些值相同则相同


**常用排序算法原理及实际复杂度**

1：冒泡排序，稳定，双层循环，一个缓存值的变量，每轮循环两两相比，不符合条件则交换位置。最优时间复杂度n，最差n^2

2: 插入排序，稳定，双层循环，一个缓存值的变量，假设第n个之前都是排好序的，第n+1个从后往前与前面的值进行比较，temp值等于第n+1位置的值，若不符合条件则相邻两个值互换位置，若符合条件则直接退出当前循环，用temp值放入比较到的位置，最优时间复杂度n，最差n^2

3：快速排序，不稳定，双层循环，一个缓存位置的值，加上当前第i个值是最小的，第二层循环依次两两比较，若不符合条件则将最小值的位置替换为新的位置值，依次完成与后面的值的比较，比较完成后，若位置i有变化，则与那个值进行互换。最优时间复杂度nlog2n，最差n^2

**常用数据结构**

1：HashMap 线程不安全，Hashtable线程安全，ConcurrentHashMap同步性能更好

2：ArrayList，可变数组，线程不安全，当数据长度超过容量，会将数组按原来长度延长50%，然后把数据拷贝进去，vector，也是可变数组，线程安全，容量超过默认扩充到原来2倍，LinkedList是双向链表，SparseArray是基于两个数组的map，产生冲突直接覆盖

**Android系统架构**

1：应用层

2：framework层，封装的系统组件

3：运行层，基于c/c++，DVM运行于此层

4：linux内核层

**Android自定义VIEW**（待完善）

1：attr.xml 里面添加自定义属性，布局里添加 xmlns:android_custom="http://schemas.android.com/apk/res-auto"，通过TypedArray obtainStyledAttributes获取到自定义属性数组

2：onMeasure

3：onLayout

4：onDraw

5：其它，viewtreeobservable，onfourcechange，。。

**Android Filter**

1：action匹配任意一个，category都要匹配，data匹配任意一个

**Android Handler机制**

ActivityThread 创建时会执行main方法，调用looprpemainloop，创建loop对象及messagequeue对象，每个handler与一个message对象对应，message对象持有handler对象引用，handler可以往消息队列里面添加对象，当消息循环到时，通过message保存的handler对象回调出来，
进行相应业务处理


**Android 主线程无线循环为什么不会卡死**

进入无线循环之前创建了binder线程，binder线程与WMS，AMS交互，binder再通过handler与主线程交互

**Android Binder机制**

1：分为服务端，会创建一个binder引用，驱动层，会创建一个mRemote binder引用，还有客户端

2：当服务端和客户端处于同一个进程时，客户端获取到的是服务端的这个binder引用，当不在同一个进程，获取到的是驱动层的mRemote Binder引用

**Android AMS WMS工作原理**

**Android Activity启动过程**

**Android Surfaceview双缓冲**

**Android Webview 用法及优化**

**Android Viewpager注意**

1：实现PagerAdapter，ViewPager.OnPageChangeListener

2：在instantiateItem创建类

3：在destroyItem移除view

4：getItemPosition POSITION_NONE重新创建view


**Android Viewpager懒加载**

1：通过setUserVisibleHint里面的boolean值，判断是否加载，依然是三个界面，只是不加载数据

2：onPageSelected的时候再去初始化界面

**JAVA 类加载过程**

**JAVA 线程状态**

1：新建

2：运行

3：阻塞

4：等待

5：超时

6：终止


**JAVA final，finally，finallize区别**

final:其修饰的常量不可更改，方法无法重载，类无法继承

finally：任何执行try 或者catch中的return语句之前，（如果有finally）都会先执行finally语句。
如果finally中有return语句，那么程序就return了，所以finally中的return是一定会被return的，
编译器把finally中的return实现为一个warning

finallize：对象在内存回收之前，可重写该函数，将对象重新挂上引用链，如果再次GC时finallize方法不会再执行，对象将被回收

**JAVA 接口和抽象类区别**

1：接口只能有公开方法
2：接口不能有方法体
3：均不能实例化
4：一个类可以实现多个接口，一个类只能继承一个抽象类
5：接口不能有静态方法，抽象类可以
6：接口不能有普通成员变量，抽象类可以


**Android WebView 用法**

1：通过addJavascriptInterface注入js方法

2：WebChromeClient，处理Javascript的对话框、网站图标、网站标题以及网页加载进度等

3：WebViewClient，处理各种通知、请求等事件


**热修复原理,DEX**

**View SuferaceView GLSuferaceView区别**

**JNI理解**

**JNI基本用法**

**FFmpeg各个库的作用**
目录bin下有四个可执行文件：
ffplay：一个简单的音视频播放器。
ffmpeg：用于格式转换、解码或编码的工具。
ffprobe：用来获得每天文件的信息。
ffserver：用来搭建流媒体服务器。

目录lib下有七种库文件：
ibavformat：用于各种音视频封装格式（音频文件）的生成和解析。
libavcodec：用于各种类型图像、声音和视频的编解码。
libavdevice: 封装了和底层设备打交道的函数。
libavfilter:包括了图像处理中的各种滤镜效果。
libavutil：包含一些公共的函数。
libswscale：包含了图像，视频缩放、色彩映射转换等函数。
libswresample: 包含了调整声音采用率的函数。

**FFmpeg编译过程**

**Ijkplayer的编译过程**

**FFmpeg解码MP4**

**FFmpeg解码H264**

**FFmpeg 代码封装执行命令行**

**静态库和动态库的区别**

静态库（.a 、.lib）动态库（.so 、.dll ）
动态库和静态库的本质区别是，动态库是在程序运行时链接的，而静态库在编译时把代码加入目标程序，那么程序运行时就不需要了。所以使用静态库时生成的目标程序可以脱离源码运行，而动态库生成的目标程序，还需要先安装库才行

**I帧 P帧 B帧**

**码率**

**分辨率**

**帧率**
