---
title: Bloom效果
tags:
  - Shader
  - Shader基础
  - 屏幕后处理
categories:
  - [技术美术, UnityShader，屏幕后处理]
author:
  - nightstardawn
---

# Bloom效果

## 一、Bloom效果是什么？

<br>Bloom效果（中文也可以叫做**高光溢出效果**），是一种
<br>使**画面中亮度较高的区域产生一种光晕或发光效果**的图像处理技术
<br>Bloom效果的主要目的是模拟现实世界中强光源在相机镜头或人眼中造成的散射和反射现象
<br>使得画面中较亮的区域“扩散”到周围的区域，造成一种朦胧的效果
![效果示意图](https://s2.loli.net/2024/11/20/nkZWYwgasFiObuP.png)
## 二、基本原理

可以概括成以下三点
1. 提取：提取源图像中的亮度区域存储到一张新纹理中
2. 模糊：将提取出来的纹理进行模糊处理(一般采用高斯模糊)
3. 合成：将模糊处理后的亮度纹理和源纹理进行颜色叠加

**关键知识点：**
- 多个Pass的运用
- 如何提取
- 如何模糊
- 如何合成


## 三、一些关键的知识点

### 1.多个Pass的运用

<br>通过高斯模糊的学习，
<br>我们知道在屏幕后处理时，可以单独调用某个Pass对渲染纹理进行处理
<br>只需要利用Unity中的三个函数
- RenderTexture.GetTemporary(纹理宽，纹理高，0)
- Graphics.Blit(源纹理,目标纹理,材质,passID)
- RenderTexture.ReleaseTemporary(引擎渲染对象)

在处理Bloom效果时，将使用4个Pass，他们分别是：
1. 用于**提取亮度区域**存储到新纹理中的一个Pass
2. 用于**处理提取出来的纹理进行高斯模糊**的两个Pass
3. 用于**与原图进行合成**的一个Pass

### 2.如何提取

<br>在Shader中我们声明一个亮度阈值，亮度低于该阈值不会被提取
<br>主要用于“提取”Pass的片元着色器函数中
<br>用于当前像素的灰度值 `L =  0.2125*R + 0.7154*G + 0.0721*B`与亮度阈值变量进行运算

<br>如果“提取”Pass是Shader中的第一个Pass，那么我们完全可以利用
<br>RenderTexture.GetTemporary 和 Graphics.Blit 函数将源纹理提取亮度信息后存储到缓存区中

**示例代码**
```shaderlab
fixed luminance(fixed4 color)
    return 0.2125 * color.r + 0.7154 * color.g + 0.0721 * color.b

fixed4 fragExractBright(v2f i) : SV_Target
{
    fixed4 c = tex2D(_MainTex,i.uv);
    fixed4 val = clamp(luminance(c) - _LuminanceThreshold , 0.0 , 1.0 );
    return c * val;
}
```
**注释：**
- 灰度值 - 亮度阈值变量 
  <br>是为了仅仅保留超过阈值的部分，可以提取出图像中亮度较亮的地方
- Clamp函数
  <br>如果小于0，则为0.大于1，则则为1；得到的val表示像素的亮度贡献
- 颜色 * 亮度
  <br>基于亮度阈值调节颜色亮度，若val为0，则为黑色。越接近1越接近原始颜色

### 3.如何模糊
1. 利用UsePass指令
   <br>通过之前的高斯模糊Shader中为两个Pass取名，让后在Bloom的Shader中复用
2. 在Bloom效果的Shader中声明一个纹理属性 _Bloom
   <br>主要用于存储模糊完毕后的纹理
   <br>在C#代码中完成高斯模糊处理后，只需要将缓存区的内容，写入材质球中的纹理属性即可
   <br>`material.SetTexture("_Bloom",buffer0);`
   <br>这样在Shader中就能得到模糊处理后的纹理亮度信息了

### 4.如何合成

<br>在“合成”的Pass 中
<br>我们只需要用
<br>**主纹理_MainTex** （其中使用的纹理是屏幕原图像）和
<br>**纹理属性_Bloom** （其中使用的纹理是模糊处理后的亮度纹理信息）
<br>进行颜色叠加即可，我们对两张纹理进行采样，将获取到的**颜色信息**进行**加法**运算
<br>因为颜色相加带来的效果就是增加亮度，使得原本高亮的部分变得更加显眼
<br>从而达到Bloom效果（高光溢出效果）


## 四、具体实现

### 1.提取部分
**实现思路：**
1. Shader部分
   1. 声明属性
      - 主纹理 _MainTex
      - 亮度区域纹理 _Bloom
      - 亮度阈值 _luminanceTreshlod
   2. 实现共享代码
      1. 属性映射
      2. 内置文件引用
      3. 结构体(顶点、uv)
      4. 灰度值(亮度值)计算函数
   3. Pass部分
      1. 屏幕后处理标配
      2. 顶点着色器
         <br>顶点转换、UV赋值
      3. 片元着色器
         <br>颜色采样、亮度贡献值计算、颜色*亮度贡献值
2. C#部分
   1. 声明亮度阈值成员变量
   2. 重写OnRenderImage函数
   3. 设置材质球的亮度阈值
   4. 利用三个映射函数对纹理进行Pass处理
      - Graphics.Blit
      - RenderTexture.GetTemporary
      - RenderTexture.ReleaseTemporary

### 2.模糊部分

1. Shader部分
   1. 添加模糊半径 _BlurSize 进行属性映射(注意：需要用到纹素)
   2. 修改之前的高斯模糊Shader，为其中两个Pass命名
   3. 在Bloom Shader中，利用UsePass 复用高斯模糊Shader中两个Pass
2. C#部分
   1. 复制高斯模糊中的三个属性
   2. 复制高斯模糊中C#代码中处理高斯模糊的逻辑
   3. 将模糊处理后的纹理，存储到_Bloom纹理属性中

### 3.合并部分

1. Shader部分
   1. 结构体
      1. 顶点坐标
      2. 4维的uv(xy存主纹理、wz存亮度纹理)
   2. 顶点着色器
      <br>顶点坐标转化，纹理坐标赋值
   3. 注意点
      <br>亮度纹理的uv坐标需要判断是否进行Y轴翻转
      <br>因为使用RenderTexture写入Shader的纹理变量时
      <br>Unity可能会对其进行Y轴翻转
      <br>可以通过Unity提供的宏进行判断
      ```csharp
      #if UNITY_STARTS_AT_TOP
      #endif
      ```
      <br>如果这个宏被定义，说明当前平台的纹理坐标系的Y轴原点在顶部
      <br>还可以在该宏中用纹素进行判断
      <br>如果纹素的y小于0，为负数，说明需要对Y轴进行调整
      ```csharp
      #if UNITY_STARTS_AT_TOP
      if(_MainTex_TexelSize.y < 0.0)
        翻转uv的y轴坐标
      #endif
      ```
      <br>主纹理不需要我们进行额外处理，一般Unity会自动处理
      <br>一般只需要在使用RenderTexture时才考虑该问题
   4. 片元着色器
      <br>两个纹理颜色采样后相加
   5. FallBack Off
2. C#部分
   <br>对源纹理进行合并处理


### 4.实现示例

```shaderlab
Shader "Unlit/Bloom"
{
    Properties
    {
        _MainTex ("Texture", 2D) = "white" {}
        //用于存储亮度纹理模糊后的结果
        _Bloom("Bloom",2D) = "" {}
        //亮度阈值 控制亮度纹理 亮度区域的
        _LuminanceThreshold("LuminanceThreshold",Float) = 0.5
        
        //模糊半径
        _BlurSpread("BlurSize",Float) = 1.0
    }
    SubShader
    {
        CGINCLUDE
        
        sampler2D _MainTex;
        half4 _MainTex_TexelSize;
        sampler2D _Bloom;
        float _LuminanceThreshold;
        float _BlurSpread;
        #include "UnityCG.cginc"
        struct v2f
            {
                float2 uv : TEXCOORD0;
                float4 vertex : SV_POSITION;
            };
        //计算颜色亮度值（灰度值）
        fixed luminance(fixed4 color)
        {
            return 0.2125 * color.r + 0.7154 * color.g + 0.0721 * color.b;
        }
        ENDCG
        //提取Pass
        Pass
        {
            //后处理标配
            ZTest Always
            Cull Off
            ZWrite Off
            
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            
            v2f vert(appdata_base v)
            {
                v2f o;
                o.vertex = UnityObjectToClipPos(v.vertex);
                o.uv = v.texcoord;
                return o;
            }
            fixed4 frag(v2f i) :SV_Target
            {
                //采样源纹理颜色
                fixed4 color = tex2D(_MainTex,i.uv);
                //得到亮度颜色贡献值
                fixed4 value = clamp(luminance(color) - _LuminanceThreshold,0,1);
                //返回颜色 * 亮度贡献值
                return color * value;
            }   

            

            
            ENDCG
        }
        //复用高斯模糊的Pass
        UsePass "Unlit/GaussianBlur/GASSIAN_BLUR_HORIZONTAL"
        UsePass "Unlit/GaussianBlur/GASSIAN_BLUR_VERTICAL"
        //合成Pass
        Pass
        {
            //后处理标配
            ZTest Always
            Cull Off
            ZWrite Off
            CGPROGRAM
            #pragma vertex vertBloom
            #pragma fragment fragBloom

            struct v2fBloom
            {
                float4 pos:POSITION;
                //xy主要用于对主纹理采样
                //zw主要用于对亮度模糊后的纹理进行采样
                half4 uv:TEXCOORD0;
            };

            v2fBloom vertBloom(appdata_base v)
            {
                v2fBloom o;
                o.pos = UnityObjectToClipPos(v.vertex);
                //亮度纹理和主纹理 要采样相同的地方进行颜色叠加
                o.uv.xy = v.texcoord;
                o.uv.zw = v.texcoord;

                //用宏去判断uv坐标是否被翻转
                #if UNITY_UV_STARTS_AT_TOP
                //如果纹素的y小于0，表示对y需要进行翻转
                if(_MainTex_TexelSize.y < 0)
                    o.uv.w = 1 - o.uv.w;
                #endif
                return o;
            }
            fixed4 fragBloom(v2fBloom i):SV_Target
            {
                return tex2D(_MainTex,i.uv.xy) + tex2D(_Bloom,i.uv.zw);
            }
            
            ENDCG
        }
    }
    Fallback Off
}
```

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UIElements.Experimental;

public class Bloom : PostEffectBase
{
    //亮度阈值
    [Range(0,4)]
    public float luminanceThreshold = 0.5f;
    //控制缩放的参数
    [Range(1,8)]
    public int downSample = 1;
    //模糊代码执行次数
    [Range(1,100)]
    public int iterations = 1;
    //模糊半径
    [Range(0,3)]
    public float blurSpread = 0.6f;
    protected override void OnRenderImage(RenderTexture source, RenderTexture destination)
    {
        if (material)
        {
            //设置亮度阈值变量
            material.SetFloat("_LuminanceThreshold",luminanceThreshold);
            int  rtW = source.width / downSample;
            int  rtH = source.height / downSample;
            //渲染纹理缓冲区
            RenderTexture buffer =  RenderTexture.GetTemporary(rtW,rtH,0);
            //采用双线性过滤模式 可以让缩放效果更加平滑
            buffer.filterMode = FilterMode.Bilinear;
            //第一步 提取Pass 得到对应的亮度信息 存入到缓冲区纹理中
            Graphics.Blit(source,buffer,material,0);
            //第二步 模糊处理
            //多次去执行 高斯模糊逻辑
            for (int i = 1; i <= iterations; i++)
            {
                //优化写法 更加常用
                //一般我们可以在我们的迭代中进行设置 相当于每次迭代处理高斯模糊时 都在增加我们的间隔距离
                material.SetFloat("_BlurSpread", 1 + i * blurSpread);
                //又声明一个新的缓存区
                RenderTexture buffer1 = RenderTexture.GetTemporary(rtW, rtH, 0);
                
                //因为我们需要两个Pass 处理图像两次
                //进行第一次水平卷积计算
                Graphics.Blit(buffer, buffer1, material, 1); // Color1
                //这时 关键内容都在buffer1中 相当于buffer已经没用了 可以直接释放
                RenderTexture.ReleaseTemporary(buffer);
                
                buffer = buffer1;
                buffer1 = RenderTexture.GetTemporary(rtW, rtH, 0);
                //进行第二次垂直卷积计算
                Graphics.Blit(buffer, buffer1, material, 2); // 在Color1 的基础上 乘上Color2 得到最终的高斯模糊计算的结果
                //释放缓存区
                RenderTexture.ReleaseTemporary(buffer);
                //相当于 buffer和buffer1都是指向的 这一次高斯模糊处理的结果
                buffer = buffer1;
            }
            //将提取出来的内容进行高斯模糊后 存储到Shader中的一个纹理变量
            //用于之后合成
            material.SetTexture("_Bloom",buffer);
            #region 测试看见提取效果
            /*//测试看见提取效果
            Graphics.Blit(buffer,destination);*/
            #endregion
            //合成步骤
            Graphics.Blit(source,destination,material,3);
            
            RenderTexture.ReleaseTemporary(buffer);
        }
        else
        {
            Graphics.Blit(source, destination);
        }
    }
}
```














