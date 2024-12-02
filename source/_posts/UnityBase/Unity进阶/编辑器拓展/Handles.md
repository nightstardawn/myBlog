---
title: Handles
tags:
  - Unity客户端
  - Unity进阶
  - 编辑器拓展
categories:
  - [Unity客户端, 编辑器拓展]
author:
  - nightstardawn
---

# Handles

## 一、Handles公共类的作用

<br>Handles公共类提供了很多api
<br>让我们可以在Scene窗口中绘制我们自定义内容
<br>它和GUI、EditorGUI类似，只不过专门提供给Scene使用
<br>主要用于绘制编辑器控制手柄等

<br>想要在Scene窗口中显示自定义内容
<br>我们需要在对应的响应函数进行处理

## 二、Scene窗口相应函数

1. 单独为某一个脚本实现一个自定义的脚本，并且该自定义的脚本继承Editor
   <br>一般命名为 自定义脚本名+Editor
2. 在自定义的脚本前添加特性
   <br>命名空间：UnityEditor
   <br>特性名：[CustomEditor(想要自定义脚本类名的type)]
3. 在该脚本中实现void OnSceneGUI()方法
   <br>该方法会在我们**选中**挂载**自定义脚本的对象**时，**自动更新**

## 三、自定义窗口中监听Scene窗口更新响应函数

可以在自定义窗口显示时
- 监听更新事件
  <br>SceneView.duringSceneGui += 函数事件
- 窗口隐藏或销毁时移除事件
  <br>SceneView.duringSceneGui -= 函数事件

## 四、颜色控制、文本、线段、虚线

### 1.颜色控制
```csharp
Handles.color = new Color(0,1,1,0.5f);
Handles.color = Color.red;
```
### 2.文本控件
**注意：**
<br>文本控件 不受color印象
<br>可以通过GUIStyle设置

```csharp
Handles.Label(Vector3.zero, "测试文本显示");
Handles.Label(obj.transform.position, "测试文本显示跟随游戏对象移动");
```
### 3.线段控件
```csharp
Handles.DrawLine(obj.transform.position,obj.transform.position + obj.transform.forward*5,5);
Handles.DrawLine(起点位置，终点位置，线段粗细);
Handles.DrawLines(多个点的位置)
```
### 4.虚线控件

```csharp
//虚线
Handles.DrawDottedLine(obj.transform.position,obj.transform.position + obj.transform.right*5,5);
Handles.DrawDottedLine(起点位置，终点位置，线段粗细);
Handles.DrawDottedLine(多个点的位置);
```

## 五、弧线、圆、立方体框线、几何体

### 1.弧度
```csharp
//绘制线框的弧线
//Handles.DrawWireArc(圆心，法线，绘制的开始朝向，角度，半径);
Handles.color = Color.red;
Handles.DrawWireArc(obj.transform.position,obj.transform.up,obj.transform.forward,90,5);

//绘制填充的弧线
//Handles.DrawSolidArc(圆心，法线，绘制的开始朝向，角度，半径);
Handles.DrawSolidArc(obj.transform.position,obj.transform.up,obj.transform.right,30,3);
```

### 2.圆

```csharp
//绘制圆的线框
//Handles.DrawWireDisc(圆心，法线，半径);
Handles.DrawWireDisc(obj.transform.position,obj.transform.up,2);
//绘制圆的填充
//Handles.DrawWireDisc(圆心，法线，半径);
Handles.DrawSolidDisc(obj.transform.position,obj.transform.up,1);
```

### 3.立方体线框

```csharp
//Handles.DrawWireCube(中心点，zyz的大小);
Handles.DrawWireCube(obj.transform.position,Vector3.one);
```

### 4.几何体

```csharp
//几何体
Handles.color = Color.blue;
//Handles.DrawAAConvexPolygon(几何体各个顶点);
Handles.DrawAAConvexPolygon(Vector3.zero,Vector3.right,Vector3.right+Vector3.forward,Vector3.forward);
```

## 六、移动轴、旋转轴、缩放轴


```csharp
//--------移动轴--------
//Handles.DoPositionHandle(位置，角度)
obj.transform.position = Handles.DoPositionHandle(obj.transform.position,obj.transform.rotation);
//Handles.PositionHandle(位置，角度)
obj.transform.position = Handles.PositionHandle(obj.transform.position,obj.transform.rotation);

//--------旋转轴--------
//Handles.DoRotationHandle(角度，位置)
obj.transform.rotation = Handles.DoRotationHandle(obj.transform.rotation, obj.transform.position);
//Handles.RotationHandle(角度，位置);
obj.transform.rotation = Handles.RotationHandle(obj.transform.rotation, obj.transform.position);

//--------缩放轴--------
//Handles.DoScaleHandle(缩放，位置，角度，HandleUtility.GetHandleSize(位置))
obj.transform.localScale = Handles.DoScaleHandle(obj.transform.localScale,obj.transform.position, obj.transform.rotation,HandleUtility.GetHandleSize(Vector3.zero));
//Handles.ScaleHandle(缩放，位置，角度，HandleUtility.GetHandleSize(位置));
obj.transform.localScale = Handles.ScaleHandle(obj.transform.localScale,obj.transform.position, obj.transform.rotation,HandleUtility.GetHandleSize(Vector3.zero));
```
HandleUtility.GetHandleSize的作用是：
获取给定位置的操纵器控制柄的世界控件位置大小
使用当前摄像机计算合适的大小
决定了控制柄的缩放大小

## 七、自由移动、自由旋转

一个不受约束的移动控制柄
这个把手可以在所有方向上自由移动
vector3 Handles.FreeMoveHandle(位置，句柄大小，移动步进值(按住ctrl键时会按该单位移动)，渲染控制手柄的回调函数);

1. 句柄大小一般配合Handleutility.GetHandlesize()函数使用
2. 渲染控制手柄的常用回调函数:
   - Handles.RectangleHandleCap:一个矩形形状的控制手柄，通常用于表示一个平面的控制面
   - Handles.CircleHandleCap:一个圆形的控制手柄，通常用于表示一个球体的控制面
   - Handles.ArrowHandleCap:一个箭头形状的控制手柄，通常用于表示方向

效果演示：

<br>![效果演示](https://s2.loli.net/2024/11/22/WLiyEgqH6UcKj4o.png)

## 八、显示GUI

### 1.GUI显示基础
```csharp
//Handles.BeginGUI();
//GUI相关代码
//Handles.endGUI();
```
### 2.获取Scene窗口大小

```csharp
float w = SceneView.currentDrawingSceneView.posizrion.w;
float h = SceneView.currentDrawingSceneView.posizrion.h;
GUILayout.BeginArea(new Rect(w -300,h - 300, 100,100));
//显示控件内容
GUILayout.EndArea();
```
















