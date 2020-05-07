---
layout: post
title: 设计模式
date:  2018-03-20 09:50:00 +0900
description: Handler机制
img: post-3.jpg # Add image post (optional)
tags: [原理，设计模式]
author: # Add name author (optional)

theory: true
#needcomplete: true
---

{{site.label1}} <a href="https://www.leachchen.com/" target="\_blank">https://www.leachchen.com/</a> {{site.label2}}

**单例模式**

```

饱汉模式(懒汉模式)

  优点：懒加载启动快，资源占用小，使用时才实例化，无锁。

  缺点：非线程安全。

  public class DesignModeSingle
  {
    private static DesignModeSingle mInstance = null;

    public static DesignModeSingle getInstance()
    {
      if(mInstance == null)
      {
        mInstance = new DesignModeSingle()
      }
      return mInstance
    }
  }

饱汉模式(懒汉模式),线程安全

  优点：懒加载，线程安全。

  注：实例必须有 volatile 关键字修饰，其保证初始化完全。

  public class DesignModeSingle
  {
    private volatile static DesignModeSingle mInstance = null;

    public static DesignModeSingle getInstance()
    {
       if(mInstance == null)
       {
          synchronized(DesignModeSingle.class)
          {
              if(mInstance == null)
              {
                  mInstance = new DesignModeSingle();
              }
          }

       }
      return mInstance;
    }  

  }



饿汉模式

  优点：饿汉模式天生是线程安全的，使用时没有延迟。

  缺点：启动时即创建实例，启动慢，有可能造成资源浪费。

  public class DesignModeSingle
  {
    private static DesignModeSingle mInstance = new DesignModeSingle();

    public static DesignModeSingle getInstance()
    {
      return mInstance;
    }

}
```

**工厂模式**

```

简单工厂模式

一个抽象产品类，多个产品实现它，一个工厂类，根据传入的类型生产不同的对象

缺点
1 扩展性差（我想增加一种面条，除了新增一个面条产品类，还需要修改工厂类方法）
2 不同的产品需要不同额外参数的时候 不支持。

public interface IPeople
{
  public void location();
}

public class GungDong implements IPeople
{

  public void location()
  {
    System.out.println("people from guangdong")
  }
}

public class JiangXi implements IPeople
{

  public void location()
  {
    System.out.println("people from jiangxi")
  }
}

public class PeopleFactory
{

  public static IPeople createLocation(int type)
  {
    switch(type)
    {
      case 1:
        return new GungDong();
      break;
      case 2:
        return new JiangXi();
      break;  
    }
    return null;
  }
}

public static void main(String []args)
{
  IPeople people = PeopleFactory.createLocation(1)
  people.location()
}


工厂方法模式

一个抽象产品类，可以派生出多个具体产品类。一个抽象工厂类，可以派生出多个具体工厂类。每个具体工厂类只能创建一个具体产品类的实例。


public interface IProduct
{
  void create()
}

public PhoneProduct implaments IProduct
{

    public void create()
    {

    }
}

public PenProduct implaments IProduct
{

    public void create()
    {

    }
}


public interface IFactory
{
    IProduct createProduct()
}

public PhoneFactory implaments IFactory
{
    IProduct createProduct()
    {
        return new PhoneProduct()
    }

}


抽象工厂模式

多个抽象产品类，每个抽象产品类可以派生出多个具体产品类。一个抽象工厂类，可以派生出多个具体工厂类。每个具体工厂类可以创建多个具体产品类的实例。

public interface IProduct1
{
  void create()
}

public interface IProduct2
{
  void create()
}

public PhoneProduct implements IProduct1
{
  public void create()
  {

  }
}

public PenProduct implements IProduct2
{
  public void create()
  {

  }
}

public interface IFactory
{
  IProduct1 create()
  IProduct2 create()
}

public ProductFactory implements IFactory
{
  IProduct1 create()
  {
    return new PhoneProduct()  
  }

  IProduct2 create()
  {
    return new PenProduct()
  }
}

```


**观察者模式**

有一个对象成为被观察者，其它对象被成为观察者，被观察者持有其它对象引用，当被观察者变更时，可以通过持有的对象引用通知其它所有观察者。<br>
一个被观察者接口，一个观察者接口

```
//被观察者接口
public interface IObserverable
{
    void registerObserver(Observer o);
    void removeObserver(Observer o);
    void notifyObserver();
}

//观察者接口
public interface IObserver
{
    void update()
}


public Observerable implements IObserverable
{
    private List<Observer> mObserverList = new ArrayList<Observer>();

    public void registerObserver(Observer o)
    {

      if(!mObserverList.contains(o))
      {
        mObserverList.add(o)
      }
    }

    public void removeObserver(Observer o)
    {
       if(!mObserverList.isEmpty())
       {
         mObserverList.remove(o)
       }
    }

    public void notifyObserver()
    {
        for(int i = 0;i<mObserverList.size();i++)
        {
          mObserverList.get(i).update()
        }

    }

}


public Observer implements IObserver
{
    public void update()
    {

    }
}

```

**适配器模式**

创建一个接口，再创建一个类重写该接口里面的一个方法，再创建一个类，继承该类并实现接口，此时这个类只需要实现接口中的另外一个方法

```
public interface A
{
    void methodsA()
    void methodsB()
}

public class B
{
    public void methodsA()
    {

    }
}

public class C extends B implements A
{
    public void methodsB()
    {

    }
}

```

**建造者模式**

```
public class A
{
    private String value1
    private String value2

    public void setValue1(String v)
    {
      this.value1 = v
    }

    public void setValue2(String v)
    {
      this.value2 = v
    }
}

public interface IBuilder
{
    IBuilder buildValue1(String v);
    IBuilder buildValue2(String v);
    A create()
}

public Builder implements IBuilder
{
    private A mA

    Builder()
    {
      mA = new A()
    }

    public IBuilder buildValue1(String v)
    {
        mA.setValue1(v)
    }

    public IBuilder buildValue2(String v)
    {
        mA.setValue2(v)
    }

    public A create()
    {
      return mA
    }

}

Builder builder = new Builder().setValue1("aa").setValue2("bb")

```
