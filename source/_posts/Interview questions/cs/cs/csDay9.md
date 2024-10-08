---
title: 面试题汇总CShapeDay9
tags:
  - 面试题汇总
categories:
  - [面试题，C#]
author:
  - nightstardawn
---

# C# Day9

## 1. 内存中堆和栈的区别？

**答案：**

堆和栈是操作系统进程占用内存空间的两种管理方式

- 栈：由操作系统自动分配释放，存放函数的参数值 ，局部变量值，栈中数据的生命周期随着函数的执行完成而结束
- 堆：一般由程序员分配释放，如果开发人员不释放，程序结束时会被操作系统回收
  (C# 中 托管堆内存 会由 C# 帮助我们管理，存在 GC 垃圾回收机制)

具体见：https://blog.csdn.net/K346K346/article/details/80849966/

## 2.TCP 协议和 UDP 协议的区别？

**答案：**

- 连接方面：TCP 面向连接，UDP 无连接
- 是否可靠：TCP 可靠(无差错，不丢失，不重复，按顺序)，UDP 不可靠
- 传输效率：TCP 相对 UDP 较低
- 连接对象：TCP 一对一，UDP n 对 n

## 3.TCP 协议的可靠性是如何达到的？

**答案：**

TCP 协议是通过 检验和确认应答信号、重发机制、连接管理、流量控制、拥塞控制等手段达到可靠的

## 4.内存抖动指的是什么？如何避免内存抖动？

**答案：**

1. 内存抖动指短时间内有大量的对象被创建或者被回收的现象
   频繁的内存抖动会造成 GC 频繁运行，造成卡顿
2. 避免方式
   - 对象池
   - 享元模式
   - 等等

## 5.buff 系统中，如何利用一个 byte，记录多种 buff 状态标识？

**答案：**
一个 byte 有 8 位，我们可以让每一位标识一种状态，0 代表有，1 代表无
例如：

```cs
byte buff = 0；
0000 0000 无 buff
0000 0001 中毒
0000 0010 灼烧
0000 0100 回血
```

- 当状态被添加时，进行或(|)运算

  ```cs
  buff | 灼烧buff = 0000 0010
  buff | 中毒buff = 0000 0011
  ```

- 当状态被移除时，进行异或(^)运算
  ```cs
  buff ^ 中毒buff = 0000 0011 ^ 0000 0001 = 0000 00010
  ```
