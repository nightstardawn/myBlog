---
title: LinkedList
tags:
  - 程序语言
  - C#语言
categories:
  - [程序语言, C#语言]
author:
  - nightstardawn
---

# LinkedList

## 一、本质

LinkedList 是一个 C#为我们封装好的类
它的本质是一个可变类型的泛型双向链表#endregion

## 二、申明

```cs
//需要引用命名空间
using system.Collections.Generic
LinkedList<int> linkedList = new LinkedList<int>();
LinkedList<string> linkedList2 = new LinkedList<string>();
//链表对象 需要掌握两个类
//一个是链表本身 一个是链表节点类LinkedListNode
```

## 三、增删查改

### 1.增

```cs
//1.在链表尾部添加元素
linkedList.AddLast(10);
//2.在链表头部添加元素
linkedList.AddFirst(20);
//3.在某一个节点之后添加一个节点
//  要指定节点 先得得到一个节点
LinkedListNode<int> n = linkedList.Find(20);
linkedList.AddAfter(n, 15);
//4.在某一个节点之前添加一个节点
//  要指定节点 先得得到一个节点
LinkedListNode<int> n = linkedList.Find(20);
linkedList.AddBefore(n, 15);

```

### 2.删

```cs
//1.移除头节点
linkedList.RemoveFirst();
//2.移除尾节点
linkedList.RemoveLast();
//3.移除指定节点
// 无法通过位置直接移除
linkedList.Remove(20);
//4.清空
linkedList.clear();
```

### 3.查

```cs
//1.头节点
LinkedListNode<int> first = linkedList.First;
//2.尾节点
LinkedListNode<int> last = linkedList.Last;
//3.找到指定值的节点
//  无法直接通过下标获取中间元素
// 只有遍历查找指定位置元素
LinkedListNode<int> node = linkedList.Find(3);
Console.WriteLine(node.Value);
node = linkedList.Find(5);
//4.判断是否存在
if(linkedList.contains(1))
  Console.WriteLine("链表中存在1");
```

### 4.改

```cs
//要先得再改 得到节点 再改变其中的值
Console.WriteLine(linkedList.First.Value);
linkedList.First.Value = 10;
Console.WriteLine(linkedList.First.Value)
```

### 5.遍历

```cs
//1.foreach遍历
//  注意：这里取出来的直接就是值
foreach(int item in linkedList)
  Console.WriteLine(item);

//2.通过节点遍历
// 从头到尾
LinkedListNode<int> nowNode = linkedList.First;
while(nowNode != null)
{
  Console.WriteLine(nowNode.Value);
  nowNode = nowNode.Next;
}
```
