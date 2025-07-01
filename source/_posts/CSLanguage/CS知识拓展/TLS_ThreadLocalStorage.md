---
title: 线程本地存储（TLS：Thread Local Storage）
tags:
  - 程序语言
  - C#语言
categories:
  - [程序语言, C#语言]
author:
  - nightstardawn
---
# 线程本地存储

这就简单的说明一下，有机会再详细的写，～(∠・ω< )⌒★

## 一、定义

在一个进程中，所有线程的堆内存是共享的（栈除外，线程的栈内存是相互隔离的）。线程局部存储技术是使每个线程与其它线程数据存储隔离。

.Net 提供了三个用于线程本地存储的方法
- 线程的静态字段（ThreadStatic）
- ThreadLocal<T>
- 数据槽（LocalDataStoreSlot ）

## 二、三种方法

### 线程的静态字段（ThreadStatic）

关键词：`[ThreadStatic]`

使用`ThreadStaticAttribute`标记的`static`字段 不会在线程之间共享。
</br>每个执行线程都有单独的字段实例，并分别设置和获取该字段的值。 如果在不同的线程上访问该字段，则该字段将包含不同的值。

下面是使用案例：

```csharp
class Programe
{
   
    [ThreadStatic] public static string Username = "";
    static void Main()
    {

        Username = "mainthread";
   
        Thread thread1 = new Thread(() => {  Username = "thread1"; Console.WriteLine($"Username：{Username}"); });
        Thread thread2 = new Thread(() => { Username = "thread2"; Console.WriteLine($"Username：{Username}"); });
        Thread thread3 = new Thread(() => { Username = "thread3"; Console.WriteLine($"Username：{Username}"); });
  
        thread1.Start();
        thread2.Start();
        thread3.Start();

        Console.WriteLine($"Username：{Username}");
    }
}


//输出结果：
//Username：mainthread
//Username：thread1
//Username：thread2
//Username：thread3
```

### 数据槽

据说用的不多，暂时先不写了 欸嘿~
一般可以用ThreadLocal<T> 类代替（ai说的）

### ThreadLocal<T> 类

这个类相当于将一个变量的值封装到这类中，各个线程在使用这个类型的时候，是各自独立的。

该类包装过的类型变量的只能在该线程中使用，其他线程包括子线程无法使用。即：

- 因为每个 Thread 内有自己的实例副本，且该副本只能由当前 Thread 使用。这是也是 ThreadLocal 命名的由来。
- 既然每个 Thread 有自己的实例副本，且其它 Thread 不可访问，那就不存在多线程间共享的问题。

```csharp
static void Main(string[] args)
{
     ThreadLocal<int> threadLocal = new ThreadLocal<int>();
     //在主线程这个变量值为1
     threadLocal.Value = 1;
     new Thread(() => Console.WriteLine($"托管线程ID：{Thread.CurrentThread.ManagedThreadId} 值为：{threadLocal.Value++}")).Start();
     new Thread(() => Console.WriteLine($"托管线程ID：{Thread.CurrentThread.ManagedThreadId} 值为：{threadLocal.Value++}")).Start();
     new Thread(() => Console.WriteLine($"托管线程ID：{Thread.CurrentThread.ManagedThreadId} 值为：{threadLocal.Value++}")).Start();
     Console.WriteLine($"主线程ID：{Thread.CurrentThread.ManagedThreadId} 值为：{threadLocal.Value}");
}
//输出
//托管线程ID：10 值为：0
//主线程ID：1 值为：1
//托管线程ID：11 值为：0
//托管线程ID：12 值为：0
```








