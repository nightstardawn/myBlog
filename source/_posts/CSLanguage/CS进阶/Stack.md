---
title: Stack
tags:
  - 程序语言
  - C#语言
categories:
  - [程序语言, C#语言，核心]
author:
  - nightstardawn
---

# Stack

## 一、本质

Stack(栈)是 C#为我们封装好的类，本质上是 object 数组,只是封装了特殊的存储规则(先进后出)

![原理图](https://s2.loli.net/2024/08/19/ALCWwQE7ra2MFjk.png)

## 二、申明

引用命名空间: using System.Collections;

`Stack stack = new Stack();`

## 三、增取查改

### 1.增

```cs
//压栈
stack.push(1);
stack.push("string");
stack.push(true);
stack.push(1.2f);
```

### 2.取

**注意：**

- 栈中不存在删除的概念，只有取 即弹栈
- 取出的是栈最顶上的内容

```cs
object o = stack.Pop();
```

### 3.查

**注意：**
栈只能查看栈顶的内容
无法查看栈中的内容

#### 1.）查看栈顶的内容

```cs
object o = stack.Peek();
```

#### 2.）查看栈中是否存在某元素

```cs
bool isInStack = stack.Contains("string");
```

### 4.改

**注意：**

- 无法改变其中的内容
- 只能清空

#### 清空

```cs
stack.Clear();
```

## 四、遍历

### 1.获取长度

```cs
int stackCount = stack.Count();
```

### 2.foreach 遍历

顺序：栈顶到栈底

### 3.特殊遍历方法——转数组

```cs
object[] stackArray = stack.ToArray();

for(int i = 0;i < stackArray.Length;i++)
{
    Debug.Log(stackArray[i]);
}
```

### 4.循环弹栈

```cs
while(stack.Count > 0)
{
    object o = stack.Pop();
    Debug.Log(o);
}
```

## 五、装箱拆箱

由于用万物之父来存储数据，自然存在装箱拆箱。
当往其中进行值类型存储时就是在装箱。
当将值类型对象取出来转换使用时，就存在拆箱。
