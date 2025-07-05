---
title: GPU Usage
tags:
  - 性能优化
  - 性能分析工具
categories:
  - [性能优化, 性能分析工具]
author:
  - nightstardawn
---

# GPU usage

## 一、GPU Usage是什么

GPU Usage(使用率)
是 Unity Profiler 中最重要的性能分析模块之一用于 分析 GPU(图形处理器)在一帧中在哪些渲染阶段上花了多少时间

游戏开发中会造成GPU开销的主要有:
1. **几何处理**:模型顶点相关处理
2. **光照计算**:阴影、反射等
3. **渲染输出**:shader处理、对象渲染、游戏画面显示
4. **屏幕后处理Shader部分**:屏幕画面特殊效果(模糊、黑白、景深等等)
5. **纹理处理**:采样、解压缩、贴图操作等
6. **特效处理**:粒子、流体、烟雾等等等等

一般项目出现卡顿、掉帧问题、图像问题、发热、掉电快等情况可以着重排査GPU和CPU使用情况它们的异常表现几乎一致

根本原因通常是计算压力过大GPU和CPU计算的区别在于:
- CPU专注于 游戏逻辑 处理相关(物理、脚本、AI、动画控制等等)
- GPU专注于 图形渲染处理相关(shader、模型绘制、光照、特效、后处理等等)

注意：

1. GPU Usage 的数据收集 会影响性能表现
2. 会禁用 GPU Jobs (图形工作线程优化)
3. 并不支持显示详细的URP 和 HDRP 相关数据

## 二、GPu各个参数功能的含义及其作用

### 1.警告提示

![警告提示](https://s2.loli.net/2025/07/03/D9SaZk8HPwmtgAy.png)

采集 GPU Profiler 数据会禁用图形作业、增加性能开销，并降低 CPU 模块的准确性
</br>如果你不需要这些数据，请关闭此模块，HDRP 和 URP 渲染器当前不受支持:
</br>如果启用了这些渲染器，Profler 将不会显示大多数 GPU 标记,

注意:
1.Graphics Jobs 是 Unity 用于加速清染流程的多线程技术，开启 GPU Profiler 会关闭它，影响性能测试的真实性。
2.在 使用 URP(通用澶染管线)或 HDRP(高清澶染管线) 时，GPU 模块不会显示详细的 GPU时间采样(比如各个 pass、shader 的耗时)，所以数据不完整

建议:
</br>如果你只关心 CPU 性能或者使用 SRP(如 URP/HDRP)，可以暂时关闭 GPU Profiler。
</br>除非你在测试传统内置渲染管线的 GPU 性能。
</br>对于SRP(URP/HDRP)我们可以使用Rendering Debugger进行性能检测(以后再讲解URP先关知识时，再讲解)

### 2.分析窗口

![分析窗口](https://s2.loli.net/2025/07/03/UeR8ZqKDoLmsT57.png)

- **Opaque**; 内置渲染管线清染 不透明对象的时间
- **Transparent**;内置渲染管线染 透明对象的时间
- **Shadows/Depth**:内置渲染管线渲染 阴影贴图/深度贴图 的时间(比如阴影贴图生成、摄像机深度图)
- **Deferred Geometry**:内置廷迟渲染管线处理 延迟几何通道的时间;用于延迟渲染管线，写入GBuffer 阶段
- **Deferred Lighting**:内置延迟滀染管线处理 延迟光照通道的时间;从 GBuffer 读取信息并执行光照计算
- **PostProcess**:内置渲染管线处理 屏幕后期处理效果的时间
- **Other**:处理可编程渲染管线等其他事务的渲染时间(比比如URP或者HDRP)，还有其他无法分类的 GPU 开销，如一些插件、系统调用等

注意:
</br>该模块并不是支持所有的平台调试
</br>对于不支持的平台我们只有在特定平台上用特定工具调试比如IOS使用xcode的GPU Frame Debugger

使用建议:
</br>我们主要就是根据具体的数据表现，特别是在内置渲染管线中可以通过这些数值明确的知道是那一块演染相关出现的性能问题，然后针对性的优化即可

### 3.两种视图

![两种视图](https://s2.loli.net/2025/07/03/rtuHBdYMoSi9WU5.png)

- Hierarchy:层级视图
  </br>最常用的分析视图，展示 GPU 所做工作的树状结构(父子关系)
  </br>将 GPU 操怍按 渲染流程的逻辑顺序组
  </br>适用于:识别 哪个阶段(如阴影、后处理)消耗了 GPU 时间分析 GPU 执行的整体结构
- Raw Hierarchy:原始层级视图
  </br>更接近 GPU 实际命令流的视图，展示 未经整理的 GPU 操作列表所有 GPU 调用按记录顺序直接列出(无父子结构)
  </br>没有逻辑分组，也没有折叠结构，像一张"流水账"
  </br>精细诊断性能问题时，确认某一个 draw call 或 GPU pass 造成了卡顿直看某条 GPU 调用命令具体花了多长时间

Hierarchy视图

![Hierarchy视图](https://s2.loli.net/2025/07/04/cFiLRm6TfrODGwK.png)

- **Overview**:
  </br>显示调用路径或函数名的层级结构(如 PlayerLoop > RenderPipelineManager.DoRenderLoop)
- **Total**:
  </br>当前函数及其所有子函数占据本帧 GPU 总耗时的百分比(%)
- **DrawCalls**:
  </br>该函数在本帧触发的這染调用次数(Draw Call 的数量)
- **GPU ms**:
  </br>当前函数在 GPU 上的耗时(单位:亳秒)


- **No Details**:
  </br>只显示基本数据(默认)
- **Related Data**:
  </br>显示与当前函数相关的纹理、材质、shader 等资源信息
- **Calls**:
  </br>显示底层的绘制调用(如 DrawMesh、Blit)

## 三、GPU对于我们的意义

1. 分析掉帧来源
   </br>如果帧时间高于 16ms(60FPS)或其它预定帧率，看看 GPU 有没有占用太多时间
2. 优化瓶颈定位
   </br>例如发现 PostProcess(屏幕后处理)占用 5ms，就可以尝试禁用或简化后处理
3. DrawCall 分析
   </br>搭配右侧 Hierarchy 面板査看哪些方法触发了多少 DrawCal1，以及 GPU 耗时
   












