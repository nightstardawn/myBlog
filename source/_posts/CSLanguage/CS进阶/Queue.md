---
title: Queue
tags:
  - 程序语言
  - C#语言
categories:
  - [程序语言, C#语言，核心]
author:
  - nightstardawn
---

# Queue

## 一、本质

Queue（队列）是 C#为我们封装好的类，本质上是 object 数组,只是封装了特殊的存储规则(先进先出)

![示意图](https://s2.loli.net/2024/08/20/aYlnyUIMg59Ej8z.png)

## 二、申明

引用命名空间: using System.Collections;

`Queue queue = new Queue();`

## 三、增取查改

### 1.增

```cs
queue.Enqueue(1);
queue.Enqueue("string");
queue.Enqueue(true);
queue.Enqueue(1.4f);
```

### 2.取

**注意：**

- 队列中不存在删除的概念，只有取 即出队
- 取出的是队列最头部的内容

```cs
object o = queue.Dequeue();
```

### 3.查

**注意：**
栈只能查看队列头部的内容
无法查看队列中的内容

#### 1.）查看栈顶的内容

```cs
object o = queue.Peek();
```

#### 2.）查看栈中是否存在某元素

```cs
bool isInQueue = queue.Contains("string");
```

### 4.改

**注意：**

- 无法改变其中的内容
- 只能清空

#### 清空

```cs
queue.Clear();
```

## 四、遍历

### 1.获取长度

```cs
int queueCount = queue.Count();
```

### 2.foreach 遍历

顺序：队列头到队列尾

### 3.特殊遍历方法——转数组

```cs
object[] queueArray = queue.ToArray();

for(int i = 0;i < queueArray.Length;i++)
{
    Debug.Log(queueArray[i]);
}
```

### 4.循环出列

```cs
while(queue.Count > 0)
{
    object o = queue.Dequeue();
    Debug.Log(o);
}
```

## 五、装箱拆箱

由于用万物之父来存储数据，自然存在装箱拆箱。
当往其中进行值类型存储时就是在装箱。
当将值类型对象取出来转换使用时，就存在拆箱。
