---
title: Selection
tags:
  - Unity客户端
  - Unity进阶
  - 编辑器拓展
categories:
  - [Unity客户端, 编辑器拓展]
author:
  - nightstardawn
---

# Selection

## 一、Selection公共类是什么？

主要用于获取当前在Unity编辑器中选择的对象
只能用于编辑器开发

## 二、常用的静态成员

### 1.获取当前选择的Object
主要api：
Selection.activeObject
```csharp
if (GUILayout.Button("获取当前选择的Object的名字"))
{
    if (Selection.activeObject)
    {
        _stringBuilder.Clear();
        _stringBuilder.Append(Selection.activeObject.name);
        
        if(Selection.activeObject is GameObject)
            Debug.Log("它是游戏对象");
        else if(Selection.activeObject is Texture)
            Debug.Log("它是纹理资源");
        else if(Selection.activeObject is TextAsset)
            Debug.Log("它是一个文本");
        else
            Debug.Log("其他资源");
    }
    else
    {
        _stringBuilder.Clear();
        _stringBuilder.Append("没有选择");
    }
}
EditorGUILayout.LabelField("当前选择的对象",_stringBuilder.ToString());
```

### 2.获取当前选择的GameObject
主要api：
Selection.activeGameObject
```csharp
if (GUILayout.Button("获取当前选择的GameObject的名字"))
{
    if (Selection.activeGameObject)
    {
        _stringBuilder2.Clear();
        _stringBuilder2.Append(Selection.activeGameObject.name);
    }
    else
    {
        _stringBuilder2.Clear();
        _stringBuilder2.Append("没有选择");
    }
}
EditorGUILayout.LabelField("当前选择的游戏对象",_stringBuilder2.ToString());
```
### 3.获取当前选择的Transform
主要api：
Selection.activeTransform
```csharp
//注意：
//这里只能获取到场景中对象的Transform
if (GUILayout.Button("获取当前选择的有Transform对象的名字"))
{
    if (Selection.activeTransform)
    {
        _stringBuilder3.Clear();
        _stringBuilder3.Append(Selection.activeTransform.name);
        Selection.activeTransform.position = new Vector3(0, 1, -10);
    }
    else
    {
        _stringBuilder3.Clear();
        _stringBuilder3.Append("没有选择");
    }
}
```
### 4.获取当前选择所有的Object
主要api：
Selection.objects
```csharp
if (GUILayout.Button("获取当前选择所有的Object"))
{
    if (Selection.objects!=null)
    {
        _stringBuilder4.Clear();
        for (int i = 0; i < Selection.objects.Length; i++)
        {
            _stringBuilder4.Append(Selection.objects[i].name + "||");
        }
    }
    else
    {
        _stringBuilder4.Clear();
        _stringBuilder4.Append("没有选择");
    }
}
EditorGUILayout.LabelField("获取当前选择所有的Object",_stringBuilder4.ToString());
```
### 5.获取当前选择所有的GameObject
主要api：
Selection.gameObjects
```csharp
if (GUILayout.Button("获取当前选择所有的GameObject"))
{
    if (Selection.gameObjects!=null)
    {
        _stringBuilder5.Clear();
        for (int i = 0; i < Selection.gameObjects.Length; i++)
        {
            _stringBuilder5.Append(Selection.gameObjects[i].name + "||");
        }

    }
    else
    {
        _stringBuilder5.Clear();
        _stringBuilder5.Append("没有选择");
    }
}
EditorGUILayout.LabelField("获取当前选择所有的Object",_stringBuilder5.ToString());
```
### 6.获取当前选择的所有的Transform

主要api：
Selection.transform
```csharp
if (GUILayout.Button("获取当前选择的所有的Transform"))
{
    if (Selection.transforms!=null)
    {
        _stringBuilder6.Clear();
        for (int i = 0; i < Selection.transforms.Length; i++)
        {
            _stringBuilder6.Append(Selection.transforms[i].name + "||");
        }

    }
    else
    {
        _stringBuilder6.Clear();
        _stringBuilder6.Append("没有选择");
    }
}
EditorGUILayout.LabelField("获取当前选择的所有的Transform",_stringBuilder6.ToString());
```

## 三、常用的静态方法

### 1.判断某个对象是否被选中
主要api：
Selcction.Contain
```csharp
_obj = EditorGUILayout.ObjectField("用于判断是否被选中的对象" , _obj, typeof(GameObject), true);
if (GUILayout.Button("判断对象是否被选中"))
{
    if(Selection.Contains(_obj))
        Debug.Log("对象有被选中");
    else
        Debug.Log("对象没有被选中");
} 
```
### 2.筛选对象
1. 主要api：
    Selection.GetFiltered(类型，筛选模式枚举)
    Selection.GetFiltered<类型>(筛选模式枚举)
2. 筛选模式

| 筛选模式枚举             | 含义              |
|--------------------|-----------------|
| Unfiltered         | 不过滤             |
| TopLevel           | 只获取最上层对象，子对象不获取 |
| Deep               | 父对象和子对象都获取      |
| ExcludePrefab      | 排除预设体           |
| Editable           | 只选择可编辑对象        |
| OnlyUserModifiable | 仅用户可修改的内容       |
| Assets             | 只返回资源文件夹下的内容    |
| DeepAssets         | 如果存在子文件夹下，也可以获取 |

**注意：**
<br>如果要混用 用 | 即可

```csharp
if (GUILayout.Button("筛选所有对象"))
{
    Object[] objs = Selection.GetFiltered<Object>(SelectionMode.Assets | SelectionMode.DeepAssets);
    for (int i = 0; i < objs.Length; i++)
    {
        Debug.Log(objs[i].name);
    }
}
```

### 3.当选中的对象发生变化时调用的委托

主要api：
Selection.seletionChanged += 函数;
```csharp
private void OnEnable()
{
    Selection.selectionChanged += SelectionChanged;
}
private void OnDestroy()
{
    Selection.selectionChanged -= SelectionChanged;
}
private void SelectionChanged()
{
    Debug.Log("选择的对象发生了变化");
}
```























