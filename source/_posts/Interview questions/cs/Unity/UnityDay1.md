---
title: Unity面试题汇总Day1
tags:
  - 面试题汇总
categories:
  - [面试题，Unity]
author:
  - nightstardawn
---

# Unity Day1

## 1.Unity 中点乘和又乘对于我们来说的作用是什么?

**答案：**
点乘作用

1. 判断对象的方位
2. 计算两向量之间的夹角

叉乘作用

1. 获取一个平面的法向量
2. 得到两向量之间的左右位置关系

## 2.Unity 中多线程执行下面哪些代码会报错?

**A.Application.persistentDataPath
B. File.Exists("文件名”)
C.transform.Translate
D.Object.Destroy(对象)**

**答案：**
A、C、D
UnityEngine 命名空间中相关类基本都不能被 Unity 中多线程使用

## 3.Application.streamingAssetsPath 和 Application.persistentDataPath 两个路径有何区别?对于我们的意义是什么?

**答案：**
Application.streamingAssetsPath 只读 Application.persistentDataPath 可读可写
Application.streamingAssetsPath 适合放置一些默认 2 进制配置文件
Application.persistentDataPath 用于处理数据持久化，或作为热更新下载内容的存放目录，因为它可读可写

## 4.简述 Unity 中协程的原理

**答案：**
Unity 中的协同程序分为两部分

1. 协程函数本体(迭代器函数)
2. 协程调度器(协程管理器)

协程利用迭代器函数的分步执行的特点加上
协程调度器对迭代器函数们进行统一管理根据迭代器函数的返回值来决定下一次执行函数逻辑的时间点从而实现逻辑分时分步执行的目的

## 5.Unity 底层如何处理 C#代码?

**答案：**

1. Mono
   ![Mono](https://s2.loli.net/2024/07/31/IgrCe2EZvnY79oM.png)
1. IL2cpp
   ![IL2cpp](https://s2.loli.net/2024/07/31/cOVzmj2nLJWolG6.png)
