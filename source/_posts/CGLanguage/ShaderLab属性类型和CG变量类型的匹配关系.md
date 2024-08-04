---
title: ShaderLab属性类型和CG变量类型的匹配关系
tags:
  - 程序语言
  - CG语言
  - ShaderLab
categories:
  - [程序语言, CG语言]
author:
  - nightstardawn
---

# ShaderLab 属性类型和 CG 变量类型的匹配关系

## 一、ShaderLab 的属性类型对应 CG 中变量类型的

| shaderLab 中的属性类型 |    CG 中变量类型    |
| :--------------------: | :-----------------: |
|      Color,Vector      | float4,half4,fixed4 |
|    Range,Float,Int     |  float,half,fixed   |
|           2D           |      sampler2D      |
|          Cube          |     samplerCUBE     |
|           3D           |      sampler2D      |
|        2DArray         |   sampler2DArray    |

## 二、如何在 CG 语句块中使用 ShaderLab 中声明的属性

直接在 CG 语句块中
声明和属性中对应类型的同名变量即可

例子：

```cs
// Upgrade NOTE: replaced 'mul(UNITY_MATRIX_MVP,*)' with 'UnityObjectToClipPos(*)'


Shader "Lesson20/NewUnlitShader"
{
    Properties
    {
        _MyInt("MyInt",Int) =1
        _Myfloat("Myfloat",Float) =1
        _MyRang("MyRang",Range(0,5)) =1

        _MyColor("MyColor",Color)=(0,0,0,0)
        _MyVector("MyVector",Vector)=(1,2,0,0)

        _My2D("My2d",2D)=""{}
        _MyCube("MyCube",Cube)=""{}

        _My2DArray("My2DArray",2DArray)=""{}
        _My3D("My3D",3D)=""{}

    }
    SubShader
    {

        Pass
        {
            CGPROGRAM
            // 顶点着色器
            #pragma vertex vert;
            //片元着色器
            #pragma fragment frag;
            float4 _MyInt;
            float _Myfloat;
            fixed _MyRang;

            fixed4 _MyColor;
            float4 _MyVector;
            sampler2D _My2D;
            samplerCUBE _MyCube;
            //应用阶段到顶点着色器阶段 命名方式 a2v
            struct a2v
            {
                //顶点坐标（基于模型空间）
                float4 vertex:POSITION;
                //顶点法线（基于模型空间）
                float3 nomal:NORMAL;
                //纹理坐标（UV坐标）
                float2 uv:TEXCOORD0;
            };
            //顶点着色器传递给片元着色器
            struct v2f
            {
                //裁剪空间下的坐标
                float4 position:SV_POSITION;
                //顶点法线（基于裁剪空间）
                float3 nomal:NORMAL;
                //纹理坐标（UV坐标）
                float2 uv:TEXCOORD0;
            };
            //顶点着色器 回调函数
            v2f vert(a2v data)
            {
                //需要传递给片元着色器的数据
                v2f v2fData;
                //将模型顶点空间 转换 到裁剪空间当中
                v2fData.position  = UnityObjectToClipPos(data.vertex);
                v2fData.nomal = data.nomal;
                v2fData.uv = data.uv;

                return v2fData;

            }
            //片元着色器 回调函数
            fixed4 frag(v2f data):SV_TARGET
            {
                return _MyColor;
            }

            ENDCG
        }
    }
}

```
