---
title: Dictionary
tags:
  - 程序语言
  - C#语言
categories:
  - [程序语言, C#语言]
author:
  - nightstardawn
---

# Dictionary

## 一、本质

可以将 Dictionary 理解为 拥有泛型的 Hashtable
它也是基于键的哈希代码组织起来的 键/值对
键值对类型从 Hashtable 的 object 变为了可以自己制定的泛型

## 二、申明

```cs
//需要引用命名空间 using system.collections.Generic
Dictionary<iht,string> dictionary = new Dictionary<int, string>();
```

## 三、增删查改

### 1.增

```cs
//注意:不能出现相同键
dictionary.Add(1,"123");
dictionary.Add(2"222");
dictionary.Add(3,"222");
```

### 2.删

```cs
//1.只能通过键去删除
// 删除不存在键 没反应
dictionary.Remove(1);
dictionary.Remove(4);
//2.清空
dictionary.clear()
```

### 3.查

```cs
//1.通过键查看值
// 找不到直接报错
Console.WriteLine(dictionary[2]);
Console.WriteLine(dictionary[1]);
//2.查看是否存在
// 根据键检测
if( dictionary.containsKey(4))
    Console.writeLine("存在键为1的键值对")
// 根据值检测
if(dictionary.containsValue("1234"))
    Console.WriteLine("存在值为123的键值对");
```

### 4.改

```cs
Console.WriteLine(dictionary[1]);
dictionary[1]="555";
Console.WriteLine(dictionary[1]);
```

## 四、遍历

```cs
//1.遍历所有键
foreach(int item in dictionary.Keys)
{
    Console.WriteLine(item);
    Console.WriteLine(dictionary[item]);
}
//2.遍历所有值
for(string item in dictionary.Values)
    Console.WriteLine(item);
//3.键值对一起遍历
foreach(KeyValuePair<int,string> item in dictionary)
    Console.WriteLine("键:"+item.Key +"值:"+ item.Value);
```
