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

## 五、折叠和折叠组
**注意：**
折叠和折叠组的区别只是表现上的不同
```csharp
//折叠控件
//参数一：折叠状态
//参数二：折叠标题
//参数三：是否允许点击文字触发
isHide = EditorGUILayout.Foldout(isHide, "折叠控件",true);
if (isHide)
{
    //枚举选择控件
    type = (E_TestEnum)EditorGUILayout.EnumPopup("枚举选择",type);
    //多选枚举
    type2 = (E_TestEnum)EditorGUILayout.EnumFlagsField("多选枚举", type2);
}
//折叠组控件
//和折叠空间仅有表现上的不同
isHideGroup = EditorGUILayout.BeginFoldoutHeaderGroup(isHideGroup, "折叠组控件");
if (isHideGroup)
{
    //枚举选择控件
    type = (E_TestEnum)EditorGUILayout.EnumPopup("枚举选择",type);
    //多选枚举
    type2 = (E_TestEnum)EditorGUILayout.EnumFlagsField("多选枚举", type2);
}
EditorGUILayout.EndFoldoutHeaderGroup();
```
![ 2024-11-08 204935.png](https://s2.loli.net/2024/11/08/hBkyZEL52M8KYDP.png)


## 六、开光和开关组

```csharp
//开关控件
isTog = EditorGUILayout.Toggle("开关控件", isTog);
isTogLeft = EditorGUILayout.ToggleLeft("左侧开关控件", isTogLeft);
//开关组控件
isTogGroup = EditorGUILayout.BeginToggleGroup("开关组控件", isTogGroup);
isTog = EditorGUILayout.Toggle("开关控件", isTog);
isTogLeft = EditorGUILayout.ToggleLeft("左侧开关控件", isTogLeft);
EditorGUILayout.EndToggleGroup();
```

![ 2024-11-08 204945.png](https://s2.loli.net/2024/11/08/zbtQfReE6OFcdoY.png)

## 七、滑动条、双块滑动条控件

### 1.滑动条
```csharp
//滑动条
//浮点数滑动条
fslider = EditorGUILayout.Slider("滑动条", fslider, 0, 100);
//整数滑动条
islider = EditorGUILayout.IntSlider("整数滑动条", islider, 0, 100);
```
![ 2024-11-08 211009.png](https://s2.loli.net/2024/11/08/Nb9GvpW1lF5dJEi.png)
### 2. 双块滑动条
```csharp
//双块滑动条
EditorGUILayout.MinMaxSlider("双块滑动条", ref leftValue, ref rightValue, 0, 100);
EditorGUILayout.LabelField("左值：" + leftValue.ToString());
EditorGUILayout.LabelField("右值：" + rightValue.ToString());
```
![ 2024-11-08 211013.png](https://s2.loli.net/2024/11/08/uZCPjtbmiXWJH28.png)

## 八、帮助框、间隔控件
### 1.帮助框
```csharp
EditorGUILayout.HelpBox("一般内容", MessageType.None);
EditorGUILayout.HelpBox("感叹号内容", MessageType.Info);
EditorGUILayout.HelpBox("警告符提示", MessageType.Warning);
EditorGUILayout.HelpBox("错误符提示", MessageType.Error);
```
![ 2024-11-08 211702.png](https://s2.loli.net/2024/11/08/pxOEPWyHw1s4RGo.png)
### 2.间隔控件
```csharp
EditorGUILayout.LabelField("间隔控件1");
EditorGUILayout.Space(50);
EditorGUILayout.LabelField("间隔控件2");
EditorGUILayout.Space(100);
EditorGUILayout.LabelField("间隔控件3");
```
![ 2024-11-08 211920.png](https://s2.loli.net/2024/11/08/GfQkEDIn31xiwHe.png)

## 九、动画曲线控件、布局api
```csharp
//---动画曲线控件---
 curve = EditorGUILayout.CurveField("动画曲线", curve);
```
![ 2024-11-08 213827.png](https://s2.loli.net/2024/11/08/AuZVqaQjRiYIxEL.png)
```csharp

//---布局api---
//---------
EditorGUILayout.BeginHorizontal();//开始水平布局
//一堆控件
EditorGUILayout.LabelField("控件1");
EditorGUILayout.LabelField("控件2");
EditorGUILayout.LabelField("控件3");
EditorGUILayout.EndHorizontal();//结束水平布局
//----------

//----------
EditorGUILayout.BeginVertical();//开始垂直布局
//一堆控件
EditorGUILayout.LabelField("控件1");
EditorGUILayout.LabelField("控件2");
EditorGUILayout.LabelField("控件3");
EditorGUILayout.EndVertical(); //结束垂直布局
//---------- 
vecPos =EditorGUILayout.BeginScrollView(vecPos);//开始滚动视图
//一堆控件
EditorGUILayout.EndScrollView();//结束布局
//----------
```
![ 2024-11-08 213818.png](https://s2.loli.net/2024/11/08/ILipOabd8vEhto9.png)
![ 2024-11-08 213833.png](https://s2.loli.net/2024/11/08/HpkodTNXZBGvAwY.png)















