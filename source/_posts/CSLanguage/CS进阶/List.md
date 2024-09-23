---
title: List
tags:
  - 程序语言
  - C#语言
categories:
  - [程序语言, C#语言, 核心]
author:
  - nightstardawn
---

# List

## 一、本质

List 是一个 c#为我们封装好的类
它的本质是一个可变类型的泛型数组
List 类帮助我们实现了很多方法，比如泛型数组的增删查改

## 二、申明

```cs
//需要引用命名空间
//using System.Collections.Generic
List<int> list = new List<int>();
```

## 三、增删查改

### 1.增

```cs
//1.单体增
list.Add(1)
list.Add(2);
list.Add(3);
list.Add(4);
list2.Add("123");
//2.群体增
List<string>listStr = new List<string>();liststr.Add("123");
list2.AddRange(liststr);
//3.插入
list.Insert(0,999);
```

### 3.删

```cs
//1.移除指定元素
list.Remove(1),
//2.移除指定位置的元素
list.RemoveAt(0);
//3.清空
list.clear();
```

### 4.查

```cs
//1.得到指定位置的元素
Console.WriteLine(list[0]);
//2.查看元素是否存在
if( list.contains(1))
    Console.WriteLine("存在元素 1")
//3.正向查找元素位置（第一个出现的元素）
// 找到返回位置 找不到 返回-1
int index= list.Index0f(5);
Console.WriteLine(index);
//4.反向查找元素位置（最后一个出现的元素）
//找到返回位置 找不到 返回-1
index = list.LastIndex0f(2);
Console.WriteLine(index);
```

### 5.改

```cs
Console.WriteLine(list[0]);
list[0]= 99;
Console.WriteLine(list[0]);
```

## 四、遍历

1. 长度
   `Console.WriteLine(list.count);`
2. 容量
   //避免产生垃圾
   `Console.WriteLine(list.Capacity);`

3. for 遍历

   ```cs
   for(int i=0;i< list.count;i++)
   {
        Console.WriteLine(list[i]);
   }
   ```

4. foreach 遍历

   ```cs
   foreach(int item in list)
   {
       Console.WriteLine(item);
   }
   ```
