---
layout: post
title: RxJava原理
date:  2018-03-20 10:18:00 +0900
description: RxJava原理
img: post-4.jpg # Add image post (optional)
tags: [原理,Android]
author: # Add name author (optional)

theory: true
#needcomplete: true
---

{{site.label1}} <a href="https://www.leachchen.com/" target="\_blank">https://www.leachchen.com/</a> {{site.label2}}


```
Observable.create(new ObservableOnSubscribe<Object>() {
    @Override
    public void call(@NonNull Observer<Object> e) throws Exception {
        //事件源业务代码
        e.onNext(o);
    }
}).subscribe(new Observer<Object>() {

    @Override
    public void onNext(@NonNull Object o) {
        //订阅者业务代码
    }
});

public class Observable<T> {
  final ObservableOnSubscribe<T> source;

  ...

  public static <T> Observable<T> create(ObservableOnSubscribe<T> source) {
      return new Observable<T>(source);
}

public class Observable<T> {
  final ObservableOnSubscribe<T> source;



  protected void subscribe(Observer<? super T> observer) {
      source.call(observer);
  }        

```




**背压问题**

当上下游在不同的线程中，通过Observable发射，处理，响应数据流时，如果上游发射数据的速度快于下游接收处理数据的速度，这样对于那些没来得及处理的数据就会造成积压，这些数据既不会丢失，也不会被垃圾回收机制回收，而是存放在一个异步缓存池中，如果缓存池中的数据一直得不到处理，越积越多，最后就会造成内存溢出，这便是响应式编程中的背压（backpressure）问题.RxJava1未能处理这个问题，RxJava2 新增Flowable来处理该问题。

由于基于Flowable发射的数据流，以及对数据加工处理的各操作符都添加了背压支持，附加了额外的逻辑，其运行效率要比Observable慢得多，只有在需要处理背压问题时，才需要使用Flowable。

Flowable有几种策略：

缓存：当事件消费者不能及时处理事件时，产生的事件可以缓存起来，等待消费者消费

丢弃：当事件消费者处理速度慢无法处理更多事件时，它可以丢弃所有的事件，当消费者可以继续工作时，处理最新生成的事件

报错 事件消费者可以直接抛出MissingBackpressureException
