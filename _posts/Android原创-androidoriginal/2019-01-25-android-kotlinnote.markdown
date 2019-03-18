---
layout: post
title: Kotlin 摘记
date:  2019-01-25 17:37:00 +0900  
description:  Kotlin 摘记
img: post-1.jpg # Add image post (optional)
tags: [Android]
author: # Add name author (optional)
androidoriginal: true
---
{{site.label1}} <a href="https://www.leachchen.com/" target="\_blank">https://www.leachchen.com/</a> {{site.label2}}

持续更新...

1. 定义变量时，可在类型后面加一个问号?，表示该变量是Nullable，不加表示该变量不可为null。如下：

```
var s:String? = null
var s2:String = "xxx" //如果该变量被赋值为null，则编译不通过
```
