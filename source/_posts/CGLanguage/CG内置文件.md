---
title: CG内置文件
tags:
  - 程序语言
  - CG语言
categories:
  - [程序语言, CG语言]
author:
  - nightstardawn
---

# CG 内置文件

## 一、CG 内置文件的位置和作用

### 1.位置

我们可以在 Unity 的安装目录中找到 cG 内置文件
在 Editor->Data->CGIncludes 中
后缀为 cginc 的文件为 CG 语言内置文件
后缀为 g1slinc 的文件为 GLSL 语言内置文件

### 2.作用

他们是预定义的 shader 文件，里面包含了一些已经写好的 shader 相关逻辑
作用和 cG 内置函数一样，可以提升我们的 shader 开发效率，可以直接使用其中的方法等内容来进行逻辑开发

### 3.unity 中常用的内置文件有

1. UnitycG.cginc (包含最常用的帮助函数、宏和结构体等)
2. Lighting.cginc (包含各种内置光照模型，如果编写的是 surface shader(标准表面着色器)，会自动包含进来)
3. UnityShaderVariables.cginc (编译 Unityshader 时，会**自动包含进来**。包含许多内置的全局变量)
4. HLSLsupport.cginc (编译 Unityshader 时，会**自动包含进来**。声明了很多用于跨平台编译的宏和定义)
5. 等等

## 二、使用 CG 内置文件

在 CG 语句块中通过编译指令`#includ "内置文件名.cginc"`的形式进行引用

## 三、常用内容（学习阶段）

### 1.方法

| 方法                                           | 意义                                                                              |
| ---------------------------------------------- | --------------------------------------------------------------------------------- |
| float3 WorldSpaceViewDir(float4 v)             | 输入一个模型空间中的顶点位置 返回世界空间中从该点到摄像机的观察方向               |
| float3 ObjSpaceViewDir(float4 v)               | 输入一个模型空间中的顶点位置 返回模型空间中从该点到摄像机的观察方向               |
| float3 WorldSpaceLightDir(float4 v)            | 输入一个模型空间中的顶点位置 返回世界空间中从该点到光源的光照方向(仅用于向前渲染) |
| float3 ObjSpaceLightDir(float4 v)              | 输入一个模型空间中的顶点位置 返回模型空间中从该点到光源的光照方向(仅用于向前渲染) |
| float3 UnityObjectToWorldNormal(float3 normal) | 把法线方向从模型空间转换到世界空间                                                |
| float3 UnityObjectToWorldDir(in float3 dir)    | 把方向矢量从模型空间转换到世界空间                                                |
| float3 UnityWorldToObjectDir(float3 dir)       | 把方向矢量从世界空间转换到模型空间                                                |

### 2.结构体

**UnityCG.cginc**
| 结构体 | 意义 |
| ------------ | -------------------------------------------------------------- |
| appdata_base | 用于顶点着色器输入，顶点位置、顶点法线、第一组纹理坐标 |
| appdata_tan | 用于顶点着色器输入，顶点位置、顶点法线、顶点切线第一组纹理坐标 |
| appdat_full | 用于顶点着色器输入，顶点位置、顶点法线、四组（更多）纹理坐标 |
| appdat_img | 用于顶点着色器输入，顶点位置、第一纹理坐标 |
| v2fimg | 用于顶点着色器输出，裁剪空间中的位置，纹理坐标 |

**例子**

```cs
CGPROGRAM
            // 顶点着色器
            #pragma vertex vert;
            //片元着色器
            #pragma fragment frag;
            #include "UnityCG.cginc"
            sampler2D _My2D;
            //顶点着色器 回调函数
            v2f_img vert(appdata_base data)
            {
                //需要传递给片元着色器的数据
                v2f_img v2fData;
                //将模型顶点空间 转换 到裁剪空间当中
                v2fData.pos  = UnityObjectToClipPos(data.vertex);
                //v2fData.nomal = data.normal;
                v2fData.uv = data.texcoord;

                return v2fData;

            }
            //片元着色器 回调函数
            fixed4 frag(v2f_img data):SV_TARGET
            {
                fixed4 color = tex2D(_My2D,data.uv)
                return color;
            }

            ENDCG
```

### 3.变换矩阵宏

**注意：** 变换矩阵宏，多配合 mul()方法使用，一般用于坐标或者向量的矩阵变换

坐标空间变换顺序：
模型空间->(M)世界空间->(V)观察空间->(P)裁剪空间->屏幕空间
**UnityShaderVariables.cginc**
| 变换矩阵宏 | 意义 |
| ---------- | ---- |
|UNITY_MATRIX_MVP|当前模型\*观察\*投影矩阵，用于将顶点/方向向量从**模型空间**变换到**裁剪空间**|
|UNITY_MATRIX_MV|当前模型\*观察矩阵,用于将顶点/方向向量从**模型空间**变换到**观察空间**|
|UNITY_MATRIX_V|当前观察矩阵，用于顶点/方向向量从**世界坐标**变换到**观察空间**|
|UNITY_MATRIX_P|当前投影矩阵，用于顶点/方向向量从**观察矩阵**变换到**裁剪空间**|
|UNITY_MATRIX_VP|当前观察\*投影矩阵，用于将顶点/方向向量从**世界空间**变换到**裁剪空间**|
|UNITY_MATRIX_T_MV|UNITY_MATRIX_MV 的转置矩阵|
|UNITY_MATRIX_IT_MV|UNITY_MATRIX_MV 的逆转置矩阵，用于将法线从模型空间变换到观察空间，也可以用于得到|
|\_Object2World|用于将顶点/方向矢量从模型空间变换到世界空间|
|\_World2Object|\_Object2World 的逆矩阵，用于将顶点/方向矢量从世界空间变换到模型空间|

### 4.变量

- \_Time 自关卡以来加载的时间，用于对着色器内的事物进行动画处理
- \_LightColor0 光的颜色(向前渲染时，UnityLightingCommon.cginc;延迟渲染，UnityDefendLibrary.cginc)
- 等等

## 四、官方文档

- [内置文件](https://docs.unity3d.com/Manual/SL-BuiltinIncludes.html)
- [内置宏](https://docs.unity3d.com/Manual/SL-BuiltinMacros.html)
- [内置函数](https://docs.unity3d.com/Manual/SL-BuiltinFunctions.html)
- [内置变量](https://docs.unity3d.com/Manual/SL-UnityShaderVariables.html)
