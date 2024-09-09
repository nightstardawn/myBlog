---
title: Unity面试题汇总Day8
tags:
  - 面试题汇总
categories:
  - [面试题，Unity]
author:
  - nightstardawn
---

# Unity Day8

## 1.Unity 中如何将本地坐标转化为世界坐标？

**答案：**

1. 用本地坐标加上父对象相对世界的坐标(多个父子关系，不停向上加即可)
2. 利用 Transform 中的 TransformPoint 方法

## 2. Unity 中如何计算两个向量之间的夹角，请说出两种方式

**答案：**

1. 利用 Vector3.Dot 计算出方向向量点乘结果，再通过 Mathf.Acos 反三角函数算出弧度，再转化为角度
2. 利用 Vecto3.Angle 的 api

## 3.请说出 UGUI 中两种处理异形按钮的具体方法

**答案：**

1. 通过子对象的拼凑
2. 自带的像素检测阈值
   1. 将异形按钮的判断的图片的可读写打开
      ![_20240906120222.png](https://s2.loli.net/2024/09/06/luopTrgtE1AF27M.png)
   2. 通过代码控制 Image 组件上的阈值改变
      alpha 阈值指定像素必须具有最小的 alpha，以便事件被视为监听
      `img.alphaHitTestMinimumThreshold = 0.1f`

## 4.请说出 Unity 中如何进行数据持久化，至少 5 种

**答案：**

- PlayerPrefs
- json
- xml
- 2 进制文件存储
- 数据库存储(本地、远端、通过服务器存储到数据库)

## 5.Unity 种如何控制渲染优先级？(谁先渲染，谁后渲染，分情况回答)

**答案：**

1. 不同摄像机渲染时
   摄像机深度(Camera depth) 控制优先级
2. 相同摄像机时
   排序层(Sorting Layer) 控制优先级
3. 相同摄像机，相同排序层时
   层中顺序(Order in Layer) 控制优先级
4. 以上都相同时
   Shader 中 RenderQueue(渲染队列) 控制优先级
