---
title: 面试题汇总CShapeDay5
tags:
  - 面试题汇总
categories:
  - [面试题，C#]
author:
  - nightstardawn
---

# C# Day5

## 1. 以下的代码谁的效率更高

```cs
//代码一
List<int>list = new List<int>();
for(inti=0;i<50;i++)
{
    list.Add(i);
}
//代码2:
List<int>list2 = new List<int>(50);
for(int i=0;i<50;i++)
{
    list2.Add(i);
}

```

**答案：**
代码 2 的效率更高
因为 List 的本质是数组，在初始化时，如果不默认为其指明分配多少容量扩容会带来效率的降低和垃圾的产生效率的降低,从旧数组到新数组的搬家垃圾的产生:每次扩容时，就数组就变成了垃圾

## 2.数组和链表的区别？

**答案：**

1. 存储结构不同

   - 数组是顺序存储结构，在内存中是连续存储的
   - 链表是链式存储结构，在内存中是非连续存储的

2. 访问效率不同
   - 数组由于是顺序存储，通过下标访问，访问效率高
   - 链表由于是非连续存储，我们想要获取其中某一元素，需要从头或尾遍历，效率低
3. 插入、删除效率不同
   - 数组由于是顺序存储，在插入和删除时需要整体移动数组中的大部分元素，效率低
   - 链表由于是链式存储，在插入和删除时，效率高
4. 越界问题
   - 数组由于是顺序存储，存在越界风险，声明时容量是固定的，如果不处理扩容逻辑
   - 链表由于是链式存储，无越界风险

## 3.C# 中的 Action 和 Func 是什么? | Unity 中的 UnityAction 是什么? | 他们有什么区别?

**答案：**
Action 和 Func 是 System 命名空间下 C#为我们提供的两个写好的委托

- Action 本身是一个无参无返回值的委托对应的 Action<>泛型委托支持最多 16 个参数
- Func 本身是一个无参有返回值的委托对应的 Func<>泛型委托支持最多 16 个参数，并且有返回值

UnityAction 是 UnityEngine.Events 命名空间下 Unity 为我们提供的写好的委托

- UnityAction 本身是一个无参无返回值的委托对应的 UnityAction<>泛型委托支持最多 4 个参数

## 4.请问下列代码，最终的打印结果是什么?

```cs
public struct Record
{
    public int id;
    public string name;
    public int[] children;
}

public void Dosomething(Record record)
{
    record.id =6;
    record.name = "Bob";
    record.children[0]=7;
}

var record = new Record();
record.name ="Alice";
record.children =new int[]{1,2,3};
Dosomething(record);
Debug.Log(string.Format("{0}-{1}-{2}", record.id, record.namejgmpeeora:Cildren[0]));
```

**答案：**
0-Alice-7

1. 值和引用的区别
2. 特殊引用类型 string
3. 结构体中的引用成员

## 5. 网络游戏开发中，网络传输数据的基本流程是什么?

**答案：**
答案

- 客户端将自定义定义类对象数据**序列化**为二进制数据发送给服务端
- 服务端将收到的二进制数据**反序列化**为对应的类对象进行逻处理

如果是服务端发送给客户端的消息也是同理
