---
title: 面试题汇总CShapeDay3
tags:
  - 面试题汇总
categories:
  - [面试题，C#]
author:
  - nightstardawn
---

# C# Day3

## 1.泛型的约束有哪几种？

**答案：**

6 种

- 值类型约束 T:Struct
- 引用类型约束 T:class
- 公共无参构造约束 T:new()
- 类约束 T:类名
- 接口约束 T:接口名
- 另一个泛型约束 T:U

## 2.什么是闭包？可以举例说明。

**答案：**

```cs
public UnityAction Test()
{
    int i =10;
    UnityAction action =()=>{
        int b =i+1;
    }
    return action;
}
```

定义：闭包是指有权访闻另一个函数作用域中的变量的函数，所以闭包一般都是指的一个函数，创建这种特殊闭包函数的方式往往是在一个函数中创建另一个函数

## 3.内存泄露指的是什么？常见的内存泄露有哪些？

**答案：**

- 内存泄漏指的就是对象超过生命周期后而不能被 GC 回收，一般指不会再使用的引用对象由于某些操作而不能被 GC 垃圾回收，而一直占用着内存
- 更风趣通俗一点的说就是:没用的家伙没有被当成垃圾回收

常见的内存泄漏有

1. 静态引用
2. 不使用的引用对象没有置 null，一直被引用
3. 文件操作时，没有使用 using 或者没有进行 Dispose()
4. 委托或事件注册后没有解出注册
5. 等等

## 4.序列化是什么？常见的序列化是什么？什么时候我们会使用序列化？

**答案：**

1. 序列化是将程序中数据对象转换为可以存储或传输的形式的过程
2. 比如我们常见的序列化方式 xml、Json、2 进制等。就是将内存中的数据按照我们自己定义的规则进行序列化，序列化之后就可以用于存储和传输，当读取和接受数据时，只需要按照对应规则进行反序列化便可得到原始数据
3. 所谓的存储读取和传输接受，其实一般指的就是数据持久化和网络通讯，所以我们经常会在这两块知识点看到序列化反序列化这两个关键词

## 5.请问下列 A、B、C 三处打印的是什么？为什么？

```cs
static unsafe void Main(string[] args)
{
    int test1Value = 10;
    Test1(test1Value);
    Console.WriteLine($"A:{test1Value}");

    int test2Value = 10;
    Test2(&test2Value);
    Console.WriteLine($"B:{test2Value}");

    int test3Value =10;
    Test3(ref test3Value);
    Console.WriteLine($"C:{ test3Value}");
    Console.ReadKey();
}

1 个引用
private static void Test1(int value)
{
    value += 90;
}
1个引用
private unsafe static void Test2(int* value)
{
    *value += 90;
}
1个引用
private static void Test3(ref int value)
{
    value += 90;
}
```

**答案：**
A 是 10，B 和 C 为 100。

- Test1 处参数传递进去后，函数内部的形参 value 是在栈上重新开辟的空间将传入参数的值拷贝到了该空间中，和传入参数没有关系
- Test2 处参数是指针类型，指针是用于存储内存地址的变量，我们传入的是在函数内部改变的是地址中存储的值，所以外部的值得地址&test2Value,test2Value 会随之改变
- Test3 处 ref 关键字，底层逻辑中是将 value 作为 test3Value 的一个别名，他们指向的空间一致，所以 value 改变后，外部的 test3Value 也会改变
