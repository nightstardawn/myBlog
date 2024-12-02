---
title: HandleUtility
tags:
  - Unity客户端
  - Unity进阶
  - 编辑器拓展
categories:
  - [Unity客户端, 编辑器拓展]
author:
  - nightstardawn
---

# HandleUtility

## 1.HandleUtility的主要作用

HandleUtility是Unity的一个工具类
主要是用于处理场景中编辑器Handles以及其他一些与编辑器交互相关功能
它提供了一些静态方法，用于处理编辑器中的鼠标交互、坐标转换以及其他与Handles相关的功能

## 2.常用api

| 常用api                                             | 含义                                                                          |
|---------------------------------------------------|-----------------------------------------------------------------------------|
| GetHandleSize(Vector3 position)                   | 获取在场景中给定位置的Handles的合适尺寸<br>通常用于根据场景中对象的距离来调整Handle的大小，以便在不同的的缩放级别下保持合适的显示大小 |
| WorldToGUIPoint(Vector3 worldPosition)            | 将世界坐标转化为GUI坐标<br>通常同于将场景中的某一个点的位置转化为屏幕上的像素坐标，以便在GUI中绘制相关信息                  |
| GUIPointToWorldRay(Vetor2 positon)                | 将屏幕上的像素坐标转化为射线<br>通常用于从屏幕坐标中获取一条射线，用于检测场景中的物体或者进行射线投射                       |
| DistanceToLine(Vector3 lineStart,Vector3 lineEnd) | 计算场景中一条线段与鼠标光标的最短距离<br>通常用来制作悬停变色等功能                                        |
| PickGameObject(Vector2 position,bool isSelecting) | 在编辑器中进行对象的拾取<br>通常用于根据鼠标光标位置获取场景中的对象，以实现对象的选择或交互操作                          |
**使用示例：**
```csharp
//1.GetHandleSize(Vector3 position)
obj.transform.position = Handles.DoScaleHandle(obj.transform.localScale, obj.transform.position,
    obj.transform.rotation,
    HandleUtility.GetHandleSize(obj.transform.position));
//2.WorldToGUIPosition(Vector3 worldPosition)
Vector2 pos = HandleUtility.WorldToGUIPoint(obj.transform.position);
Handles.BeginGUI();
GUI.Button(new Rect(pos.x, pos.y, 50, 50), "测试按钮");
Handles.EndGUI(); 
//3.GUIToWorldRay(Vector2 position)
Ray r = HandleUtility.GUIPointToWorldRay(Event.current.mousePosition);
RaycastHit hit;
if(Physics.Raycast(r, out hit))
    Debug.Log(hit.collider.name);
//4.DistanceToLine(Vector3 lineStart,Vector3 lineEnd)
float dis = HandleUtility.DistanceToLine(Vector3.zero, Vector3.right);
Debug.Log(dis);
//5.PickGameObject(Vector2 position,bool isSelecting)
GameObject testObj = HandleUtility.PickGameObject(Event.current.mousePosition,true);
if (testObj)
    Debug.Log(testObj.name);
```
## 3.更多api

查阅官网

















