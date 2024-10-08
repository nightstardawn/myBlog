---
title: Unity面试题汇总Day9
tags:
  - 面试题汇总
categories:
  - [面试题，Unity]
author:
  - nightstardawn
---

# Unity Day9

## 1.Unity 判断两个 2D 矩形是否相交，有几种方式(至少两种)。

**答案：**

1. 使用 Unity 物理系统进行碰撞检测
2. 使用 Unity 中范围检测相关 api
3. 自己写算法检测(AABB 算法、OBB 算法)

## 2.Unity 中制作角色的连招功能，在制作状态机时我们一般如何处理？

**答案：**

1. 状态机可以添加一个 Trigger 类型和 Int 类型
   Trigger 条件主要用于触发动作，Int 条件主要用于连招计数判断
2. 逻辑上，当攻击输入时，我们需要触发动作，并进行攻击计数
   每次按按键时，都应该重新进行攻击计数延迟清零

## 3.某一时刻进行伤害检测，我们应该怎么做？(请说出两种做法)

**答案：**

1. 添加动画事件
2. 动画一开始，进行延迟触发，延迟时间为想要触发伤害的时间(使用协程 也可以)

## 4.Unity 中如何制作自动寻路逻辑？(两种)

**答案：**

1. Unity 自带的网格寻路系统
2. 自定义寻路算法(例如 A 星寻路算法)

## 5.游戏编辑器 (例如 角色编辑器、关卡编辑器、地图编辑器)的本质是什么？

**答案：**

数据的图形化编辑工具
