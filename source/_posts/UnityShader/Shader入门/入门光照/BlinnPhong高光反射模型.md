---
title: BlinnPhong高光反射模型
tags:
  - Shader
  - Shader入门
  - 光照模型
  - BlinnPhong高光反射模型
categories:
  - [技术美术, UnityShader，光照模型]
author:
  - nightstardawn
---

# BlinnPhong 高光反射模型

## 一、BlinnPhong 高光反射模型的来历和原理

### 1. 来历

通过我们之前知识的学习，Phong 式高光反射光照模型是 Phong 光照模型的一部分
因此很容易推测出 Blinn-Phong 式高光反射光照模型其实也是 Blinn-Phong 光照模型的一部分，主要是用于计算高光反射颜色
Blinn-Phone 光照模型的提出者：吉姆·布林(Jim Blinn，美国计算机科学家)

### 2. 原理

Blinn-Phong 式高光反射光照模型的理论是
它是对 Phong 式高光反射光照模型的改进，它不再使用反射向量计算镜面反射，而是使用半角向量来进行计算(半角向量为视角方向和灯光方向的角平分线方向)
认为高光反射的颜色和顶点法线向量以及半角向量夹角的余弦成正比,并且通过对余弦值取 n 次幂来表示光泽度(或反光度)

![ 2024-08-26 173325.png](https://s2.loli.net/2024/08/26/MTBDyx4tjkIhN2H.png)

## 二、BlinnPhong 高光反射模型的公式

高光反射颜色 = 光源颜色 \* 材质高光反射颜色 \* man(0, 标准化后顶点法线方向向量 · 标准化后半角方向向量) 幂

1. 标准化后顶点法线方向向量 · 标准化后半角反向向量 的结果就是 cosθ
2. 半角方向向量 = 视角单位向量 + 入射光单位向量
3. 幂 代表光泽度 余弦值取幂

## 三、Phong 和 BlinnPhong 的区别

由于两个光照模委型的计算方式不一样
**表现**上的不同

1. 高光散射

- Blinn-Phong 模型的高光通常会产生相对均匀的高光散射，这会使物体看起来光滑而均匀
- Phong 模型的高光可能会呈现更为锐利的高光散射，因为它基于观察者和光源之间的夹角。这可能导致一些区域看起来特别亮，而另一些区域则非常暗。

1. 高光锐度

- Blinn-Phong 模型的高光通常具有较广的散射角，因此看起来不那么锐利。
- Phong 模型的高光可能会更加锐利，特别是在观察者和光源夹角较小时，可能表现为小而亮的点

3. 光滑度和表面纹理

- Blinn-Phong 模型通常更适合表现光滑的表面，因为它考虑了表面微观凹凸之间的相互作用，使得光照在表面上更加均匀分布
- Phong 模型有时更适合表现具有粗糙表面纹理的物体，因为它的高光散射可能会使纹理和细节更加突出。

4. 镜面高光大小

- Blinn-Phong 模型通常产生的镜面高光相对较大，但均匀分布。
- Phong 模型可能会产生较小且锐利的镜面高光。

**性能**上的不同

- Blinn-Phong 模型通常比
- Phong 模型计算更快

## 四、顶点着色器的实现

### 1.实现步骤

1. 属性声明（材质高光反射的颜色，光泽度）
2. 渲染标签 Tags 的设置 将 LightMode 光照模式 设置为 ForwardBase 向前渲染（该模式通常用于不透明物体的基本渲染）
3. 引用内置文件 "UnityCG.cginc" "UnityCG.cginc"
4. 结构体声明
5. 基本逻辑的实现

### 2.实例代码

```cs
Shader "Unlit/BlinnSpecular"
{
    Properties
    {
        _SpecularColor("SpecularColor",Color) = (1,1,1,1)
        _SpecularNum("SpecularNum",Range(0,20)) = 0.5
    }
    SubShader
    {
        Pass
        {
            Tags{"LightMode" = "ForwardBase"}
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
            float4 _SpecularColor;
            float _SpecularNum;

            v2f vert (appdata_base v)
            {
                v2f data;
                data.pos = UnityObjectToClipPos(v.vertex);
                //标准化后顶点法线方向向量
                float3 wNormal = normalize(UnityObjectToWorldNormal(v.normal));
                //标准化后半角方向向量
                //1.标准化后的光源方向
                float3 lightDir = normalize(_WorldSpaceLightPos0);
                //2.标准化后的视角方向

                float3 wPos = mul(UNITY_MATRIX_M,v.vertex);
                float3 viewDir = normalize(_WorldSpaceCameraPos.xyz - wPos);
                //3.半角向量
                float3 halfDir = normalize(viewDir + lightDir);
                //高光反射颜色 = 光源颜色 * 材质高光反色颜色 * pow（max（0，dot（标准化顶点法线向量，标准化半角方向向量））,幂）

                data.color = _LightColor0.rgb * _SpecularColor.rgb * pow(max(0,dot(wNormal,halfDir)),_SpecularNum);
                return data;

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

![ 2024-08-29 132115.png](https://s2.loli.net/2024/08/29/EyUvBMITSjr7P9d.png)
![ 2024-08-29 132109.png](https://s2.loli.net/2024/08/29/P7jpmYAD1kSHNig.png)

## 四、片元着色器的实现

### 1.实现步骤

1. 属性声明（材质高光反射的颜色，光泽度）
2. 渲染标签 Tags 的设置 将 LightMode 光照模式 设置为 ForwardBase 向前渲染（该模式通常用于不透明物体的基本渲染）
3. 引用内置文件 "UnityCG.cginc" "UnityCG.cginc"
4. 结构体声明
5. 基本逻辑的实现

### 2.实例代码

```cs
Shader "Unlit/BlinnSpecularF"
{
    Properties
    {
        _SpecularColor("SpecularColor",Color) = (1,1,1,1)
        _SpecularNum("SpecularNum",Range(0,20)) = 0.5
    }
    SubShader
    {

        Pass
        {
            Tags{"LightMode" = "ForwardBase"}
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag

            #include "UnityCG.cginc"
            #include "Lighting.cginc"

            float4 _SpecularColor;
            float _SpecularNum;
            struct v2f
            {
                //裁剪空间下的位置
                float4 pos:SV_POSITION;
                //基于世界坐标下的坐标位置
                float3 wPos:TEXCOORD0;
                //基于世界坐标下的法线
                float3 wNormal:NORMAL;
            };
            v2f vert (appdata_base v)
            {
                v2f data;
                data.pos = UnityObjectToClipPos(v.vertex);
                data.wPos = mul(UNITY_MATRIX_M,v.vertex);
                data.wNormal = UnityObjectToWorldNormal(v.normal);
                return data;
            }

            fixed4 frag (v2f i) : SV_Target
            {
                //标准化后半角方向向量
                //1.标准化后的光源方向
                float3 lightDir = normalize(_WorldSpaceLightPos0);
                //2.标准化后的视角方向
                float3 viewDir = normalize(_WorldSpaceCameraPos.xyz - i.wPos);
                //3.半角向量
                float3 halfDir = normalize(viewDir + lightDir);
                //高光反射颜色 = 光源颜色 * 材质高光反色颜色 * pow（max（0，dot（标准化顶点法线向量，标准化半角方向向量））,幂）

                fixed3 color = _LightColor0.rgb * _SpecularColor.rgb * pow(max(0,dot(i.wNormal,halfDir)),_SpecularNum);
                return fixed4(color.rgb,1);
            }
            ENDCG
        }
    }
}

```

### 3.实现效果

![ 2024-08-29 133746.png](https://s2.loli.net/2024/08/29/GItHFa5MhWso4EX.png)
![ 2024-08-29 133752.png](https://s2.loli.net/2024/08/29/9HLrc4IjMm1hVvN.png)
