---
title: Hashtable
tags:
  - 程序语言
  - C#语言
categories:
  - [程序语言, C#语言，核心]
author:
  - nightstardawn
---

# ArrayList

## 一、本质

Hashtable (又称散列表) 是基于键的哈希代码组织起来的 键/值对
主要作用是提高数据的查询效率
使用键来访问集合中的元素

## 二、申明

**命名空间：** using System.Collections;

```cs
Hashtable hashtable = new Hashtable();
```

## 三、增删查改

### 1.增

**注意：** 键不要相同

```cs
hashtable.Add(1,1);
hashtable.Add("string","string");
hanshtable.Add(true,"true");
```

### 2.删

#### 1.）移除键指定元素

**注意：** 若键不存在，则无反应

```cs
hashtable.Remove(1);
hashtable.Remove(2);
```

#### 2.）清空

```cs
hashtable.Clear();
```

### 3.查

#### 1.）获取对应键的元素

**注意：** 找不到回返回空

```cs
hashtable[1];
hashtable["string"];
```

#### 2.）判断是否存在

1. 根据键检测
   ```cs
   bool isInhashtable = hashtable.Contains("键名")；
   bool isInhashtable = hashtable.ContainsKey("键名")；
   ```
2. 根据值检测
   ```cs
   bool isInhashtable = hashtable.ContainsValue("键名")；
   ```

### 4.改

**注意：** 只能改值，不能改键

```cs
hashtable[1] = "改的内容"；
```

## 四、遍历

- 获取键值对对数：hashtable.Count

### 1.遍历所有键

```cs
foreach (object key in hashtable.Keys)
{
    Debug.Log(key);
}
```

### 2.遍历所有值

```cs
foreach (object Value in hashtable.Values)
{
    Debug.Log(Value);
}
```

### 3.遍历所有键值对

```cs
foreach (DictionaryEntry item in hashtable)
{
    Debug.Log(item.Key);
    Debug.Log(item.Value);
}
```

### 4.迭代器遍历法

## 五、装箱拆箱

由于用万物之父来存储数据，自然存在装箱拆箱。
当往其中进行值类型存储时就是在装箱。
当将值类型对象取出来转换使用时，就存在拆箱。
