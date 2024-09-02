---
title: 面试题汇总Day6
tags:
  - 面试题汇总
categories:
  - [面试题，Unity]
author:
  - nightstardawn
---

# Unity Day6

## 1.Unity 中和 Awake 和 Start 两个生命周期函数，分别在什么时候调用？

**答案：**

- Awake:运行时，
  1. 当脚本被动态添加到对象上时立即被调用
  2. 当对象被实例化时，依附它的脚本会立即调用 Awake
  3. 类似于构造函数
- Start：第一次 Update 之前被调用

## 2.Unity 场景上有多个对象，分别挂在了 n 个对象，我们如何控制不同脚本间生命周期 Awake 的执行先后顺序？

**答案：**

1. 通过选中脚本文件，点击 Inspector 窗口右上角的 Exeution Order（执行顺序）按钮
2. 打开 Project Setting 窗口，选择 Script Execution Order 选项

通过这两种方法我们可以打开脚本执行顺序窗口
在其中我们可以自己设置自定义的脚本执行顺序
![脚本执行顺序窗口](https://s2.loli.net/2024/08/19/w4kiHP6cerjVdGF.png)

## 3.在 Unity 中使用指针需要进行什么操作？

**答案：**

1. 需要在 PlayerSetting 中的 OtherSetting 中勾选 Allow'unsafe'code 选项
2. 使用指针必须在 unsafe 修饰的代码块中

## 4.Unity 中的协同程序中 yield return 不同内容，代表不同的涵义，请说出下列 yield return 的含义？

- yield return 数字；
- yield return null;
- yield return new WaitForSeconds(数字);
- yield return new WaitForFixedUpdate();
- yeild return new WaitForEndFrame();
- yeild break;
  **答案：**
  |代码|含义|
  |---|---|
  |yield return 数字;|下一帧执行|
  |yield return null;|下一帧执行|
  |yield return new WaitForSeconds(数字);|等待指定秒数后执行|
  |yield return new WaitForFixedUpdate();|等待下一个固定物理帧更新时执行|
  |yeild return new WaitForEndFrame();|等待摄像机和 GUI 渲染完成后执行|
  |yeild break;|跳出协程|

## 5.Unity 协同程序进行异步加载时，底层是否使用多线程？

**答案：**
可能会
协同程序的原理是分时分布完成指定逻辑
在其中某一步中，是可以使用多线程来完成某些加载操作的，多线程加载完后，再进入协同程序的下一步继续执行
