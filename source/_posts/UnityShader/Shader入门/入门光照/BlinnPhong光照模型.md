---
title: BlinnPhong光照模型
tags:
  - Shader
  - Shader入门
  - 光照模型
  - BlinnPhong光照模型
categories:
  - [技术美术, UnityShader, 光照模型]
author:
  - nightstardawn
---

# BlinnPhong 光照模型

## 一、来历和原理

### 1.来历

Blinn Phong 光照模型我们之前提到过，它是由吉姆·布林(jim Blinn，美国计算机科学家)，在 1977 年时，在 Phong 光照模型基础上进行修改提出的
它和 Phong 一样是一个经验模型，并不符合真实世界中的光照现象
它们只是看起来正确

### 2.原理

Blinn Phong 和 Phong 光照模型一样，认为物体表面反射光线是由三部分组成的

> 环境光 +漫反射光 +镜面反射光(高光反射光)

## 二、公式

Phong 光照模型公式:

> 物体表面光照颜色环境光颜色 + 漫反射光颜色 + 高光反射光颜色

其中:

- 环境光颜色 = UNITY_LIGHTMODEL_AMBIENT(unity Ambientsky、unity AmbientEquator、unity AmbientGround)
- 漫反射光颜色 = 兰伯特光照模型 计算得到的颜色
- 高光反射光颜色= Blinn Phong 式高光反射光照模型 计算得到的颜色

## 三、顶点着色器的实现

### 1.实现步骤

1. 属性声明（材质高光反射的颜色，光泽度）
2. 渲染标签 Tags 的设置 将 LightMode 光照模式 设置为 ForwardBase 向前渲染（该模式通常用于不透明物体的基本渲染）
3. 引用内置文件 "UnityCG.cginc" "UnityCG.cginc"
4. 结构体声明
5. 基本逻辑的实现

### 2.实例代码

```cs
Shader "Unlit/BlinnPhong"
{
    Properties
    {
        _MainColor("MainColor",Color) = (1,1,1,1)
        _SpecularColor("SpecularColor",Color) = (1,1,1,1)
        _SpecularNum("SpecularNum",Range(0,20)) = 0.5
    }
    SubShader
    {
        Pass
        {
            Tags { "LightMode"="ForwardBase" }
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #include "UnityCG.cginc"
            #include "Lighting.cginc"
            struct v2f
            {
                float4 pos:SV_POSITION;
                fixed3 color:COLOR;

            };
            fixed4 _MainColor;
            fixed4 _SpecularColor;
            float _SpecularNum;

            fixed3 GetLambertColor(float3 normal)
            {
                float3 WNnormal = UnityObjectToWorldNormal(normal);
                float3 lightDir = normalize(_WorldSpaceLightPos0.xyz);
                fixed3 color = _MainColor * _LightColor0.rgb * max(0,dot(WNnormal,lightDir));
                return color;
            }
            fixed3 GetBlinnPhongSpecularColor(float4 pos,float3 normal)
            {
                float3 worldPos = mul(UNITY_MATRIX_M,pos);
                float3 viewDir = _WorldSpaceCameraPos.xyz - worldPos;
                viewDir = normalize(viewDir);

                float3 lightDir = normalize(_WorldSpaceLightPos0.xyz);
                float3 wNormal = UnityObjectToWorldNormal(normal);

                float3 halfA = normalize(viewDir +lightDir);

                fixed3 color = _LightColor0.rgb * _SpecularColor.rgb * pow(max(0,dot(wNormal,halfA)),_SpecularNum);
                return color;
            }
            v2f vert (appdata_base v)
            {
                v2f v2fData;
                v2fData.pos = UnityObjectToClipPos(v.vertex);
                fixed3 LambertColor = GetLambertColor(v.normal);
                fixed3 SpecularColor = GetBlinnPhongSpecularColor(v.vertex,v.normal);

                fixed3 color = UNITY_LIGHTMODEL_AMBIENT.rgb + LambertColor + SpecularColor;
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

### 3.实现效果

![ 2024-08-29 142811.png](https://s2.loli.net/2024/08/29/68QkcD5anU3z1qI.png)
![ 2024-08-29 142818.png](https://s2.loli.net/2024/08/29/vZp6RhzKJGmw79x.png)

## 四、片元着色器的实现

### 1.实现步骤

1. 属性声明（材质高光反射的颜色，光泽度）
2. 渲染标签 Tags 的设置 将 LightMode 光照模式 设置为 ForwardBase 向前渲染（该模式通常用于不透明物体的基本渲染）
3. 引用内置文件 "UnityCG.cginc" "UnityCG.cginc"
4. 结构体声明
5. 基本逻辑的实现

### 2.实例代码

```cs
Shader "Unlit/BlinnPhongF"
{
    Properties
    {
        _MainColor("MainColor",Color) = (1,1,1,1)
        _SpecularColor("SpecularColor",Color) = (1,1,1,1)
        _SpecularNum("SpecularNum",Range(0,20)) = 0.5
    }
    SubShader
    {

        Pass
        {
            Tags {"LightMode"="ForwardBase"}
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #include "UnityCG.cginc"
            #include "Lighting.cginc"
            struct v2f
            {
                float4  pos:SV_POSITION;
                float3  normal:NORMAL;
                float3  wPos:TEXCOORD0;
            };
            v2f vert (appdata_base v)
            {
                v2f data;
                data.pos = UnityObjectToClipPos(v.vertex);
                data.normal  =  UnityObjectToWorldNormal(v.normal);
                data.wPos = mul(UNITY_MATRIX_M,v.vertex).xyz;
                return data;
            }


            float3 _MainColor;
            fixed3 GetLambertFColor(in float3 normal)
            {
                float3 lightDir = normalize(_WorldSpaceLightPos0.xyz);
                fixed3 color =  _MainColor.rgb *  _LightColor0.rgb * max(0,dot(lightDir,normal));
                return color;
            }


            float3 _SpecularColor;
            float _SpecularNum;
            fixed3 GetBlinnSpecularColor(in float3 wPos,in float3 normal)
            {
                float3 viewDir = normalize(_WorldSpaceCameraPos.xyz-wPos);

                float3 lightDir = normalize(_WorldSpaceLightPos0).xyz;

                float3 halfA = normalize(viewDir + lightDir);

                fixed3 color = _LightColor0.rgb * _SpecularColor.rgb *pow(max(0,dot(halfA,normal)),_SpecularNum);
                return color;
            }
            fixed4 frag (v2f i) : SV_Target
            {
                fixed3 lambertColor = GetLambertFColor(i.normal);
                fixed3 SpecularColor = GetBlinnSpecularColor(i.wPos,i.normal);
                fixed3 color = UNITY_LIGHTMODEL_AMBIENT.rgb + lambertColor + SpecularColor;
                return fixed4(color.rgb , 1);
            }
            ENDCG
        }
    }
}

```

### 3.实现效果

![ 2024-08-29 142828.png](https://s2.loli.net/2024/08/29/PSqK8Ut5xLuYwog.png)
![ 2024-08-29 142833.png](https://s2.loli.net/2024/08/29/NsnKQdApFaLqPu4.png)
