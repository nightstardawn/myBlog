---
title: 面试题汇总CShapeDay4
tags:
  - 面试题汇总
categories:
  - [面试题，C#]
author:
  - nightstardawn
---

# C# Day4

## 1.请说明字符串中 string str = null , tring str ="" , string str = string.Empty 三者的区别?

**答案：**

- str=null, 在堆中没有分配内存地址
- str = "" 和 string.Empty 一样都是在堆内存中分配了空问，里面存储的是空字符串
- 而 string.Empty 是一个静态只读变量

## 2.C# 重载运算符，重载 == 和!=，以及万物之父 Object 基类中的虚方法 virtual bool Equals(Object obj)对于我们的意义是什么?

**答案：**
为了判断两个对象的非引用地址相等
我们可以选择 使用 重载运算符 == 和 != 或者重写 Equals 方法来自定义判断两个对象是否相等
如果想保留原有的引用地址相等判断，那么一般我们选择重写 Equals 方法

## 3.在开发时，对 string 和 StringBuilder 我们应该如何选择

**答案：**

- string 在每次拼接时都会产生垃圾
- StringBuilder 在拼接时，是在原空问中进行修改，不会产生垃圾

所以当字符串需要频繁修改拼接时，我们使用 StringBuilder

## 4.请简要说明 .Net 跨语言原理

**答案：**
.Net 制定了了 CLI 公共语言基础结构的规则
只要是按照该规则设计的语言在进行 .Net 相关开发时编译器会将源代码(C#、VB 等等)编译为 CIL 通用中问代码。
也就是说不管什么语言进行开发，最终都会统一规范变为中间代码最终通过
CLR(公共语言运行时或者称为 .Net 虚拟)将中问代码翻译为对应作系统的原生代码(机器码)
在操作系统(windows)上运行

## 5.请简要说明 .Net 跨平台原理

**答案：**
由于 .Net Framework 中利用 CLI 和 CLR 实现了跨语言
CLR 主要起到一个翻译、运行、管理中问代码的作用 Net Core 和 Mono 就是利用了 CLR 的这一特点，为不同操作系统实现对应 CLR(公共语言运行时或 .Net 虚拟机)
那么不同操作系统对应的 CLR 就会将 IL 中问代码翻译为对应系统可以执行的原生代码(机器码)
