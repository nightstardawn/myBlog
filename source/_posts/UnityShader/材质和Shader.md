---
title: 材质和Shader
tags:
  - Shader
categories:
  - [技术美术, UnityShader]
author:
  - nightstardawn
---

# Lesson1 认识材质和 Shader

## 一、Unity shader 和 Sharder 的区别

![Unity shader 和 Sharder 的区别](https://s2.loli.net/2024/07/24/RrbufAhWxJ5mOpC.png)

## 二、Unity 中材质和 Shard

![Unity中材质和Shard](https://s2.loli.net/2024/07/24/jvFRUqVd49EeLul.png)

## 三、创建 Shader

在 Inspector 窗口中右键 创建 Shader

### 1. **Shader 类型**

1. _Standard Surface Shader_（标准曲线着色器）
   包含标准光照模型的表面着色器
2. _Unlit Shader_
   不包含光照的基本顶点/片元着色器
3. _Imange Effect Shader_
   用于实现屏幕后处理效果的基本模板
4. _Compute Shard_
   利用 GPU 并行计算一些常规渲染流水线无关的内容
5. _Ray Tracing Shader_
   用于实现光线追踪效果的着色器
   #### **注：**
   之后学习的重点主要是顶点/片元着色器
   即：**Unlit Shader**

### 2.shader 参数

这里以 Surface Shader 为例
![Surface Shader参数](https://s2.loli.net/2024/07/24/3QicYoOnzMZFDvg.png)
