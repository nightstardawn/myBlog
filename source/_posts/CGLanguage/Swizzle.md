---
title: Swizzle操作符
tags:
  - 程序语言
  - CG语言
categories:
  - [程序语言, CG语言]
author:
  - nightstardawn
---

# Swizzle 操作符

## 一、什么是 Swizzle 操作符

- Swizzle 操作符通常以`.`的形式使用，后面跟着所需要的分量顺序
- 对于四维向量来说，我一般通过
  1. `向量.xyzw`或者`向量.rgba`两种写法来分别表示向量中的四个元素
  2. 这的意义就是向量一般用于表示坐标和颜色

## 二、如何使用 Swizzle 操作符

### 1. 利用它来提取分量

```
int4 i4 = int4(1,2,3,4);
int ix = i4.x；
int iy = i4.y；
int iz = i4.z；
int iw = i4.w；
```

### 2. 利用它来重新排列分量

```
int4 i4 = int4(1,2,3,4);
i4 = i4.yzxw;
```

### 3. 利用它来创建新向量

```
//高维向量取出一部分变成低维向量
int4 i4 = int4(1,2,3,4);
int3 i3 = i4.xyz;

//低维向量组成变成高维向量
int4 = int(i3,i4.x)
```

## 三、向量和矩阵的更多用法

### 1.利用向量声明矩阵

```
fixed4x4 f4x4 = {fixed(1,2,3,4),
                fixed(5,6,7,8),
                fixed(9,10,11,12),
                fixed(13,14,15,16)
                }
```

### 2.获取矩阵中的元素

等同于获取二维数组的方法

```
int2x3 sampleRect = {1,2,3,
                    4,5,6}
int i = sampleRect[0][0];
```

### 3.利用向量获取矩阵中的某一行

```
int2x3 sampleRect = {1,2,3,
                    4,5,6}
int3 i3 = sampleRect[0];
```

### 4. 高维转低维

矩阵和向量都可以低维转高位，会自动舍去多余元素

```
int4 i4 = int4(1,2,3,4);
int3 i3 = i4;//如果不指定明确元素，会直接使用前几个元素赋值

int2x3 sampleRect = {1,2,3,
                    4,5,6}
int2x2 sampleRect2 = sampleRect;//这里取{1，2，
                                        4，5}//即前n行n列
```
