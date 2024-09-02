---
title: ArrayList
tags:
  - 程序语言
  - C#语言
categories:
  - [程序语言, C#语言]
author:
  - nightstardawn
---

# ArrayList

## 一、本质

- Unity 为我们封装好的类
- 实质上是一个 object 数组
- ArrayList 给我们提供了很多方法——增删查改等

## 二、申明

**命名空间：** using System.Collections;

```cs
ArrayList arrayList = new ArrayList();
```

## 三、增删查改

### 1.增

#### 1.单独增加

可以增加各种类型

```cs
ArrayList arrayList = new ArrayList();
arrayList.Add("string");
arrayList.Add(1);
arrayList.Add(true);
arrayList.Add(new object());
```

#### 2.范围增加

将另一个容器内容 增加到该容器后面

```cs
ArrayList arrayList = new ArrayList();
ArrayList arrayList2 = new ArrayList();
arrayList2.AddRange(arrayList);
```

### 2.删

#### 1.）移除指定元素

```cs
ArrayList arrayList = new ArrayList();
arrayList.Add("string");
arrayList.Remove("string");//移除元素string
```

#### 2.）移除指定位置的元素

```cs
ArrayList arrayList = new ArrayList();
arrayList.Add("string");
arrayList.Add(1);
arrayList.Add(3);
arrayList.RemoveAt(2);//移除元素3
```

#### 3.）清空

```cs
arrayList.clean;
```

### 3.查

#### 1.）获取对应索引的元素

```cs
arrayList[Index];
```

#### 2.）判断某一个元素是否存在

```cs
arrayList.Contains("string");
```

#### 3.）正向查找元素位置

返回值：所在列表的索引值 若找不到则为-1

```cs
int Index = arrayList.IndexOf("string");
```

**注意：** 该方法 如果有元素相同，则返回第一个检索到的索引值

#### 4.）反向查找元素位置

返回值：所在列表的索引值 若找不到则为-1

```cs
int Index = arrayList.LastIndexOf("string");
```

**注意：** 该方法 如果有元素相同，则返回最后一个检索到的索引值

### 4.改

```cs
arrayList[Index] = 999;
```

### 5.插

```cs
arrayList.Insert(1,"插入元素");
```

参数一：插入位置索引
参数二：插入元素

## 四、遍历

- 获取长度：arrayList.Count
- 获取容量：arrayList.Capacity

for/foreach 遍历即可

## 五、装箱拆箱

ArrayList 本质上是一个可以自动扩容的 obiect 数组，由于用万物之父来存储数据，自然存在装箱拆箱。当往其中进行值类型存储时就是在装箱，当将值类型对象取出来转换使用时，就存在拆箱，所以 ArrayList 尽量少用，之后我们会学习更好的数据容器（List\<T\>）。
