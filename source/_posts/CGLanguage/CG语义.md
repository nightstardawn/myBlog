---
title: CG语法的语义
tags:
  - 程序语言
  - CG语言
  - 语义
categories:
  - [程序语言, CG语言]
author:
  - nightstardawn
---

# CG 语法的语义

## 一、语义的作用

CG 语法中提供了许多语义，这种特殊关键字用于修饰函数中的传入参数和返回值
主要作用是让 Shader 知道从哪里读取数据，并且把数据传输到哪里
让我们在 Shader 开发中获取想要的数据，并且可以把数据传输出去

**注意：** Unity 只支持 CG 中部分语义

## 二、常用语义

### 1.应用阶段->顶点着色器

意思：应用阶段传递给模型数据给顶点着色器时 Unity 支持的语义
一般才顶点着色器回调函数的传入参数中应用
|语义代码|意义|类型|
|:---:|:---:|:---:|
|POSITION|模型空间中的顶点位置|通常是 float4 类型|
|NORMAL|顶点法线|通常是 float3 类型|
|TANGENT|顶点切线|通常是 float4 类型|
|TEXCOORDn|顶点的纹理坐标|通常是 float2 或 float4 类型|
|COLOR|顶点颜色|通常是 fixed4 或者 float4 类型|

纹理坐标：也称 UV 坐标 表示顶点对应纹理图像上的位置

### 2.顶点着色器->片元着色器

意思：顶点着色器传递数据给片元着色器时 Unity 支持的语义
一般在顶点着色器回调函数的返回值中应用
|语义代码|意义|
|:---:|:---:|
|SV_POSITION|裁剪空间中的顶点坐标（必备）|
|COLOR0|通常用于输出第一组顶点的颜色（非必须）|
|COLOR1|通常用于输出第二组顶点的颜色（非必须）|
|TEXCOORD0~TEXCOORD7|通常用来输出纹理坐标（非必须）|

### 3.片元着色器输出阶段

意思：片元着色器输出时 Unity 支持的常用语义
一般在片元着色器回调函数的返回值中应用
|语义代码|意义|
|:---:|:---:|
|SV_Target|输出会存储带渲染目标中|

## 三、更多语义

HLSL 语义[汇总](https://learn.microsoft.com/zh-cn/windows/win32/direct3dhlsl/dx-graphics-hlsl-semantics?redirectedfrom=MSDN)
