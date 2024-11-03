---
title: EditorGUI
tags:
  - Unity客户端
  - Unity进阶
  - 编辑器拓展
categories:
  - [Unity客户端, 编辑器拓展]
author:
  - nightstardawn
---
# EditorGUI

## 一、定义
</br>EditorGUI 类似于GUI
</br>是一个主要用于绘制编辑器拓展 UI 的工具类
</br>它提供了一些 GUI 中没有的API
</br>主要是 编辑器功能中会用到的一些 特殊控件

</br>而EditorGUILayout 类似于GUILayout
</br>是一个带有自动布局功能的 EditorGUI 绘制工具类

</br>我们经常会将 EditorGUI 和 GUI 混合使用 来制作一些编辑器拓展功能
</br>但是由于更多时候我们会用到自动布局功能
</br>因此现们接下来着重讲解EditorGUILayout中的功能
</br>EditorGUI和它的区别仅仅是需要自己设置位置布已

## 二、文本、层级和标签、颜色拾取 控件
### 1.文本控件
![ 2024-11-03 141424.png](https://s2.loli.net/2024/11/03/nKgV4CqEBj7HPmU.png)
```csharp
EditorGUILayout.LabelField("文本标签1");
EditorGUILayout.LabelField("文本标签2","测试内容");
```
### 2.层级和标签
![ 2024-11-03 142932.png](https://s2.loli.net/2024/11/03/IglfZ27nv9BpGT4.png)
```csharp
//层级
layer = EditorGUILayout.LayerField(layer);
layer = EditorGUILayout.LayerField("层级标签",layer);
//标签
tag = EditorGUILayout.TagField(tag);
tag = EditorGUILayout.TagField("标签标签", tag);
```

### 3. 颜色获取
![ 2024-11-03 142739.png](https://s2.loli.net/2024/11/03/fBmraHLu3MEYqsx.png)
```csharp
//颜色获取
//参数一：GUIContent 标签
//参数二：颜色变量
//参数三：是否显示拾色器
//参数四：是否显示Alpha通道
//参数五：是否使用支持HDR
color = EditorGUILayout.ColorField(new GUIContent("自定义颜色获取"), color, true, true, true);
```

