---
title: Unity面试题汇总Day7
tags:
  - 面试题汇总
categories:
  - [面试题，Unity]
author:
  - nightstardawn
---

# Unity Day7

## 1.Unity 中动态加载资源的方式有哪些？

**答案：**

1. Resource 类中相关方法加载 Resources 文件夹下的资源
2. AssetBundle 类中或者 Addressables 类中的相关方法加载 AB 包中的资源
3. WWW 类中或者 UnityWebRequest 类中的相关方法加载本地或远端资源
4. C# 原生的一些文件加载相关 File 类 FileStream 类等等

## 2.Unity 中光照贴图的作用？

**答案：**

在移动平台上(或者配置较低的设备上)使用实时光源是非常消耗性能的
我们可以使用光照贴图，预先将环境光烘培到贴图上，可以减少性能的消耗



## 3.场景中有两个点连成了一条线，想要旋转这条线，应该怎么做？

**答案：**
两点相减得到一条向量，向量乘以旋转的四元数即可

## 4.LOD(多细节层次)和 MipMap(纹理图)的作用是什么？

**答案：**
优化性能
从不同距离渲染对象时，使用的时质量不同打的模型(LOD)和贴图(MipMap)。一般情况下时越远模型面数越低，图片越小

## 5.游戏开发中，客户端和服务端交互数据，程序中常用的方式是什么？

**答案：**

- 消息数据：Socket 和 HTTP
- 文件数据：FTP 和 HTTP
