---
title: 面试题汇总Day5
tags:
  - 面试题汇总
categories:
  - [面试题，Unity]
author:
  - nightstardawn
---

# Unity Day5

## 1.两个四元数相乘有什么作用?|四元数乘以向量有什么作用?

**答案：**

- 两个四元数相乘：角度旋转
- 四元数乘以向量：向量旋转

## 2.下面的小球是否被渲染，是否产生 DrawCall？

![图2](https://s2.loli.net/2024/08/16/mDei6SZuL8TbyoR.png)
**答案：**
不会被渲染 ，不会产生 DrawCall
Unity 本身有摄像机视锥休剔除，也就是不会显示完全位于视锥体之外的游戏对象那么小球就不会进行渲染 ，也不会提交数据给 GPU，也就不会产生 DrawCall

## 3.在没有使用遮挡剔除的情况下，图中的 A 和 B 都是默认材质，图中的小球最终是否会被渲染？是否产生 DrawCall？

![图 3](https://s2.loli.net/2024/08/16/eTpa8zFglN6JGId.png)
**答案：**

- 最终不会被渲染，标准材质存在深度测试，小球在立方体后方，不会通过深度测试，所以不会被染
- 会产生 DrawCall，既然都深度测试了，那么肯定是存在 DrawCall 的深度测试发生在片元着色器处理之后，GPU 会对每个片元执行深度测试来决定遮挡关系，决定是否被渲染

## 4.如果不考虑 IOS 平台，只在 Windows 和 Android 平台上发布游戏，如何在不使用第三方热更新方案的前提下实现热更新功能

**答案：**
C#的反射
可以通过热更 DLL 文件的形式，加载程序集(dll)利用反射执行热更 DLL 包中的逻辑

## 5.Unity 中如何调试排查 Android 上运行的项目问题

**答案：**

1. 如果需要进行断点调试通过数据线链接运行项目的 Android 没备发布时开启了

   - Development Build 开发模式构建
   - Autoconnect Profiler 自动连接分析器 Script Debuggins 脚本调试
   - Wait For Managed Debugger 等待托管调试器
   - 等选项
     然后只需要 Build and Run 既可以利用 Unity 的 Profler 性能剖析器窗口排查性能问题并且还可以进行断点调试

2. 如果只是获取一些打印调试信息可以利用 Unity2019.4 及其以上版本提供的 Android Logcat 具获取信息 Unity2019.4 以下的版本，可以使用 Android 的 ADB(安卓调试桥)工具
3. 如果需要获取设备输入信息
   可以利用 Unity Remote 来测试移动设备的输入相关逻钼
