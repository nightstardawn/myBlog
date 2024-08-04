---
title: 面试题汇总CShapeDay2
tags:
  - 面试题汇总
categories:
  - [面试题，C#]
author:
  - nightstardawn
---

# C# Day2

## 1.请说说你认为 C#中 ==和 Equals 的区别是什么?

**答案：**

1. == 是运算符，Equals 是万物之父 Object 中的虚方法，子类可重写
2. Equals 一般在子类中重写后用于比较两个对象中内容是否相同
   ==在没有运算符重载的前提下时，引用类型用于比较地址;值类型用于比较值是否相同
3. 运算效率不同，一般 Equals 没有==效率高，因为一般 Equals 比较内容比较多

## 2.浅拷贝和深拷贝的区别?可以举例说明

**答案：**

1. 浅拷贝
   只复制对象的引用地址
   两个对象指向同一内存地址，修改其中一个另一个也会随之变化
2. 深拷贝
   将对象和值赋值过来，两个对象修改其中任意值都不会影响对方

举例:
比如引用对象 A 和引用对象 B 让 A=B，就是浅拷贝，此时 A、B 的引用地址相同，改 A 中内容，B 也变
如果想要深拷贝，简单处理就是 new 包括对象中的成员

## 3.下面两种获 10000 个数的方式，哪种效率更高?为什么

```cs
//方式一
List<int> list = new List<int>();
for(int i=0;i<10000;i++)
{
    list.Add(i);
}
//方式二
float[] array = new float[10000];
for(int i=0;i< array.Length; i++)
{
    array[i] = i；
}
```

**答案：**
方式 2 的效率更高
因为 List 本质是数组，我们通过 Add 往 List 中添加元素时，会不断的触发扩容扩容会带来内存和性能上的消耗
内存方面:每次扩容会产生垃圾
性能方面::每次扩容会进行“搬家”(老数组中内容存入新数组中)

## 4.请说出以以下代码 1.A 处和 B 处谁先打印?2.A、B 出打印的 ǐ 值分别是多少?

```cs
static void Main(string[] args)
{
    int i = GetInt();
    Console.WriteLine($"第A处i= {i}");
}
static int GetInt()
{
    int i = 10;
    try
    {
        return i;
    }
    finally
    {
        i = 11;
        Console.WriteLine($"第B处i={i}");
    }
}
```

**答案：**
1.B 处先打印，A 处后打印
2.A 处 i=10，B 处 i= 11
考点:finally 的执行顺序

## 5.请问 A、B 两处 i 的值为多少?

```cs
class Test
{
    public int i= 10;
}
static void Main(string[] args)
{
    Test t= Getobj();
    Console.WriteLine($"第A处i={t.i}");
}

static Test Getobj()
{
    Test t= new Test();
    try
    {
        return t;
    }
    finally
    {
        t.i = 11;
        Console.WriteLine($"第B处i={t.i}");
    }
}
```

**答案：**
A、B 两处都为 11

考点:

1. finally 的执行顺序
2. 值和引用赋值表现的区别
