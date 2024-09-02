---
title: 面试题汇总CShapeDay6
tags:
  - 面试题汇总
categories:
  - [面试题，C#]
author:
  - nightstardawn
---

# C# Day6

## 1.C#中如何让自定义容器类能够使用 for 循环遍历?(通过 类对象[索引] 的形式遍历)

**答案：**
通过在类对象中实现索引实现

## 2.C#中如何让自定义容器类能够使用 foreach 循环遍历?

**答案：**
通过在类对象中实现迭代器

- 传统的方式
  1. 继承 IEnumerator、IEnumerable 两个接口
  2. 实现其中 GetEnumerator 方法 、Current 属性、MoveNext 方法
- 语法糖的形式
  利用 yield return 语法糖，实现 GetEnumertor 方法即可完成迭代器的实现

## 3.C#中接口的作用时什么？说说你的理解

**答案：**

用于建立行为的继承关系，而不是对象
不同对象有相同的行为时，我们就可以利用接口对不同对象的行为进行整合

## 4.Unity 中那些功能使用了 c#反射的功能？

**答案：**

1. Inspector 窗口中显示的内容
2. 预设体文件
3. 场景文件
4. Unity 中的各种特性
5. 等等

## 5.下列代码运行后，在堆上会分配几个房间？

```cs
string str = "123";
string str2 = "123";
string str3 = "1234";
```

**答案：**
2 个房间
"123"一个
"1234"一个
![在VS跟踪示意图](https://s2.loli.net/2024/08/19/fbhvIYlKUBjaDHF.png)
