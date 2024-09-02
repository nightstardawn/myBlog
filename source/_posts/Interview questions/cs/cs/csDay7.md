---
title: 面试题汇总CShapeDay7
tags:
  - 面试题汇总
categories:
  - [面试题，C#]
author:
  - nightstardawn
---

# C# Day7

## 1.C#中如何让一个类不被其他类继承？

**答案：**
使用 密封关键字 sealed 修饰该类

## 2.c#中使用泛型的好处？

**答案：**

1. 可以为不同类型对象的相同行为进行通用处理，提升代码复用率
2. 避免装箱拆箱，提升性能

## 3.c#中元组对于我们的作用是什么？

**答案：**

- 可以在不写数据结构的情况下，利用元组处理多返回值，或者临时的数据集合

- 关于元组的使用
  ```cs
    //1.无变量名元组的声明(获取值:Item''作为从左到右依次的参数，N从1开始)
    (int,float,bool,string)yz=(1,5.5f,true,"123");
    print(yz.Item1);
    print(yz.Item2);
    print(yz.Item3);
    print(yz.Item4);
    //2.有变量名元组的声明
    (int i,flaat f,bool b,string str)yz2=(1,5.5f, true, "123");
    print(yz2.i)
    print(yz2.f);
    print(yz2.b);
    print(yz2.str);
  ```

## 4.请说明 Thresd、ThreadPool、Task 分别是什么？简单说明彼此的区别？

**答案：**

- Thread 是线程，可以通过开启线程处理复杂逻辑，避免主线程卡顿
- ThreadPool 是线程池，它是 C# 为线程实现的缓存池，主要用于减少线程的创建，减少 GC 的触发
- Task 是任务，它是基于线程池的优化，让我们可以更加方便的控制线程

## 5.请简述 GC（垃圾回收）产生的原因，并且至少说出避免 GC 发生的三种方式？

**答案：**
原因：避免堆内存溢出而产生的回收机制，当不在使用的堆内存占用达到一定上限时，将会进行垃圾回收
避免方式：

1. 尽量减少 new 的对象，尽量复用对象（可以使用缓存池）
2. 用 StringBuilder 替换 string，避免字符串拼接时产生的垃圾
3. 公共对象用静态声明
4. 等等
