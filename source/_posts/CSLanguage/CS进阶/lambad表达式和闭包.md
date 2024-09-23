---
title: lambad表达式和闭包
tags:
  - 程序语言
  - C#语言
categories:
  - [程序语言, C#语言, 核心]
author:
  - nightstardawn
---

# lambad 表达式和闭包

## 一、定义

可以将 lambad 表达式 理解为匿名函数的简写
它除了写法不同外
**使用上和匿名函数一模一样**
都是和委托或者事件 配合使用的

## 二、基本语法

```cs
(参数列表) =>
{

};
```

## 三、使用

1. 函数中传递委托参数时
2. 委托或事件赋值时
3. 脱离委托和事件时，是不会使用 lambad 表达式的

## 四、缺点

添加到委托或者事件容器后 lambad 表达式是不记录 是无法删除的

## 五、闭包

内层的函数可以引用包含在它外层的函数的变量
即使外层函数的执行已经终止
注意:
该变量提供的值并非变量创建时的值，而是在父函数范围内的最终值。

```cs
class Test()
{
  public event Action action;
  public Test()
  {
    int value = 10;
    //这里就形成的闭包
    //因为当构造函数执行完毕时 其中申明的临时变量value的生命周期发生了改变
    action = () =>
    {
      Console.WriteLine(value);
    }
    //--------------//
    for(int i = 0; i< 10;i++)
    {
      action = () =>
      {
        Console.WriteLine(i);
        //这里会打印10个10
      }
    }
    //--------------//
    for(int i = 0; i< 10;i++)
    {
      //此index非彼Index
      int index = i;
      action = () =>
      {
        Console.WriteLine(index);
        //这里会打印1~9
      }
    }
  }
}
```
