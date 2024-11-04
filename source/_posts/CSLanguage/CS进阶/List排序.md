---
title: List排序
tags:
  - 程序语言
  - C#语言
categories:
  - [程序语言, C#语言，核心]
author:
  - nightstardawn
---

# List 排序

## 一、List 自带的排序

```cs
List<int> list = new List<int>();
//List自带的升序排序方法
list.Sort();
//list反转排序
list.Reverse();
```

## 二、自定义类的排序

### 1.实现方法

继承 IComparable 或者 IComparable<>接口 并实现对应 CompareTo 方法

### 2.示例

```cs
class Item:IComparable<Item>
{
    public int money;
    pulic Item(int money)
    {
        this.money = money;
    }
    public int CompareTo(Item other)
    {
        //返回值的含义
        //小于0：放在传入对象的前面
        //等于0：保持当前的位置不变
        //大于0：放在传入对象的后面
        //简单理解：
        //传入对象的位置就是0
        //如果你的返回为负数 就放在它左边 也就是前面
        //如果你的返回为正数 就放在它右边 也就是后面
        if(this.money > other.money)
            return 1;
        else
            return -1;

    }
}


List<Item> list = new List<Item>();
list.Sort();
```

## 三、通过委托函数进行委托函数

### 1.实现方法

Sort 函数中可以传入函数委托

### 2.示例

```cs
class Item
{
    public int money;
    pulic Item(int money)
    {
        this.money = money;
    }
}


static int SortItem(Item a,Item b)
{
    //传入的两个对象 为列表中的两个对象
    //进行两两的比较 用左边的和右边的条件 比较
    //返回值规则 和之前一样 8做标准 负数在左(前)正数在右(后)
    if(a.money>b.money)
        return 1;
    else
        return -1;
}

List<Item> list = new List<Item>();
list.Sort(SortItem);


```
