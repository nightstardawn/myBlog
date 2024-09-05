---
title: 面试题汇总CShapeDay8
tags:
  - 面试题汇总
categories:
  - [面试题，C#]
author:
  - nightstardawn
---

# C# Day8

## 1.如果我们想为 Unity 中的 Transform 类添加一个自定义的方法，应该如何处理？

**答案：**

通过 C# 的拓展方法

## 2.请说出 using 关键字的两个作用

**答案：**

- 引入命名空间
  using System；
- 安全使用应用对象
  在 using 后面的()中 声明一个引用对象
  {}这段代码结束后 会自动调用这个对象的销毁
  也可以使用 try catch finally 来达到相同的效果

## 3.C#中 Dictionary 不支持相同键的存储，如何处理一个键对应多个值？

**答案：**
通过键-容器的方式存储

```cs
//一键对多值 List
Dictionary<string, List<Player>> dic = new Dictionary<string, List<Player>>();
//一键对多值 数组
Dictionary<string, player[]>dic2 = new Dictionary<string, player[]>();
//一键对多值 链表
Dictionary<string, LinkedList<Player>> dic3 = new Dictionary<string, LinkedList<Player>>();
//一键对多值 栈
Dictionary<string,Stack<Player>> dic4 = new Dictionary<string, Stack<Player>>();
//一键对多值 队列
Dictionary<string, Queue<Player>> dic5 = new Dictionary<string, Queue<Player>>();
```

## 4.下列代码最终打印的是多少？

```cs
Action action = null;
for (int i = 0;i<10;i++)
{
    action += () =>
    {
        Console.WriteLine(i);
    }
    action();
}
```

**答案：**
全是 10
当委托最终执行时，他们使用的 i，都是 for 循环中声明的 i，此时的 i 已经变成了 10

## 5.上题中的代码，如果我们希望打印出 0~9，应该如何修改代码?

**答案：**
在循环内部声明临时变量，使用临时变量进行输出即可

```cs
Action action = null;
for (int i = 0;i<10;i++)
{
    int j = i;
    action += () =>
    {
        Console.WriteLine(j);
    }
    action();
}
```
