---
layout: post
title: Kotlin 摘记
date:  2019-05-06 17:35:00 +0900  
description:  Java 后台开发摘记
img: post-1.jpg # Add image post (optional)
tags: [Android]
author: # Add name author (optional)
androidoriginal: true
---
{{site.label1}} <a href="https://www.leachchen.com/" target="\_blank">https://www.leachchen.com/</a> {{site.label2}}

后续更新...

1. 安装intellij idea(收藏地址，配置maven)

2. 创建springboot init项目

3. 多模块配置,pom配置文件 https://blog.csdn.net/hpc_2015/article/details/81358165 https://blog.csdn.net/hpc_2015/article/details/81358165

4. 配置里的build放在application所在pom里，否则会报错

5. 多模块配置，每个模块parent为项目初始类（不能为公共模块，否则会导致公共模块打包时打包不出来class文件），每个模块需要添加build配置输出路径

6. mybatis 配置

7. kotlin打包每个目录下都要放build配置，否则可能有些类打不出来，目录层级也要多一级

8. 打包过程中若出现异常，则要看target目录下是否对应的类都生成了
