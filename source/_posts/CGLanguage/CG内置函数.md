---
title: CG语法的内置函数
tags:
  - 程序语言
  - CG语言
categories:
  - [程序语言, CG语言]
author:
  - nightstardawn
---

# CG 语法的内置函数

## 一、什么是 CG 语言中的内置函数

Unity Shader 中的 CG 语言提供了各种用于图形编程的函数，这些函数是 CG 为我们封装好的逻辑，用于编写 UnityShader

## 二、内置函数

### 1.数学函数

#### 1.）三角函数

| 函数                        | 意义                                                              |
| --------------------------- | ----------------------------------------------------------------- |
| sincos(floay x,out s,out c) | 该函数用于同时计算 sin 和 cos，通过 s，c 进行返回（快于分别计算） |
| sin(x)                      | 正弦函数                                                          |
| cos(x)                      | 余弦函数                                                          |
| tan(x)                      | 正切函数                                                          |
| sinh(x)                     | 双曲正弦函数                                                      |
| cosh(x)                     | 双曲余弦函数                                                      |
| tanh(x)                     | 双曲正切函数                                                      |
| asin(x)                     | 反正弦函数(输入参数范围[-1,1]),返回[$-\pi/2$,$\pi/2$]             |
| acos(x)                     | 反余弦函数(输入参数范围[-1,1]),返回[0,$\pi$]                      |
| atan(x)                     | 反正切函数(输入参数范围[-1,1]),返回[$-\pi/2$,$\pi/2$]             |
| atan2(y,x)                  | 计算 y/x 的反正切值                                               |

#### 2.）向量、矩阵相关

| 函数           | 意义                 |
| -------------- | -------------------- |
| cross(A,B)     | 叉乘                 |
| dot(A,b)       | 点乘                 |
| mul(M,N)       | 俩矩阵相乘           |
| mul(M,v)       | 矩阵和向量相乘       |
| mul(v,M)       | 向量和矩阵相乘       |
| transpose(M)   | 计算转置矩阵         |
| determinant(M) | 计算矩阵的行列式因子 |

#### 3.）数值相关

| 函数               | 意义                                                                 |
| ------------------ | -------------------------------------------------------------------- |
| abs(x)             | 绝对值                                                               |
| ceil(x)            | 向上取整                                                             |
| floor(x)           | 向下取整                                                             |
| clamp(x,a,b)       | "夹紧"函数                                                           |
| radians(x)         | 角度转弧度                                                           |
| degrees(x)         | 弧度转角度                                                           |
| max(a,b)           | 取最大值                                                             |
| min(a,b)           | 取最小值                                                             |
| sqrt(x)            | 求平方根 x>0                                                         |
| pow(x,y)           | x 的 y 次方                                                          |
| round(x)           | 四舍五入                                                             |
| rsqrt(x)           | x 的反平方根，x>0                                                    |
| lerp(a,b,f)        | 插值函数，计算`(1-f)*a+b*f`或者`a+f*(b-a)`                           |
| lit(NdotL,NodeH,m) | 计算环境光、散射光、镜面反射光，返回四维向量                         |
| noise(x)           | 噪声函数，返回值[0,1],对于相同的输入，返回相同的输出，所以不是真随机 |
| 不常用的函数       | ![不常用的函数](https://s2.loli.net/2024/08/01/KuPJORZfYWrmd3T.png)  |

### 2.几何函数

| 函数             | 意义                                                    |
| ---------------- | ------------------------------------------------------- |
| lenght(v)        | 计算向量模长                                            |
| normalize(v)     | 归一化向量                                              |
| distance(p1,p2)  | 计算两点之间的距离                                      |
| reflect(I,N)     | 计算反射光的向量,I 入射光，N 平面法向量                 |
| refract(I,N,eta) | 计算折射射光的向量,I 入射光，N 平面法向量，eta 折射系数 |

**注意：** I、N 必须归一化，必须为三维向量

### 3.纹理函数

#### 4.）二维纹理

| 函数                                                  | 意义                               |
| ----------------------------------------------------- | ---------------------------------- |
| tex2D(sampler2D tex,float2 s)                         | 二维纹理查询                       |
| tex2D(sampler2D tex,float2 s,float2 dsdx,float2 dsdy) | 使用导数值查询二维纹理             |
| tex2D(sampler2D tex,float3 sz)                        | 二维纹理查询，并进行深度值比较     |
| tex2D(sampler2D tex,float3 s,float2 dsdx,float2 dsdy) | 使用导数值查询二维纹理             |
| tex2Dproj(sampler2D tex,float3 sq)                    | 二维投影纹理查询                   |
| tex2Dproj(sampler2D tex,float4 szq)                   | 二维投影纹理查询，并进行深度值比较 |

#### 5.）立方体纹理

| 函数                                                     | 意义                             |
| -------------------------------------------------------- | -------------------------------- |
| texCUBE(samplerCUBE tex,float s)                         | 查询立方体纹理                   |
| texCUBE(samplerCUBE tex,float s,float3 dsdx,float3 dsdy) | 使用导数值来查询立方体纹理       |
| tex2Dproj(samplerCUBE tex,float4 sq)                     | 查询立方体纹理，并进行深度值比较 |

#### 6.）其他纹理

![其他纹理](https://s2.loli.net/2024/08/01/oDEMv34PG81eKkc.png)
