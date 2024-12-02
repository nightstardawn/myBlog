---
title: Insprctor窗口拓展
tags:
  - Unity客户端
  - Unity进阶
  - 编辑器拓展
categories:
  - [Unity客户端, 编辑器拓展]
author:
  - nightstardawn
---

# Inspector窗口拓展

## 一、SerializedObject和SerializedProperty的作用

### 1.主要作用
<br>主要用于在Unity编辑器中操作和修改序列化对象的属性
<br>他们通常在自定义编辑器中使用，以创建更灵活、可定制的属性面板

### 2.简单的规则
- SerializeObject 代表脚本对象
- SerializeProperty 代表脚本对象中的属性

<br>具体可以查询官网

## 二、自定义脚本在Inspector窗口显示的内容

### 1.实现步骤
1. 单独为某一个脚本实现自定义脚本，并且脚本需要继承Editor
   <br>该脚本名命名为 自定义脚本名+Editor
2. 自定义脚本前加上特性
   - 命名空间：UnityEditor
   - 特性名： [CustomEditor(想要自定义脚本类名的Type)]
3. 声明对应SerializeProperty序列化属性 对象
   <br>主要通过它和自定义脚本中的成员进行关联
   <br>可以利用继承Editor后的成员serializeObject中的FindPropety("成员变量名")，方法关联成员
   <br>例如：
   <br>`SerializeProperty mySerializeProperty;`
   <br>`mySerializeProperty = serializeObject.FindProperty("自定义脚本中的成员名");`
   <br>一般在OnEnable中初始化
4. 重写OnInspectorGUI函数
   <br>该函数控制了Inspector窗口中显示的内容
   <br>只需要在其中重写内容便可以自定义窗口
   <br>**注意：**
   <br>其中的逻辑要写在这两个代码之间
   <br>`serializeObject.Update();`
   <br>`窗口显示逻辑`
   <br>`serializeObject.ApplyModifiedProperties();`

### 2.实现示例
```csharp
//通过这个特性 我们可以为Lesson7脚本 自定义Inspector窗口中的显示
[CustomEditor(typeof(Lesson7))]
public class Lesson7Editor : Editor
{
    private SerializedProperty atk;
    private SerializedProperty def;
    private SerializedProperty obj;

    private bool foldout;
    private void OnEnable()
    {
        atk = serializedObject.FindProperty("atk");
        def = serializedObject.FindProperty("def");
        obj = serializedObject.FindProperty("target");
    }
    //该函数控制了Inspector窗口中显示的内容
    public override void OnInspectorGUI()
    {
        //如果自定义Inspector窗口 一般不需要执行父类的方法
        //base.OnInspectorGUI();
        serializedObject.Update();
        //自定义Inspector窗口逻辑
        foldout = EditorGUILayout.BeginFoldoutHeaderGroup(foldout, "基础属性");
        if (foldout)
        {
            GUILayout.Button("测试自定义Inspector测试按钮");
            EditorGUILayout.IntSlider(atk, 0, 100, "攻击力");
            def.floatValue = EditorGUILayout.FloatField("防御力",def.floatValue);

            EditorGUILayout.ObjectField(obj, new GUIContent("敌对对象"));
        }
        EditorGUILayout.EndFoldoutHeaderGroup();
        
        serializedObject.ApplyModifiedProperties();
    }
}
```


## 三、获取脚本依附对象

Editor中的target成员变量，代表的就是该脚本的依附对象


## 四、数组、List属性在Inspector窗口中显示

### 1.基础方式

<br>主要知识点：
<br>`EditorGUILayout.PropertyField(SerializedProperty对象,标题)`
<br>该api会按照属性类型自己去处理控件绘制的逻辑

### 2.自定义方法

主要知识点：
1. arraySize 获取数组或这List的长度
2. InsertArrayElementAtIndex(索引) 获取数组在指定索引插入默认元素(容量会变化)
3. DeleteArrayElementAtIndex(索引) 获取数组在指定索引删除元素(容量会发生变化)
4. GetArrayElementAtIndex(索引) 获取数组在指定索引位置的 SerializeProperty 对象

### 3.实现示例
```csharp
//通过这个特性 我们可以为Lesson7脚本 自定义Inspector窗口中的显示
[CustomEditor(typeof(Lesson7))]
public class Lesson7Editor : Editor
{
    private SerializedProperty strs;
    private SerializedProperty ints;
    private SerializedProperty floats;
    
    public SerializedProperty listObjs;
    private int count;
    private void OnEnable()
    {
        strs = serializedObject.FindProperty("strs");
        ints = serializedObject.FindProperty("ints");
        floats = serializedObject.FindProperty("floats");
        //默认得到的数组和List容量为空
        listObjs = serializedObject.FindProperty("listObjs");
        //初始化当前容量 否则 每一次一开始都是0
        count = listObjs.arraySize;
    }
    //该函数控制了Inspector窗口中显示的内容
    public override void OnInspectorGUI()
    {
        //容量设置
        count = EditorGUILayout.IntField("List 容量", count);
        //判断是否要缩减 移除尾部的内容
        //从后往前去移除 只有当容量变少时 才会走这的逻辑
        for (int i = listObjs.arraySize - 1 ; i >= count; i--)
        {
            listObjs.DeleteArrayElementAtIndex(i);
        }
        //根据容量绘制需要设置的每一个索引位置的对象
        for (int i = 0; i < count; i++)
        {
            //判断如果数组或者List中的容量不够，通过插入的形式去扩容
            if(listObjs.arraySize <= i)
                listObjs.InsertArrayElementAtIndex(i);
            
            SerializedProperty indexProperty = listObjs.GetArrayElementAtIndex(i);
            EditorGUILayout.ObjectField(indexProperty, new GUIContent($"索引{i}"));
        }
        serializedObject.ApplyModifiedProperties();
    }
}
```

## 五、自定义属性在Inspector窗口中显示

### 1.基础方式

<br>主要知识点：
<br>`EditorGUILayout.PropertyField(SerializedProperty对象,标题)`
<br>该api会按照属性类型自己去处理控件绘制的逻辑

### 2.自定义的方式

主要知识点
1. SerializeProperty.FindPropertyRelative(属性)
2. SerializeProperty.FindProperty(属性,子属性)


```csharp
//通过这个特性 我们可以为Lesson7脚本 自定义Inspector窗口中的显示
[CustomEditor(typeof(Lesson7))]
public class Lesson7Editor : Editor
{
    private SerializedProperty myCustomI;
    private SerializedProperty myCustomF;
    
    private void OnEnable()
    {
        myCustom = serializedObject.FindProperty("myCustom");
        //第一种方式查找
        myCustomI = myCustom.FindPropertyRelative("i");
        myCustomF = myCustom.FindPropertyRelative("f");
        //第二种方式查找
        myCustomI = serializedObject.FindProperty("myCustom.i");
        myCustomF = serializedObject.FindProperty("myCustom.f");
    }
    //该函数控制了Inspector窗口中显示的内容
    public override void OnInspectorGUI()
    {
        //如果自定义Inspector窗口 一般不需要执行父类的方法
        //base.OnInspectorGUI();
        serializedObject.Update();
        //EditorGUILayout.PropertyField(myCustom, new GUIContent("我的自定义属性"));

        myCustomI.intValue = EditorGUILayout.IntField("自定义属性中的i", myCustomI.intValue);
        myCustomF.floatValue = EditorGUILayout.FloatField("自定义属性中的f", myCustomF.floatValue);
        serializedObject.ApplyModifiedProperties();
    }
}
```
## 五、字典属性在Inspector窗口中显示

### 1.如何在Inspector窗口中编辑字典成员

<br>Unity默认不支持Dictionary在Inspector窗口的显示
<br>只能利用两个List（或者数组）成员来间接设置Dictionary

### 2.ISerializationCallbackReceiver接口

<br>该接口时Unity提供的用于序列化和反序列化时，执行自定义逻辑的接口
<br>实现该接口的类能够在对象被序列化到磁盘或则从磁盘反序列化时执行一些额外的代码

<br>接口中的函数
- OnBeforeSerialize ： 在对象序列化之前调用
- OnAfterSerialize ： 在对象从磁盘反序列化后调用

<br>由于我们需要用两个List存储Dictionary的具体值
<br>相当于字典中的真正内容时存储在两个List中的
<br>所以我们需要在
<br>OnBeforeSerialize 序列化之前，将Dictionary中的数据存入List中进行序列化
<br>OnAfterSerialize 反序列化之后，将List中反序列化出来的数据存入Dictionary中

```csharp
public class Lesson7 : MonoBehaviour , ISerializationCallbackReceiver
{ 
    public Dictionary<int,string> myDic = new Dictionary<int, string>();
    [SerializeField]
    private List<int> key = new List<int>();
    [SerializeField]
    private List<string> value = new List<string>();
    public void OnBeforeSerialize()
    {
        myDic.Clear();
        for (int i = 0; i < key.Count; i++)
        {
            if (!myDic.ContainsKey(i))
                myDic.Add(key[i],value[i]);
            else
                Debug.Log("字典中不允许有相同的键");
        }
    }
    public void OnAfterDeserialize()
    {
        key.Clear();
        value.Clear();
        foreach (KeyValuePair<int,string> item in myDic)
        {
            key.Add(item.Key);
            value.Add(item.Value);
        }
    }
}
```
### 3.利用两个List在Inspector窗口中自定义Dictionary显示

<br>由于我们是在Inspector窗口中显示的信息数据来源时List
<br>所以只需要利用List在Inspector窗口中自定义显示即可





















