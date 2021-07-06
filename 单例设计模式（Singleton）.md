---
title: 单例设计模式（Singleton）
date: 2020-06-10 16:40:22
categories: 
- 设计模式
tags: 
- 设计模式
---

# 代码实现

单例设计模式主要目的就是全局有且只有一个该对象，这个是唯一的，可以全局调用，比如 Logger 就是这样的存在。下面简单用三种方式来实现，这部分笔记慢慢补充起来。

**第一种实现，饿汉式（开发用）：**

```java
public void main(String[] args) {
    Singleton s = Singleton.getInstance();
}

class Singleton() {
    // 在类中直接创建好对象
    private Singleton s = new Singleton();
    // 私有构造函数，禁止外部创造该对象
    private Singleton() {};
    // 提供 accessor
    public static Singleton getInstance() {
        return s;
    }
}
```



**第二种，懒汉式：**

```java
public void main(String[] args) {
    Singleton s = Singleton.getInstance();
}

class Singleton() {
    // 声明
    private Singleton s;
    // 私有构造函数，禁止外部创造该对象
    private Singleton() {};
    // 提供 accessor
    public static Singleton getInstance() {
        if (s == null) {
            // 这里有线程隐患，从而产生两次对象
            s = new Singleton();
        }
        return s;
    }
}
```



**第三种，作为了解：**

```java
public void main(String[] args) {
    Singleton s = Singleton.getInstance();
}

class Singleton() {
    // 在这里即使 public 修饰了，但是也没办法修改它
    public static final Singleton s = new Singleton();
    // 私有构造函数，禁止外部创造该对象
    private Singleton() {};
}
```









