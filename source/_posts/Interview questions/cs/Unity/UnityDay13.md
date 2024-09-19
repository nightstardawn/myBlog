---
title: Unity面试题汇总Day13
tags:
  - 面试题汇总
categories:
  - [面试题，Unity]
author:
  - nightstardawn
---

# Unity Day13

## 1.Unity 中，有时候在 GameObject.Instance 的时候有明显卡顿，该怎么解决？

**答案：**
通过 Unity 的 Profiler 窗口排查卡顿的原因，一般有以下几种可能

1. 加载过大资源造成的卡顿
   解决方法：
   1. 预加载资源
   2. 简化资源(修改图片大小、减少模型顶点、面数、压缩优化资源等)
   3. 异步加载资源(分帧加载)
2. 对象挂载的脚本中初始化耗时
   解决方法
   1. 减少序列化或反序列化信息
   2. 优化初始化相关逻辑，提前初始化，分帧初始化等

## 2.在 Unity 中 AssetBundle 的压缩方式有不压缩、LZMA、LZ4 三种，请问 LZMA 和 LZ4 有什么区别？

**答案：**

- LZMA：
  压缩算法为 Lempel-Ziv-Markov chain-ALgorithm
  压缩包最小(压缩率更高)，但是解压过程较慢，耗时长(因为它需要进行更多的解压操作)
- LZ4：
  压缩算法为 Lempel-Ziv 4
  压缩包较大(压缩包更低)，但是不要求完整的数据包就可以解压缩，无需压缩完整压缩包，解压时间快(几乎就可以立即解压 AsssetBundle)

## 3.Unity 中 DrawCall、Batches、SetPassCalls 的意思是什么？

![ 2024-09-19 154553.png](https://s2.loli.net/2024/09/19/ABRkyvNbG6iHga2.png)
**答案：**

- DrawCall：
  表示渲染请求的数量，(每个 DrawCall 都会引起一次从 CPU 到 GPU 的数据传输)
  直接影响渲染性能，因为它决定了 CPU 和 GPU 之间的通信次数
  减少 DrawCall 的数量通常是优化性能的关键之一
  可以通过批处理(Batching)来合并多个物体为一个 DrawCall 来实现
- Batches：
  是一种将多个物体合并一个 DrawCall 的渲染优化技术
  它将多个相似的物体合并成一个 DrawCall，从而减少 CPU 到 GPU 的数据传输和渲染开销
  可以使用静态批处理、动态批处理和 GPUInstancing 等技术来进一步优化 Batch
- SetPassCall：
  渲染 Pass 的数量
  移动平台中尽量减少 Shader 中的 Pass 数量可以提升性能

## 4.UnityShader 中，深度测试是在做什么？

![深度测试流程图](https://s2.loli.net/2024/07/25/Itiv4AKkgNFnLxT.png)
**答案：**
深度测试时用于确定哪些像素应该被绘制到屏幕上，并决定它们的可见性。
深度测试的主要目的时解决遮挡关系，确保前面的对象覆盖后面的对象，从而正确呈现场景

## 5.Unity 中，某片元通过了深度测试，但是没有开启深度写入，该片元的颜色是否写入到颜色缓冲区？

**答案：**
会写入颜色缓冲区
因为深度写入和颜色写入时两个独立操作
只要通过深度测试，不管是否写入深度缓冲区
该片元的颜色信息都会写入到颜色缓存区中
