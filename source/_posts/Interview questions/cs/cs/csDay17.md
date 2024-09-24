---
title: 面试题汇总CShapeDay17
tags:
  - 面试题汇总
categories:
  - [面试题，C#]
author:
  - nightstardawn
---

# C# Day17

## 1.父类中定义了一个静态成员属性，有两个子类都继承该父类，请问打印的结果是什么？

```cs
class Father
{
    public static int static_I = 0;
}
class Son1:Father{}
class Son2:Father{}

Son1.static_I = 20;
Son2.static_I = 30;
Console.WriteLine(Father.static_I);
Console.WriteLine(Son1.static_I);
Console.WriteLine(Son2.static_I);
```

**答案：**

都是 30
因为静态成员属于类而不是实例

## 2.泛型父类中定义了一个静态成员属性，有两个子类都继承该泛型父类，请问打印结果是什么？为什么？

```cs
class Father<T>
{
    public static int static_I = 0;
}
class Son1:Father<Son1>{}
class Son2:Father<Son2>{}

Son1.static_I = 20;
Son2.static_I = 30;
Console.WriteLine(Father<int>.static_I);
Console.WriteLine(Father<Son1>.static_I);
Console.WriteLine(Son1.static_I);
Console.WriteLine(Son2.static_I);
```

**答案：**

0,20,20,30
因为静态成员属于类，而不是实例
泛型 T 的变化，会让父类发生"类型"变化
不同类型的泛型实例，他们的静态成员 static_I 是分别存在的，不会互相影响

## 3.使用 C# 制作游戏存档功能，请问有几种做法？（至少说出三种）

**答案：**

- xml
- json
- 2 进制
- 自定义文档结构
- 数据库
- 等等

## 4.C# 中是否可以通过反射获取到内部的私有成员？

**答案：**

可以，
获取成员的相关方法中，可以通过传入参数，指定获取非公共的成员

之中的关键知识是，利用 BindingFlags 枚举
常用枚举类型

- BindingFlags.Public 包括公共成员
- BindingFlags.NonPublic 包括非公共成员
- BindingFlags.Instance 包括实例成员
- BindingFlags.Static 包括静态成员
- BindingFlags.FlattenHierarchy 在层次结构中查找成员
- BindingFlags.IgnoreCase 忽略成员名称的大小写

## 5.在制作游戏存档时，C# 中反射主要可以发挥出哪些作用？

**答案：**

1. 序列化时：动态获取数据结构类信息，可以动态获取字段用于存储
2. 反序列化时：可以通过反射实例化对象，写入数据
3. 结构发生变化时：我们通过反射机制进行判断，多的数据抛弃，少的数据自定义初始化
