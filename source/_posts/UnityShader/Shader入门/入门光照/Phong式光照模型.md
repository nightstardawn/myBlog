---
title: Phong光照模型
tags:
  - Shader
  - Shader入门
  - 光照模型
  - Phong光照模型
categories:
  - [技术美术, UnityShader]
author:
  - nightstardawn
---

# Phong 光照模型

## 一、两个颜色相加

### 1.作用

两个颜色叠加在一起

### 2. 区别

- 相乘
  颜色相乘时，最终颜色会往黑色靠拢
  计算 两个颜色混合时一般用颜色相乘
  因为真实世界中多个颜色混在一起时，最终回变成黑色

- 相加
  颜色相机时，最终颜色回往往向白色靠拢
  计算关照 反射时一般用颜色相加，，因为向白色靠拢能带来更亮的感觉，复合光的表现

## 二、Unity Shader 中的环境光

我们在学习完兰伯特和半兰伯特光照模型时
在计算玩漫反射光照后，加上了一个环境光变量(UNITY_LIGHTMODEL_AMBIENT)
该环境光变量是可以在 Unity 中进行设置

**方式：**

1. Windows->Rendering->Lighting
2. Enviroment 页签中的 Enviroment Lighting
   这里可以设置环境光来源
3. ![示意图](https://s2.loli.net/2024/08/22/yTw4qducZIYQlEh.png)

- 当是 Skybox 和 Color 时
  我们可以通过 UNITY_LIGHTMODEL_AMBIENT 获取对应的环境光颜色
- 当是 Gradient(渐变)时，通过以下 3 个成员可以得到对应的环境光
  - unity_AmbientSky(周围的天空环境光)
  - unity_AmbientEquator(周围的赤道环境光)
  - uniyu_AmbientGround(周围的地面环境光)

**注意：**
这些内置变量都包含在 UnityShaderVariables.cginc(编译时自动包含该文件，可以不用手动包含)中

## 三、Phong 光照模型的来历和原理

- 来历：
  由裴祥风在 1975 年，提出的一种局部光照经验模型
- 原理：
  反射光由三部分组成：环境光 + 漫反射光 + 镜面反射光(高光反射光)

## 四、Phong 光照模型的公式

物体表面光照颜色 = 环境光颜色 + 漫反射颜色 + 高光反射光颜色

其中：
环境光颜色 ：UNITY_LIGHTMODEL_AMBIENT(unity_AmbientSky、unity_AmbientEquator、unity_AmbientGround)
漫反射颜色 ：兰伯特光照模型 计算得到的颜色
高光反色颜色 ：Phong 式高光反射光照模型 计算得到的颜色

## 五、顶点着色器的实现

### 1.关键步骤：

1. 计算兰伯特光照模型
2. 计算 Phong 式高光反射模型
3. 计算 Phong 模型

### 2.具体实现

```cs
Shader "Unlit/Phong"
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
            fixed3 GetSpecularColor(float4 pos,float3 normal)
            {
                float3 worldPos = mul(UNITY_MATRIX_M,pos);
                float3 viewDir = _WorldSpaceCameraPos.xyz - worldPos;
                viewDir = normalize(viewDir);

                float3 lightDir = normalize(_WorldSpaceLightPos0.xyz);
                float3 wNormal = UnityObjectToWorldNormal(normal);
                float3 reflectDir = reflect(-lightDir,normal);

                fixed3 color = _LightColor0.rgb * _SpecularColor.rgb * pow(max(0,dot(viewDir,reflectDir)),_SpecularNum);
                return color;
            }
            v2f vert (appdata_base v)
            {
                v2f v2fData;
                v2fData.pos = UnityObjectToClipPos(v.vertex);
                fixed3 LambertColor = GetLambertColor(v.normal);
                fixed3 SpecularColor = GetSpecularColor(v.vertex,v.normal);

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

### 3.效果示意图

![ 2024-08-23 140027.png](https://s2.loli.net/2024/08/23/wCQqiZJfGHDP3TL.png)
![ 2024-08-23 140020.png](https://s2.loli.net/2024/08/23/dvi8teaWAE9LPxU.png)

## 六、片元着色器的实现

### 1. 关键步骤

1. 计算兰伯特光照模型
2. 计算 Phong 式高光反射模型
3. 计算 Phong 模型

### 2. 具体实现

```cs
 Shader "Unlit/PhongF"
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
            fixed3 GetSpecularColor(in float3 wPos,in float3 normal)
            {
                float3 viewDir = normalize(_WorldSpaceCameraPos.xyz-wPos);

                float3 lightDir = normalize(_WorldSpaceLightPos0).xyz;
                float3 reflectDir = reflect(-lightDir,normal).xyz;

                fixed3 color = _LightColor0.rgb * _SpecularColor.rgb *pow(max(0,dot(reflectDir,viewDir)),_SpecularNum);
                return color;
            }
            fixed4 frag (v2f i) : SV_Target
            {
                fixed3 lambertColor = GetLambertFColor(i.normal);
                fixed3 SpecularColor = GetSpecularColor(i.wPos,i.normal);
                fixed3 color = UNITY_LIGHTMODEL_AMBIENT.rgb + lambertColor + SpecularColor;
                return fixed4(color.rgb , 1);
            }
            ENDCG
        }
    }
}

```

### 3.实现效果

![ 2024-08-25 110307.png](https://s2.loli.net/2024/08/25/DUvqX1cCRpZM4wz.png)
