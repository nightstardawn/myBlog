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

## 三、枚举、整数选择、按下触发按钮 控件

#### 1.枚举控件
1. 单选枚举</br>
    `type = (E_TestEnum)EditorGUILayout.EnumPopup("枚举选择",type);`
   ![ 2024-11-04 134210.png](https://s2.loli.net/2024/11/04/B4GOMJVnxkuaLvC.png)
    
2. 多选枚举</br>
   `type2 = (E_TestEnum)EditorGUILayout.EnumFlagsField("多选枚举", type2);`
   ![ 2024-11-04 134630.png](https://s2.loli.net/2024/11/04/4cxTOlFdapnzyZu.png)

#### 2.整数选择控件

```csharp
//参数一：Label标题
//参数二：选择的整数内容
//参数三：整数选项string数组
//参数四：整数选项值int数组
index = EditorGUILayout.IntPopup("整数单选框",index,strs,ints);
EditorGUILayout.LabelField(index.ToString());
```

#### 3.按下按钮触发控件
```csharp
//按下触发按钮控件
//参数一：GUIContent 按钮中的文字标签
//参数二：FocusType枚举 告诉UI系统能否获得键盘焦点 当用户按Tab键时在控件之间切换
//Keyboard 该控件可以接收键盘焦点
//Passive 该控件不能接收键盘焦点
if (EditorGUILayout.DropdownButton(new GUIContent("按下触发按钮"), FocusType.Passive))
    Debug.Log("按钮被按下");
```
## 四、对象关联、各类型输入

### 1.对象关联
```csharp
//各对象关联
obj = EditorGUILayout.ObjectField("不可以关联场景中的对象",obj,typeof(GameObject),false) as GameObject;
obj = EditorGUILayout.ObjectField("可以关联场景中的对象",obj,typeof(GameObject),true) as GameObject;
```
![ 2024-11-04 224012.png](https://s2.loli.net/2024/11/04/p6h9BX2NH78jVuq.png)

### 2.各类型输入
**注意：**
</br>EditorGUILayout中还有一些Delayed开头的输入控件
</br>他们和普通的输入控件最主要的区别是：在用户按Enter键时或将焦点移出控件前，不会立马改变数值
```csharp
//XX变量 = EditorGUILayout。XXField("label标签",XX变量);
//例如：
//str = EditorGUILayout.TextField("字符串输入框",str);
//int = EditorGUILayout.IntField("整数输入框",int);
i = EditorGUILayout.IntField("整数输入框",i);
```
![ 2024-11-04 224055.png](https://s2.loli.net/2024/11/04/DhyJAnQpbq8GCI5.png)






















