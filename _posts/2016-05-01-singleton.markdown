---
layout: post
title: 单例模式
date: 2016-05-01
categories: designpatten
tags:
- 设计模式
---

单例模式就不多说了，说一下几个问题

<!-- more -->

双重写检查问题:

```
public class Singleton {  
 private static Singleton instance = null;  
 private Singleton() {}  
 public static Singleton getInstance() {  
  if (instance == null) {  
   synchronized (Singleton.class) {// 1  
    if (instance == null) {// 2  
     instance = new Singleton();// 3  
    }  
   }  
  }  
  return instance;  
 }  
}  
```

- A、B线程同时进入了第一个if判断
- A首先进入synchronized块，由于instance为null，所以它执行instance = new Singleton();
- 由于new Singleton()操作非原子操作，jvm会执行三步操作
  1. 给 instance 分配内存
  2. 调用 Singleton 的构造函数来初始化成员变量
  3. 将instance对象指向分配的内存空间

- 由于JVM内部的优化机制，2，3步可能会出现重排序，JVM先画出了一些分配给Singleton实例的空白内存，并赋值给instance成员（注意此时JVM没有开始初始化这个实例），然后A离开了synchronized块。
- B进入synchronized块，由于instance此时不是null，因此它马上离开了synchronized块并将结果返回给调用该方法的程序。
- 此时B线程打算使用Singleton实例，却发现它没有被初始化，于是错误发生了。


#### 使用内部类延迟加载可以解决该问题

``` java
class Singleton {
    private static class LazyHolder {
        public static Singleton instance = new Singleton();
    }
    private Singleton() {
    }
    public static Singleton getInstance() {
        return LazyHolder.instance;
    }
    public void sayHello() {
        System.out.println('1');
    }
}

public class SingletonTest {
    public static void main(String[] args) {
        Singleton s = Singleton.getInstance();
        s.sayHello();
    }
}
```

#### 使用枚举定义

``` java
public enum Singleton{
    INSTANCE;
}


public class SingletonTest {
    public static void main(String[] args) {
        Singleton.INSTANCE;
    }
}
```