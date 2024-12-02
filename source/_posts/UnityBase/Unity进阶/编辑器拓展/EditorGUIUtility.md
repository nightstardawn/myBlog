---
title: EditorGUIUtility
tags:
  - Unity客户端
  - Unity进阶
  - 编辑器拓展
categories:
  - [Unity客户端, 编辑器拓展]
author:
  - nightstardawn
---

# EditorGUIUtility

## 一、EditorGUIUtility是什么？

<br>这是一个EditorGUI中的一个实用工具类
<br>提供了一些EditorGUI相关的其他辅助api

## 二、资源加载

### 1.特殊文件夹 Editor Default Resources

<br>它的主要作用就是放置提供给EditorGUIUtility加载的资源
<br>想要通过EditorGUIUtility公共类加载资源
<br>必须将资源文件放置在Editor Deafault Resources文件夹下

### 2.加载资源（如果资源不存在，返回空）

<br>对应API
<br>EidtorGUIUtility.Load
**注意事项：**
1. 只能加载Assets/Editor Default Resources/ 文件夹下的资源
2. 加载资源要加上后缀

```csharp
// 加载资源（资源不存在返回空）
if (GUILayout.Button("加载编辑器图片资源"))
    img = EditorGUIUtility.Load("21.png") as Texture;
if (img != null)
    GUI.DrawTexture(new Rect(0, 50, 160, 90), img);
```
### 3.加载资源（如果资源不存在，会报错）

对应API
EidtorGUIUtility.LoadRequired
**注意事项：**
1. 只能加载Assets/Editor Default Resources/ 文件夹下的资源
2. 加载资源要加上后缀
```csharp
// 加载资源（资源不存在返回空）
if (GUILayout.Button("加载编辑器图片资源"))
    img = EditorGUIUtility.LoadRequired("21.png") as Texture;
if (img != null)
    GUI.DrawTexture(new Rect(0, 50, 160, 90), img);
```

## 三、搜索栏查询、对象选中提示

### 1.搜索栏查询
1. 主要作用：
<br>弹出一个搜索框，用于查询自己想要的资源
2. 主要api
   1. EditorGUIUtility.ShowObjectPicker<资源类型>(默认选中的对象，是否允许查找场景对象，"查找对象名称过滤"，0)
      - 参数一：默认选中的对象的引用
      - 参数二：是否允许查找场景对象
      - 参数三：查找对象名称过滤(比如这里的normal,是指文件名称中有normal的会被搜索到)
      - 参数四：controlID (控件ID)，默认为0
   2. EditorGUIUtility.GetObjectPickerObject()
      - 获取选择对象   
   
3. 如何使用获取选择对象

<br>弹出的搜索窗口会通过发送事件的形式
<br>通知它的窗口对象信息的变化
<br>通过Event公共类可以获取其他窗口发送给自己的事件
<br>Event.current 获取当前的事件
<br>commandName 获取事件命令的名字
- ObjectSelecttoUpdated 对象选择发生变化时发送
- ObjectSelecttoClose 对象选择窗口关闭时发送
```csharp
if(Event.current.commandName == "ObjectSelecttoUpdated")
{
    //当选择发生时通知进入
}
else if(Event.current.commandName == "ObjectSelecttoClose")
{
    //当选择窗口关闭时通知进入
}
```
4. 使用示例

```csharp
// 搜索栏查询
if (GUILayout.Button("打开搜索栏查询窗口"))
{
    EditorGUIUtility.ShowObjectPicker<Texture>(null,false,null,0);
}

if (Event.current.commandName == "ObjectSelectorUpdated")
{
    img3 = EditorGUIUtility.GetObjectPickerObject() as Texture;
    if (img3)
        Debug.Log(img3.name);
}
else if(Event.current.commandName == "ObjectSelectorClosed")
{
    img3 = EditorGUIUtility.GetObjectPickerObject() as Texture;
    if (img3)
        Debug.Log("窗口关闭 - " + img3.name);
}
```
### 2.对象选中提示
1. 主要api
   <br>EditorGUIUtility.PingObject(想要提示选中的对象)
2. 使用示例

```csharp
// 对象选择提示
if (GUILayout.Button("高亮选中对象"))
{
    if (img3)
        EditorGUIUtility.PingObject(img3);
}
```
## 四、窗口事件传递、坐标转换

### 1.窗口事件传递
1. 基本使用逻辑
   <br>Event e = EditorGUIUtility.CommandEvent("事件名")
   <br>获取到另一个窗口后，让该窗口调用SendEvent(e)
   <br>在另一个窗口可以通过
   <br>Event.current.type == EventType.ExcuteCommand 判断
   <br>Event.current.CommandName == "事件名" 判断
   <br>**注意：**
   <br>如果是传递给别的窗口，则会自动打开并聚焦传递的窗口(无论是否监听)
2. 使用示例
```csharp
//窗口事件传递
if (GUILayout.Button("传递事件"))
{
    //声明一个事件
    Event e =EditorGUIUtility.CommandEvent("一个可爱的事件");
    Lesson3 window = EditorWindow.GetWindow<Lesson3>();
    window.SendEvent(e);
}
//事件的监听 判断是否有收到别的窗口发过来的事件
if (Event.current.type == EventType.ExecuteCommand)
{
    if (Event.current.commandName == "一个可爱的事件")
    {
        Debug.Log("收到一个可爱的事件");
    }
}
```

### 2.坐标转换

坐标主要有两种
- Screen坐标系(屏幕坐标系)：原点为屏幕的左上角
- GUI坐标系：原点为窗口的左下角

主要使用的api
- Point转换
  - ScreenToGUIPoint  
  - GUIToScreenPoint
- rect转换
  - ScreenToGUIRect
  - GUIToScreenRect

## 五、指定区域使用对应鼠标指针

**注意：**
这个光标样式是用户电脑自己设置的鼠标指针样式

1. 主要使用api
   AddCursorRect(Rect position , MouseCursor mouse)
2. MouseCursor枚举类型

| MouseCursor枚举类型      | 含义                        |
|----------------------|---------------------------|
| Arrow                | 普通指针箭头                    |
| Text                 | 文本文本光标                    |
| ResizeVertical       | 调整大小垂直调整大小箭头              |
| ResizeHorizontal     | 调整大小水平调整大小箭头              |
| Link                 | 带有链接徽章的链接小箭头              |
| SlideArrow           | 滑动箭头带有小箭头的箭头，用于指示在数字字段处滑动 |
| ResizeUpRight        | 调整大小向上向右调整窗口边缘的大小         |
| ResizeUpLeft         | 窗口边缘为左                    |
| MoveArrow            | 带有移动符号的箭头旁边用于场景视图         |
| RotateArrow          | 旁边有用于场景视图的旋转符号            |
| ScaleArrow           | 旁边有用于场景视图的缩放符号            |
| ArrowPlus            | 旁边带有加号的箭头                 |
| ArrowMinus           | 旁边带有减号的箭头                 |
| Pan                  | 用拖动的手拖动光标进行平移             |
| Orbit                | 用眼睛观察轨道的光标                |
| Zoom                 | 使用放大镜进行缩放的光标              |
| FPS                  | 带眼睛的光标和用于FPS导航的样式化箭头      |
| CustomCursor         |当前用户定义的光标|
| SplitResizeUpDown    |向上-向下调整窗口拆分器的大小箭头|
| SplitResizeLeftRight |窗口拆分器的左-右调整大小箭头|

## 六、绘制色板、绘制曲线

### 1.绘制色板
在指定区域绘制一个色板矩形
EditorGUIUtility.DrawColorSwatch(Rect 绘制色板的矩形，Color 颜色)
一般配合 EditorGUILayout.ColorField 颜色输入控件使用

```csharp
//绘制色板
color = EditorGUILayout.ColorField(new GUIContent("选取颜色"), color,true,true,true);
EditorGUIUtility.DrawColorSwatch(new Rect(180,180,30,30), Color.red);
```

### 2.绘制曲线

在指定位置绘制曲线
一般配合曲线输入控件使用
EditorGUIUtility.DrawCurveSwatch(Rect，AnimationCurve，SerialzedProperty，Color，Color)
- 参数一：绘制曲线的范围
- 参数二：曲线
- 参数三：要为绘制为SerializedProperty的曲线
- 参数四：绘制曲线的颜色
- 参数五：绘制背景的颜色

## 七、更多api
查看官方文档







    



















