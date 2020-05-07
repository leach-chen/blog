---
layout: post
title: Handler原理
date:  2018-03-16 09:50:00 +0900
description: Handler原理
img: post-7.jpg # Add image post (optional)
tags: [Android，Handler，原理]
author: # Add name author (optional)

theory: true
#needcomplete: true
---

{{site.label1}} <a href="https://www.leachchen.com/" target="\_blank">https://www.leachchen.com/</a> {{site.label2}}

Handler，主要用于异步消息处理线程，将耗时的操作放在后台线程执行，而执行的结果是放在UI线程中操作UI显示
异步消息处理线程，主要有几个部分组成。ActivityThread、Looper、MessageQueue，Message、Handler
当APP启动时，会执行ActivityThread.java里面的main函数，会调用Looper.prepareMainLooper()创建一个Looper对象，创建Looper对象的同时会创建一个MessageQueue，然后调用Looper.loop()进入消息循环。我们可以往消息队列里添加消息Message，Message可以与Handler对象关联。当MessageQueue轮询到要处理的消息时，可以通过Message里面保存的Handler对象回调出来，进行消息的处理。

为什么一个异步消息处理线程里只能创建一个looper对象？<br>
一个异步消息处理线程里只能有一个MessageQueue，创建Looper对象的时候会创建一个MessageQueue，如果再次去创建looper对象，程序里进行了判断会抛出异常

一个普通线程如何变成异步消息处理线程？<br>
在线程里调用Looper.prepare()创建loop对象，然后调用Looper.loop()进入消息循环

android的UI操作必须要在主线程中进行，为什么？<br>
因为在多线程中同时执行UI操作是不安全的

可以把全部的操作都放在主线程中么？<br>
不可以，耗时操作需要在后台线程执行，避免ANR
