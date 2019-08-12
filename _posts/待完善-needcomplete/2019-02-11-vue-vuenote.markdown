---
layout: post
title: Vue 摘记
date:  2018-03-20 13:16:00 +0900  
description: Vue
img: post-9.jpg # Add image post (optional)
tags: [Html,Vue]
author: # Add name author (optional)

#vue: true
needcomplete: true
---

{{site.label1}} <a href="https://www.leachchen.com/" target="\_blank">https://www.leachchen.com/</a> {{site.label2}}

Vue摘记,持续更新

## **render: h => h(App)** ##

```
new Vue({
  el: '#app', //render渲染的内容替换id为app的节点
  router,
  store,
  render: h => h(App)
})

render: h => h(App)相当于
render: function (createElement) {
   return createElement(
     'h' + this.level,   // tag name 标签名称
     this.$slots.default // 子组件中的阵列
   )
}
```
其相当于创建了一个基于APP.vue里面定义的布局节点，用来替换掉index.html里面定义的<div id="app"></div> <br>
vue demo项目里开始运作是从main.js

## **router-view** ##

```
router.map({
  '/foo': {
    // 路由匹配到/foo时，会渲染一个Foo组件,当你访问 /foo 时，router-view 就被 Foo 组件替换了。
    component: Foo
  }
})
```

相当于一个容器，用于页面切换，可局部切换

## **eslint语法限制** ##

eslint语法限制较严格，可将其关闭。若创建项目时选择了插件，可将其关闭，折腾了半天按照网上步骤操作无效，可在项目根目录下.eslintignore文件里添加忽略检查的文件或者文件夹

## **export default** ##

这是在复用组件的时候用到的。假设我们写了一个单页面组件 A 文件，而在另一个文件 B 里面需要用到它，那么就要用 ES6 的 import/export 语法 ，在文件 A 中定义输出接口
```
export default {
  name: 'App'  
}
```

在文件 B 中引入

```
import App(这个名称可任意取) from './App'
```

然后再生成一个 Vue 实例

```
new Vue({
  el: '#app',
  router,
  components: { App }, //名称与import定义的名称同名
  template: '<App/>'
})
```

把引入的组件用起来，这样就可以复用组件 A 去配合文件 B 生成 html 页面了。所以在复用组件的时候，export 和 new Vue 缺一不可。



## **for 循环** ##

```
<template>
  <div>
    <div v-for="(value, key, index)  in sites" :key="value.id">{{value}}-{{key}}-{{index}}-{{value.type}}</div>
    <div>{{msg}}</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
       sites:[{id:1,type:'a'},{id:2,type:'b'},{id:3,type:'c'}],
       msg:'aa'
    };
  }
};
</script>

界面

{ "id": 1, "type": "a" }-0--a
{ "id": 2, "type": "b" }-1--b
{ "id": 3, "type": "c" }-2--c
aa

```


## **注册组件** ##

```
第一步在back.vue中导出组件

<template>
  <div>
    <!-- <el-button id="common-back" icon="back" slot="left" v-on:click="goBack"></el-button> -->
    <mt-header id="common-back">
      <mt-button  icon="back" slot="left" @click="goBack">返回</mt-button>
    </mt-header>
  </div>
</template>

<script>
export default {
  name: "commonback",
  methods: {
    goBack() {
      this.$router.go(-1);
    }
  }
};
</script>


<style>
#common-back {
  width: 100%;
  height: 50px;
}
</style>

第二步在main.js中注册组件
import back from '@/components/back'
Vue.component('commonback', {
  render: back.render,
  data: back.data,
  methods:back.methods
})

第三步在test.vue中使用组件

<template>
    <div>
      <commonback></commonback>
        {{msg}}

    </div>    
</template>

<script>

 //局部注册组件
//import back from '@/components/back'
export default {
    data(){
        return{
            msg:"aaa"
        };
    },
    //  components:{
    //     "commonback":back
    //  }
}
</script>

```

## **引入外部js** ##

```
第一步在外部js中定义方法
function diyfun() {
    alert("bbbbb")
}
export {
    diyfun
}
第二步在test.vue中

<script>
import {diyfun} from '../public/js/aa.js' //注意路径
export default {
 data () {
  return {
   testvalue: ''
  }
 },
 methods:{
   myfun:function(){
     diyfun();
   }
 }
}
</script>

<button @click="myfun">测试</button>
```


## **:class** ##

```
https://www.cnblogs.com/big-snow/p/5718728.html
HTML代码：
<div :class="classA">Demo2</div>

Javascript代码：
data: {
  classA: 'class-a'  //当classA改变时将更新class
}
渲染后的HTML:
<div class="class-a">Demo2</div>
```


## **computed** ##

```
https://www.cnblogs.com/gunelark/p/8492468.html
自己的理解：

computed用来监控自己定义的变量，该变量不在data里面声明，直接在computed里面定义，然后就可以在页面上进行双向数据绑定展示出结果或者用作其他处理；
computed比较适合对多个变量或者对象进行处理后返回一个结果值，也就是数多个变量中的某一个值发生了变化则我们监控的这个值也就会发生变化，举例：购物车里面的商品列表和总金额之间的关系，只要商品列表里面的商品数量发生变化，或减少或增多或删除商品，总金额都应该发生变化。这里的这个总金额使用computed属性来进行计算是最好的选择
与watch之间的区别：

刚开始总是傻傻分不清到底在什么时候使用watch，什么时候使用computed。这里大致说一下自己的理解：

watch主要用于监控vue实例的变化，它监控的变量当然必须在data里面声明才可以，它可以监控一个变量，也可以是一个对象

computed相比method中，computed方法体只执行一次，除非里面依赖的值发生了改变，性能更高
```

## **store** ##
https://blog.csdn.net/qq_38658567/article/details/82847758
```
当我们在页面上点击一个按钮，它会处发(dispatch)一个action, action 随后会执行(commit)一个mutation, mutation 立即会改变state, state 改变以后,我们的页面会state 获取数据，页面发生了变化。 Store 对象，包含了我们谈到的所有内容，action, state, mutation，所以是核心了
```
