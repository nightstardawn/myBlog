---
title: Gizmos
tags:
  - Unity客户端
  - Unity进阶
  - 编辑器拓展
categories:
  - [Unity客户端, 编辑器拓展]
author:
  - nightstardawn
---

# Gizmos

## 一、Gizmos类是用于做什么的

Gizmos和Handles一样
都是用于让我们拓展Scene窗口的
主要专注与绘制辅助线、图标、形状等

## 二、Gizmos响应函数

在继承MonoBehavior的脚本中实现以下函数
便可以在其中使用Gizmos来进行图形图像的绘制
1. OnDrawGizmos() 在每一帧调用，绘制的内容可以在Scene窗口中看见
2. OnDrawGizmosSelected() 仅当脚本依附的GameObject被选中时才会每帧调用绘制相关内容

## 三、如何控制绘制内容的角度

如果直接绘制，当对象的角度或者缩放发生改变时，绘制的内容不会发生变化
此时，如果想绘制的内容和对象同步变化，需要改变Gizmos的绘制矩阵`Gizmos.maxtix`

```csharp
//绘制的内容和对象同步变化---方法一
Gizmos.matrix = Matrix4x4.TRS(transform.position,transform.rotation,transform.localScale);
//绘制的内容和对象同步变化---方法二
Gizmos.matrix = transform.localToWorldMatrix;
//还原矩阵
Gizmos.matrix = Matrix4x4.identity;
```

如果想单独控制，将`Matrix4x4.TRS`内对应参数部分还原即可

## 四、颜色、立方体、视锥

```csharp
//----------颜色----------
Gizmos.color = Color.green;
//----------立方体----------
Gizmos.color = Color.green;
Gizmos.DrawCube(Vector3.zero, Vector3.one);
Gizmos.color = Color.red;
Gizmos.DrawWireCube(gameObject.transform.position, Vector3.one);
//----------视锥----------
//Gizmos.DrawFrustum(绘制中心，FOV角度，远裁剪平面距离，近裁剪平面距离，屏幕长宽比);
Gizmos.color = Color.yellow;
Gizmos.DrawFrustum(this.transform.position, 110,50,0.5f,1.7f);
```

## 五、贴图、图标

```csharp
//贴图
if (texture)
    Gizmos.DrawGUITexture(new Rect(this.transform.position.x,this.transform.position.y,160 , 90),texture);
//图标(图标资源需要在"Asset/Gizmos/中")
Gizmos.DrawIcon(this.transform.position,"21");
```

## 六、线段、网格、射线
```csharp
//线段
//Gizmos.DrawLine(起点，终点);
Gizmos.DrawLine(transform.position,Vector3.zero);
//网格
//Gizmos.DrawMesh(mesh,位置，角度);
if(mesh)
    Gizmos.DrawMesh(mesh,transform.position,transform.rotation);
//射线
//Gizmos.DrawRay(起点，方向);
Gizmos.DrawRay(transform.position,transform.forward);
```

## 七、球体、网格线
```csharp
//球体
Gizmos.DrawSphere(transform.position,2);
Gizmos.DrawWireSphere(transform.position,3);
//网格线
Gizmos.DrawWireMesh(mesh, transform.position, transform.rotation);
```

## 八、更多api

查询官方文档











