---
layout: post
title: CSS 摘记
date:  2019-02-11 12:40:00 +0900  
description: Vue
img: post-6.jpg # Add image post (optional)
tags: [Html,Vue]
author: # Add name author (optional)
vue: true
---

{{site.label1}} <a href="https://www.leachchen.com/" target="\_blank">https://www.leachchen.com/</a> {{site.label2}}

CSS布局摘记,持续更新

## <a href="https://www.cnblogs.com/nuannuan7362/p/5823381.html" target="\_blank">flex</a> ##

**1.flex-direction**

决定主轴的方向，即项目排列的方向，有四个可能的值：row(默认)|row-reverse|column|column-reverse

row:主轴为水平方向，项目沿主轴从左至右排列

column：主轴为竖直方向，项目沿主轴从上至下排列

row-reverse：主轴水平，项目从右至左排列，与row反向

column-reverse：主轴竖直，项目从下至上排列，与column反向

**2.flex-wrap**

默认情况下，item排列在一条线上，即主轴上，flex-wrap决定当排列不下时是否换行以及换行的方式，可能的值nowrap(默认)|wrap|wrap-reverse

nowrap：自动缩小项目，不换行

wrap：换行，且第一行在上方

wrap-reverse：换行，第一行在下面

**3.flex-flow**

是flex-direction和flex-wrap的简写形式，如：row wrap|column wrap-reverse等。默认值为row nowrap，即横向排列 不换行。

**4.justify-content**

决定item在主轴上的对齐方式，可能的值有flex-start（默认），flex-end，center，space-between，space-around。当主轴沿水平方向时，具体含义为

flex-start：左对齐

flex-end：右对齐

center：居中对齐

space- between：两端对齐

space-around：沿轴线均匀分布

**5.align-items**

决定了item在交叉轴上的对齐方式，可能的值有flex-start|flex-end|center|baseline|stretch，当主轴水平时，其具体含义为

flex-start：顶端对齐

flex-end：底部对齐

center：竖直方向上居中对齐

baseline：item第一行文字的底部对齐

stretch：当item未设置高度时，item将和容器等高对齐


**6.align-content**

该属性定义了当有多根主轴时，即item不止一行时，多行在交叉轴轴上的对齐方式。注意当有多行时，定义了align-content后，align-items属性将失效。align-content可能值含义如下（假设主轴为水平方向）：

flex-start：左对齐

flex-end：右对齐

center：居中对齐

space- between：两端对齐

space-around：沿轴线均匀分布

stretch：各行将根据其flex-grow值伸展以充分占据剩余空间


**float**

relarive 使用float脱离文档流时，其他盒子会无视这个元素，但其他盒子内的文本依然会为这个元素让出位置，环绕在该元素的周围。

absolute 

<div id="相对元素a">
     <div id="绝对元素b"></div>
</div>

如果a没有position:relative,b的postion:absolute则是以body绝对定位
