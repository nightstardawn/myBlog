---
title: CShape代码修改材质
tags:
  - Shader
  - Shader基础
  - 屏幕后处理
categories:
  - [技术美术, UnityShader，屏幕后处理]
author:
  - nightstardawn
---

# CShape代码修改材质

<br>Unity中想要通过C#修改Shader中相关参数信息
<br>一般都是通过材质去进行修改的
<br>需要使用材质提供的各种相关方法进行修改

## 一、获取对象使用的材质

1. 获取到对象的渲染器的Renderer
   <br>MeshRenderer和Skinned Mesh Renderer都继承Renderer
   <br>通过里氏替换的原则父类获取，装载子类对象
2. 通过渲染器获取到对应的材质
   <br>通过利用渲染器中的material或者sharedMaterial来获取物体的材质
   <br>如果存在多个材质，可以通过使用renderer.materials或是renderer.sharedMaterial获取

**material和sharedMaterial的区别：**
- material
  <br>material属性会返回对象的实例化材质，相当于他会为对象创建一个该材质的独立副本
  <br>当你通过material属性修改材质时，这些更改只会影响这个特定对象，而不会影响使用相同材质的其他对象
  <br>使用material会增加性能的消耗
- sharedMaterial
  <br>sharedMaterial属性会返回对象的共享材质，相当于它返回的时使用这个材质的对象共享的一个材质实例
  <br>当你通过sharedMaterial属性修改材质时，这些更改会影响所有使用这个材质的对象
  <br>使用sharedMaterial不会会增加性能的消耗

## 二、如何修改材质的属性
1. 颜色
   <br>材质对象中有Color成员用于颜色修改
2. 纹理
   <br>材质对象中有mainTexture成员用于主纹理修改
3. 通用的修改方式
   <br>材质中有各种Set的方法，用于修改属性
   <br>通过传入属性名，以及对应值进行赋值
   <br>注意：这里的属性值以SubShader中声明的属性名为准，并非面板上显示的内容名
   <br>`material.SetColor("_Color",color);`
4. 修改Shader
   <br>调用材质中shader属性进行修改
   <br>利用Shader.Find(Shader名)得到对应的Shader

## 三、常用方法
1. 判断某个类型指定名字属性是否存在
   <br>`material.HasXX("属性名")`
2. 获取某个属性的值
   <br>`material.GetXX("属性名")`
3. 修改渲染队列
   <br>`material.RenderQueue = 100;`
4. 设置纹理缩放偏移
   <br>`material.SetTextureOffSet("_MainTexture",new Vector2(0.5f,0.5f));`
   <br>`material.SetTextureScale("_MainTexture",new Vector2(0.5f,0.5f));`
5. ···














