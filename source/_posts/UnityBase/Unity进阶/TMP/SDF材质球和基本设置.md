---
title: SDF材质球和基本设置
tags:
  - Unity客户端
  - Unity进阶
  - TextMeshPro
categories:
  - [Unity客户端, Unity进阶，TextMeshPro]
author:
  - nightstardawn
---

# SDF 材质球和基本设置

## 一、SDF 是什么？

SDF 是有符号距离场(signed Distance Field)的缩写

- 有符号(signed):指的是距离可以为正或负，表示一个点位于边界的内部(负值)还是外部(正值)
- 距离(Distance):表示每个像素点到字符边缘的距离
- 场(Field):指的是整个字体或图形周围的距离值的分布

> SDF 是一种用于高质量文本和图形渲染的技术，尤其适用于缩放或在低分辨率下保持边缘平滑的情况
> 它的本质就是在一个 shader(着色器)中利用 SDF 相关算法规则来染文字
> SDF 技术生成的字体纹理并不是普通的位图，而是基于每个像素到字体边缘的距离值,这些距离值存储在纹理的灰度通道中，代表每个像素到字符边缘的距离信息。
> 然后在渲染时，着色器根据这些距离值动态计算字体的边缘，最终渲染出平滑的字符轮廓。

主要在 TMP 中用于生成和渲染文本，能让字体在任意大小或距离下保持清晰和锐利的效果

## 二、SDF 材质是什么

我们创建的字体资源使用的材质球
本质上就是一个使用了 SDF 相关 Shader 的材质球
利用该 shader 渲染出来的字体效果会更好，并且该 shader 提供了很多可以被配置的参数
我们可以在对应字体的材质球中修改这些参数
从而让我们的字体实现一些更复杂的美术表现效果
因此我们需要学习字体材质球中的这些参数，从而帮助我们利用它来实现我们的表现需求

## 三、SDF 材质球的参数

### 1.基础设置

![ 2024-10-24 145635.png](https://s2.loli.net/2024/10/24/LY5GOujHXoaB1xC.png)

- **Color**
  文本表面颜色，该颜色会和 TMP 组件颜色进行相乘叠加
- **Texture**
  可以为文本添加贴图
  - Tiling 平铺
  - Offset 偏移
  - Speed 移动速度
    可以实现材质滚动效果，可以配合 UV 使用
- **Softness**
  边缘柔和度，可以让文本产生类似于毛边效果的模糊感
- **Dilate**
  拓展，类似于改变粗细的效果

### 2.边缘线

![ 2024-10-24 151844.png](https://s2.loli.net/2024/10/24/iBCaRyfNlwdbGsj.png)

- **Color**
  文本表面颜色，该颜色会和 TMP 组件颜色进行相乘叠加
- **Texture**
  可以为文本添加贴图
  - Tiling 平铺
  - Offset 偏移
  - Speed 移动速度
    可以实现材质滚动效果，可以配合 UV 使用
- **Thickness**
  边缘线轮廓粗细

### 3.阴影

![ 2024-10-24 151840.png](https://s2.loli.net/2024/10/24/FPkABa34ZWuMxpV.png)

- **Underlay Type**
  - None 无阴影
  - Normal 正常标准阴影
  - Inner 反转底图，用原始文本遮罩它
- **Color**
  文本表面颜色，该颜色会和 TMP 组件颜色进行相乘叠加
- **Offset X/Y**
  阴影偏移
- **Dilate**
  拓展，类似于改变粗细的效果
- **Softness**
  边缘柔和度，可以让文本产生类似于毛边效果的模糊感

### 4.照明

#### 1.）斜面设置

![ 2024-10-24 225706.png](https://s2.loli.net/2024/10/24/Gq48Od1HFVIDXbT.png)

- Type
  - Outer Belevl 外斜面
    让字体产生带有倾斜斜面的突起效果
    相当于中间凸出来
  - Inner Belebl 内斜面
    轮廓突起的文本
    相当于向中间凹进去
- Amount 陡峭程度
- Offset 偏移位置
- Width 斜面大小
- Roundness 平滑程度
- Clamp 限制斜面的最大高度

#### 2.）本地照明设置

![ 2024-10-24 225734.png](https://s2.loli.net/2024/10/24/QzqZbP4RY3nxWmp.png)

- Light Angle 光照角度 模拟局部光的角度
- Specular Color 镜面反射的颜色
- Specular Color 镜面反射的强度
- Reflectivity Power 反射强度，值越大越能反应周围环境的颜色
- Diffuse Shadow 漫反射阴影，调整整体的阴影等级，值越高，阴影越强
- Amblent Shadow 环境阴影，调整环境光照水平

#### 3.）凹凸贴图的设置

![ 2024-10-24 231025.png](https://s2.loli.net/2024/10/24/V7ZalYewisofjgI.png)

- Texture 凹凸贴图
- Face 凹凸贴图的程度
- Outline 凹凸贴图对文本轮廓的影响程度

#### 4.）环境设置

![ 2024-10-24 231030.png](https://s2.loli.net/2024/10/24/L9Tj32cxFgqJWzr.png)

- Face Color 立方体贴图对文本的颜色影响，会和文本进行颜色叠加
- Outline Color 立方体贴图对文本的轮廓颜色的影响，会和轮廓进行颜色叠加
- Texture 环境的立方体纹理贴图
- Rotation 旋转环境贴图

### 4.发光

![ 2024-10-24 231941.png](https://s2.loli.net/2024/10/24/29Cf7s83loBgwWq.png)

- Color 发光颜色
- Offset 发光效果中心偏移的位置
- Inner 发光效果向内扩散效果
- Outer 发光效果向外扩散效果
- Power 发光强度

### 5. 调试设置

主要是公布了 Shader 中的一些属性
可以看到该 Shader 中更多的一些属性
具体可以看[官网](https://docs.unity3d.com/Packages/com.unity.textmeshpro@4.0/manual/ShadersDistanceField.html)
![ 2024-10-24 232234.png](https://s2.loli.net/2024/10/24/tH1Rd2uLphDfqjs.png)
