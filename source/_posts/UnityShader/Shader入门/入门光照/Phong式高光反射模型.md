---
title: Phong式高光反射模型
tags:
  - Shader
  - Shader入门
  - 光照模型
  - 高光反射光照模型
categories:
  - [技术美术, UnityShader，光照模型]
author:
  - nightstardawn
---

# Phong 式高光反射模型

## 一、来历和原理

- 来历:
  高光反射光照模型没有单一的发明者，因为有多种计算的方式和半兰伯特光照模型一样，是经过很多从业者的研究和发展而演化出来的
  > 其中比较关键的几位贡献者为
  >
  > 1.  Phong 光照模型的提出者:
  >     裴祥风(Bui-Tuong Phong，越南裔美国计算机学家)
  > 2.  Blinn-Phone 光照模型的提出者:
  >     吉姆·布林(Jim Blinn，美国计算机科学家)
- 原理:

  > Phong 式高光反射光照模型的理论是：
  >
  > 1.  基于光的反射行为和观察者的位置决定高光反射的表现效果
  > 2.  认为高光反射的颜色和光源的反射光线以及观察者位置方向向量夹角的余弦成正比，并且通过对余弦值取 n 次幂来表示光泽度(或反光度)

- 原理图
  ![ 2024-08-19 165457.png](https://s2.loli.net/2024/08/19/swz9uqiEkRTU4cJ.png)

## 二、公式

> 高光反射光光照颜色 = 光照的颜色 \* 材质高光反射的颜色 \* max(0,标准化后观察方向向量 ·标准化后的反射方法) 幂
>
> 1. 标准化后观察方向·标准化后反射方向 ---> cosθ
> 2. 幂 代表光泽度 余弦值取 n 次幂

## 三、如何再 Shader 中获取公式中的关键信息

| 所需信息                   | 代码                                              |
| -------------------------- | ------------------------------------------------- |
| 观察者的位置(摄像机的位置) | `_WorldSpaceCameraPos`                            |
| 相对于法向量的反射向量     | `reflect(入射向量,顶点法线向量)`--返回值:反射向量 |
| 指数幂                     | `pow(底数,指数)`---返回值:计算结果                |

## 四、顶点着色器的实现

### 1.实现步骤

1. 属性声明（材质高光反射的颜色，光泽度）
2. 渲染标签 Tags 的设置 将 LightMode 光照模式 设置为 ForwardBase 向前渲染（该模式通常用于不透明物体的基本渲染）
3. 引用内置文件 "UnityCG.cginc" "UnityCG.cginc"
4. 结构体声明
5. 基本逻辑的实现

### 2.实例代码

```cs
Shader "Unlit/Specular"
{
    Properties
    {
        //高光反射颜射
        _SpecularColor("SpecularColor",Color) = (1,1,1,1)
        //光泽度
        _SpecularNum("SpecularNum",Range(0,20)) = 0.5
    }
    SubShader
    {
        Tags { "LightMode"="ForWardBase" }

        Pass
        {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag

            #include "UnityCG.cginc"
            #include "Lighting.cginc"
            //对应属性的颜色和光泽度
            fixed4 _SpecularColor;
            float _SpecularNum;

            struct v2f
            {
                //裁剪空间下的顶点坐标
                float4 pos:POSITION;
                //颜色信息
                fixed3 color:COLOR;
            };

            v2f vert (appdata_base v)
            {
                v2f v2fData;
                //1.将顶点坐标转换到裁剪空间当中
                v2fData.pos = UnityObjectToClipPos(v.vertex);
                //2.计算颜色相关
                //高光反射光照颜色 = 光源颜色 * 材质高光反射颜色 * max（0，标准化后观察方向向量 · 标准化后反射方向向量） 幂
                //光源的颜色 _LightColor.rgb
                //材质高光反射的颜色 _SpecularColor
                //幂 光泽度

                //标准化后观察方向向量
                    //1.将 模型空间下的顶点位置 转换到 世界空间下
                float3 worldPos = mul(UNITY_MATRIX_M,v.vertex);
                    //2.得到的视角方向
                float3 viewDir = _WorldSpaceCameraPos.xyz - worldPos;
                    //3.单位化
                viewDir = normalize(viewDir);
                //标准化后反射方向向量
                    //1.得到光位置的方向向量（世界空间下）
                float3 lightDir = normalize(_WorldSpaceLightPos0.xyz);
                    //2.得到法线在世界坐标空间下的向量
                float3 normal = UnityObjectToWorldNormal(v.normal);
                    //3.计算反射光线向量
                float3 reflectDir = reflect(-lightDir,normal);

                //高光反射光照颜色 = 光源颜色 * 材质高光反射颜色 * max（0，标准化后观察方向向量 · 标准化后反射方向向量） 幂
                fixed3 color = _LightColor0.rgb * _SpecularColor.rgb * pow(max(0,dot(viewDir,reflectDir)),_SpecularNum);
                v2fData.color = color;
                return v2fData;
            }

            fixed4 frag (v2f i) : SV_Target
            {
                return fixed4(i.color.rgb,1);
            }
            ENDCG
        }
    }
}

```

### 3.实例效果

![视角一](https://s2.loli.net/2024/08/20/ip6X8j2eoVGz3Qb.png)
![视角二](https://s2.loli.net/2024/08/20/MypJl4jIYCP1hDx.png)

## 五、顶点着色器的实现

### 1.实现步骤

1. 属性声明（材质高光反射的颜色，光泽度）
2. 渲染标签 Tags 的设置 将 LightMode 光照模式 设置为 ForwardBase 向前渲染（该模式通常用于不透明物体的基本渲染）
3. 引用内置文件 "UnityCG.cginc" "UnityCG.cginc"
4. 结构体声明
5. 基本逻辑的实现

### 2.代码实现

```cs
 Shader "Unlit/SpecularF"
{
    Properties
    {
        _SpecularColor("SpacularColor",Color) = (1,1,1,1)
        _SpecularNum("SpecularNum",Range(0,20)) = 1
    }
    SubShader
    {



        Pass
        {
            //如果有多个渲染通道时，会将Tags放在Pass中
            Tags { "LightMode"="ForwardBase" }
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #include "UnityCG.cginc"
            #include "Lighting.cginc"
            struct v2f
            {
                //裁剪空间下的顶点坐标
                float4 pos:SV_POSITION;
                //世界坐标下的法线信息
                float3 normal:NORMAL;
                //世界空间下的顶点坐标
                float3 wPos:TEXCOORD0;

            };
            v2f vert (appdata_base v)
            {
                v2f data;
                //顶点坐标变换
                data.pos = UnityObjectToClipPos(v.vertex);
                //法线坐标空间变换
                data.normal = UnityObjectToWorldNormal(v.normal);
                //顶点转到世界空间
                //下列三种写法均可
                //data.wPos = mul(UNITY_MATRIX_M,v.vertex).xyz;
                //data.wPos = mul(_Object2World,v.vertex).xyz;
                data.wPos = mul(unity_ObjectToWorld,v.vertex).xyz;


                return data;
            }

            fixed4 _SpecularColor;
            float _SpecularNum;

            fixed4 frag (v2f i) : SV_Target
            {
                //color = 光源颜色 * 材质高光反射颜色 * pow(max(0,视角方向·反射方向)，光泽度)

                //1.视角方向

                float3 viewDir = normalize(_WorldSpaceCameraPos.xyz - i.wPos);

                //2.反射方向
                //光的方向
                float3 lightDir = normalize(_WorldSpaceLightPos0.xyz);
                //反射光线的方向
                float3 reflectDir = reflect(-lightDir,i.normal).xyz;

                fixed3 color = _SpecularColor.rgb * _LightColor0.rgb *pow(max(0,dot(viewDir,reflectDir)),_SpecularNum);

                return fixed4(color.rgb,1);
            }
            ENDCG
        }
    }
}

```

### 3.实现效果

![ 2024-08-21 135027.png](https://s2.loli.net/2024/08/21/MR5umWyoxBPwUir.png)
![ 2024-08-21 135020.png](https://s2.loli.net/2024/08/21/YqlW7IExC2gruSf.png)
