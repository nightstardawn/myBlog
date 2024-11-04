---
title: TMP工具类
tags:
  - Unity客户端
  - Unity进阶
  - TextMeshPro
categories:
  - [Unity客户端, TextMeshPro]
author:
  - nightstardawn
---

# TMP 工具类

## 一、TMP_TextEventHandler 类

![ 2024-10-24 234436.png](https://s2.loli.net/2024/10/24/fsMqlkPvJwpA5XD.png)

### 1.作用

主要用于处理用户和 TMP 文本之间的交互事件
监听并响应 TMP 文本中的特定区域或标签（例如\<link> 和 特定字符的点击或鼠标悬停事件）
适用于创建超链接、工具提示、弹出信息等效果

### 2.使用

#### 1.）onLinkSelection(链接)

**当用户悬停超链接时触发**

对应函数设置参数（string，string，int）含义

- 参数 1：超链接内容
- 参数 2：超链接文本
- 参数 3：超链接索引

#### 2.）onCharacterSelection(字符)

**当用户悬停字符时触发**

对应函数设置参数（char，int）含义

- 参数 1：当前悬浮的字符
- 参数 2：当前当前悬浮的字符的索引

#### 3.）onWordSelection(单词)

**当用户悬停单词时触发**

对应函数设置参数（string，int，int）含义

- 参数 1：单词内容
- 参数 2：单词的索引
- 参数 3：单词的长度

#### 4.）onLineSelection(行)

**当用户悬停某一行时触发**

对应函数设置参数（string，int，int）含义

- 参数 1：该行的内容
- 参数 2：该行的索引
- 参数 3：该行字符的长度

#### 5.）onSpriteSelection(精灵图片)

**当用户悬停精灵图片时触发**

对应函数设置参数（char，int）含义

- 参数 1：
- 参数 2：精灵图片的索引长度

### 3.代码使用

和 UGUI 中 UI 组件添加监听事件类似

```cs
TMP_TextEventHandler tmpHandler;
tmpHandler.onLinkSelection.AddListener(对应函数);
```

## 二、TMP_TextUtilities 类

### 1.作用

包含多个常用方法，主要用于获取指定位置的文本信息
我们主要在点击文本时，利用该类来获取点击到的具体内容

### 2.常用 api

**注意：**
下面的方法返回的都是索引值，如果没有获取到信息返回-1
利用获取到的索引可以在 TMP 文本中的 textInfo 属性中的

- linkInfo
- wordInfo
- characterInfo
- lineInfo
  来获取信息

1. 获取指定位置文本中的具体内容
   - 获取链接索引 int FindIntersctingLink(TMP_Text text,Vecor3 position,Camera camera)
   - 获取单词索引 int FindIntersctingWord(TMP_Text text,Vecor3 position,Camera camera)
   - 获取字符索引 int FindIntersctingCharacter(TMP_Text text,Vecor3 position,Camera camera)
   - 获取行索引 int FindIntersctingLine(TMP_Text text,Vecor3 position,Camera camera)
2. 获取离给定位置最新文本中的具体内容
   - 获取链接索引 int FindNearestLink(TMP_Text text,Vecor3 position,Camera camera)
   - 获取单词索引 int FindNearestWord(TMP_Text text,Vecor3 position,Camera camera)
   - 获取字符索引 int FindNearestCharacter(TMP_Text text,Vecor3 position,Camera camera)
   - 获取行索引 int FindNearestLine(TMP_Text text,Vecor3 position,Camera camera)
3. 更多 api 可以查看官网

```cs
//使用示例

//第一步  继承对应的UI控制接口 如IPointerClickHandler
//第二步  实现接口的方法
//第三步  在方法中使用TMP_TextUtilities.方法
public void OnPointerClick()
{
  int index = TMP_TextUtilities.FindIntersctingLink(tmpUItext,eventData.position,null);
  //如果不为-1 就说明点到了一个超链接信息
  if(index != -1)
  {
    //得到超链接的文本信息
    print(tmpUItext.textInfo.linkInfo[linkIndex].GetLinkText());
    //得到超链接的信息
    print(tmpUItext.textInfo.linkInfo[linkIndex].GetLinkID());
  }
}

```
