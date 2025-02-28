---
title: Unity自带的全局雾效
tags:
  - Shader
  - Shader基础
  - 屏幕后处理
categories:
  - [技术美术, UnityShader，屏幕后处理]
author:
  - nightstardawn
---

# Unity自带的全局雾效

## 一、全局雾效是什么？

全局雾效（Global Fog）
是一种视觉效果，**用于在3D场景中模拟大气中的雾气对远处物体的遮挡**
它通过在场景中加入雾的效果，使得距离摄像机较远的物体看起来逐渐被雾气覆盖，
从而创造出一种朦胧、模糊的视觉效果。

![全局雾效按距离控制](https://s2.loli.net/2024/12/07/2SOZb3TgqF1BCIH.png)
![屏幕后处理效果实现的全局雾效按高度控制](https://s2.loli.net/2024/12/07/54cnyWxHkCtLNZF.png)

Unity当中本身就存在一个全局雾效功能
我们可以在Window—>Rendering—>Lighting窗口中的
Environment 环境页签中进行开启
![全局雾设置位置](https://s2.loli.net/2025/01/16/c9fRWG6ohIHjMwX.png)
## 二、Unity自带的三种雾的计算模式

### 1.Linear（线性）


Linear（线性）计算公式：
f = (end - |d|) / (end – start)

- d代表里摄像机的距离
- start代表雾开始的距离（可控）
- end代表雾最强时的距离（可控）
- 这里的距离都是相对于摄像机的

最终的颜色= （1-f）* 物体的颜色+ f * 雾的颜色

### 2.Exponential（指数）

Exponential（指数）计算公式：
- f = 1 – pow(e,−density∗|d|)
- d代表里摄像机的距离
- e是自然对数的底约等于2.71828；
- density代表雾的浓度（可控）
- 这里的距离都是相对于摄像机的

最终的颜色= （1-f）* 物体的颜色 + f * 雾的颜色

### 3.Exponenttial Squared（指数的平方）

Exponential Squared（指数的平方）的计算公式：
f = 1 –  pow(e,−(density−|d|)²)

- d代表里摄像机的距离
- e是自然对数的底约等于2.71828；
- density代表雾的浓度（可控）
- 这里的距离都是相对于摄像机的

最终的颜色= （1-f）* 物体的颜色+ f * 雾的颜色

## 三、如何响应Unity的全局雾效

如果想要让物体响应Unity自带的全局雾效，我们需要在对应物体的Shader中加入相关的CG代码。

关键的几句CG代码是（创建顶点片元着色器时自带）：
1. 编译指令#pragma multi_compile_fog
2. 内置文件#include “UnityCG.cginc”
3. v2f结构体中加入用于计算雾效坐标信息(通常是计算深度信息)的宏UNITY_FOG_COORDS(数字)
   后面的数字和阴影中的宏一样，前面有几个纹理坐标语义，这里就写几
4. 顶点着色器中加入用于计算雾效数据的宏UNITY_TRANSFER_FOG( v2f结构体, v2f结构体.顶点 );
5. 片元着色器中加入用于应用雾效的宏UNITY_APPLY_FOG(v2f结构体.fogCoord,  颜色);


















