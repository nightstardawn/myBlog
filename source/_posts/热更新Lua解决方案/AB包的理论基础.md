---
title: AB包的理论基础
tags:
  - 程序语言
  - 热更新
  - AssetBundle
categories:
  - [热更新, AssetBundle]
author:
  - nightstardawn
---

# AB 包的理论基础

## 一、AB 包是什么

- 特定于平台的资产压缩包，有点类似压缩文件
- 资产包括:模型、贴图、预设体、音效、材质球等等

## 二、AB 包的作用

1. 相对 Resources 下的资源 AB 包更好管理资源
   ![ 2024-09-04 172418.png](https://s2.loli.net/2024/09/04/UDjmMSYIA8yo3uV.png)
2. 减小包体大小
   1. 压缩资源
   2. 减少初始包大小
3. 热更新
   - 资源热更新
   - 脚本热更新
   - ![ 2024-09-04 172741.png](https://s2.loli.net/2024/09/04/TD2oxzIPWAdOyqZ.png)
