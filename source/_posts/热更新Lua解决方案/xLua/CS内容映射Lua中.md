---
title: C#内容映射Lua中
tags:
  - 程序语言
  - 热更新
  - xLua
categories:
  - [热更新, AssetBundle]
author:
  - nightstardawn
---

# 获取全局变量和函数

## 一、获取全局变量的方法

使用 lua 解析器 evn 中 的 Global 属性

```cs
int i = LuaMgr.Instance.Global.Get<int>("testNumber");
bool b = LuaMgr.Instance.Global.Get<bool>("testBool");
Debug.Log("testNumber" + i);
Debug.Log("testBool" + b);
```

## 二、更改全局变量

```cs
LuaMgr.Instance.Global.Set("testNumber", 10);
int i2 = LuaMgr.Instance.Global.Get<int>("testNumber");
Debug.Log("testNumber" + i2);
```

## 三、获取全局函数

### 1.无参无返回

#### 1）.自己写委托

```cs
//无参无返回的委托
public delegate void CustomCall();

//自己写委托
CustomCall call = LuaMgr.Instance.Global.Get<CustomCall>("testFun");
call();
```

#### 2）.UnityAction

```cs
//Unity自带的委托
UnityAction ua = LuaMgr.Instance.Global.Get<UnityAction>("testFun");
ua();
```

#### 3）.Action

```cs
//C#自带提供的委托
Action ac = LuaMgr.Instance.Global.Get<Action>("testFun");
ac();
```

#### 4）.LuaFunction

```cs
//xLua提供的一种方式
LuaFunction lF = LuaMgr.Instance.Global.Get<LuaFunction>("testFun");
lF.Call();
```

### 2.有参有返回

#### 1）.自己写委托

```cs
//有参有返回值 ---注意要编辑器中 重新生成xlua代码
[CSharpCallLua] public delegate int CustomCall2(int a);

//自己写委托
CustomCall2 call2 = LuaMgr.Instance.Global.Get<CustomCall2>("testFun2");
Debug.Log(call2(10));
```

#### 2）.Action

```cs
//C#自带提供的委托
Func<int, int> sFun = LuaMgr.Instance.Global.Get<Func<int, int>>("testFun2");
Debug.Log(sFun(20));
```

#### 3）.LuaFunction

```cs
//xLua提供的一种方式
LuaFunction lF2 = LuaMgr.Instance.Global.Get<LuaFunction>("testFun2");
Debug.Log(lF2.Call(30)[0]);
```

### 3.多返回

#### 1）.自己写委托

##### A.out

```cs
[CSharpCallLua] public delegate int CustomCall3(int a, out int b, out bool c, out string d, out int e);
//out传递
CustomCall3 call3 = LuaMgr.Instance.Global.Get<CustomCall3>("testFun3");
int b;
bool c;
string d;
int e;
Debug.Log(call3(100, out b, out c, out d, out e));
Debug.Log(b + "_" + c + "_" + d + "_" + e);
```

##### B.ref

```cs

[CSharpCallLua] public delegate int CustomCall4(int a, ref int b, ref bool c, ref string d, ref int e);
//ref传递
CustomCall4 call4 = LuaMgr.Instance.Global.Get<CustomCall4>("testFun3");
int b1 = 0;
bool c1 = true;
string d1 = "";
int e1 = 0;
Debug.Log(call4(100, ref b1, ref c1, ref d1, ref e1));
Debug.Log(b + "_" + c + "_" + d + "_" + e);
```

#### 2）.LuaFunction

```cs
//xLua提供的一种方式
LuaFunction lF3 = LuaMgr.Instance.Global.Get<LuaFunction>("testFun3");
object[] ojbs = lF3.Call(1000);
for (int i = 0; i < ojbs.Length; i++)
{
    Debug.Log($"第{i}个返回值是" + ojbs[i]);
}

```

### 4.变长参数

#### 1）.自己写委托

```cs
//自己写委托 --- 变长参数的类型是由 实际的函数决定的
[CSharpCallLua] public delegate int CustomCall5(int a, params object[] args);

CustomCall5 call5 = LuaMgr.Instance.Global.Get<CustomCall5>("testFun4");
call5(1010, 1, 1, "23", 4, 6, "5", true);
```

#### 2）.LuaFunction

```cs
//xLua提供的一种方式
LuaFunction lF4 = LuaMgr.Instance.Global.Get<LuaFunction>("testFun4");
lF4.Call(1010, 1, 1, "23", 4, 6, "5", true);

```

## 四、List 和 Dictionary 映射 table

### 1.List

```cs
//同一类型的List
//值拷贝（浅拷贝） 不会改变lua中的类型
List<int> list = LuaMgr.Instance.Global.Get<List<int>>("testList1");
for (int i = 0; i < list.Count; i++)
{
  Debug.Log("List1: " + list[i]);
}
Debug.Log("***********************************************");
//不同类型的List
//使用Object存储即可
List<object> list2 = LuaMgr.Instance.Global.Get<List<object>>("testList2");
for (int i = 0; i < list2.Count; i++)
{
  Debug.Log("List2: " + list2[i]);
}
```

### 2.Dictionary

```cs
//加载同一类型的字典
//值拷贝（浅拷贝） 不会改变lua中的类型
Dictionary<string, int> Dic1 = LuaMgr.Instance.Global.Get<Dictionary<string, int>>("testDic");
foreach (KeyValuePair<string, int> item in Dic1)
{
  Debug.Log("Dic1: " + item.Key + "_" + item.Value);
}

//加载不同类型的字典
//使用Object存储即可
Dictionary<object, object> Dic2 = LuaMgr.Instance.Global.Get<Dictionary<object, object>>("testDic2");
foreach (KeyValuePair<object, object> item in Dic2)
{
  Debug.Log("Dic2: " + item.Key + "_" + item.Value);
}
```

## 五、类映射到 table

```cs
public class CallLuaClass
{
    //声明的成员变量 要和Lua中的变量名一样 且要公共的
    //自定义的变量 可以多 也可以少
    //值拷贝（浅拷贝） 不会改变lua中的类型
    public int testInt;
    public bool testBool;
    public float testFloat;
    public string testString;
    public UnityAction testFun;

}
public class Lesson7_CallClass : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        LuaMgr.Instance.Init();
        LuaMgr.Instance.DoLuaFile("Main");
        CallLuaClass callLuaClass = LuaMgr.Instance.Global.Get<CallLuaClass>("testClass");
        Debug.Log(callLuaClass.testInt);
        Debug.Log(callLuaClass.testFloat);
        Debug.Log(callLuaClass.testBool);
        Debug.Log(callLuaClass.testString);
        callLuaClass.testFun();
    }
}
```

## 六、接口映射到 table

**注意：**
接口中是不允许有成员变量
所以我们用成员属性来接 lua 中的变量

```cs
//注意这里接口内部更改后 要重新生成代码
//接口中的拷贝是浅拷贝！！！
[CSharpCallLua]
public interface ICallLuaInterface
{
    int testInt { get; set; }
    bool testBool { get; set; }
    float testFloat { get; set; }
    string testString { get; set; }
    UnityAction testFun { get; set; }
}

public class Lesson8_CallInterface : MonoBehaviour
{
    void Start()
    {
        LuaMgr.Instance.Init();
        LuaMgr.Instance.DoLuaFile("Main");
        ICallLuaInterface iCallLuaInterface = LuaMgr.Instance.Global.Get<ICallLuaInterface>("testInterface");
        Debug.Log(iCallLuaInterface.testInt);
        Debug.Log(iCallLuaInterface.testFloat);
        Debug.Log(iCallLuaInterface.testBool);
        Debug.Log(iCallLuaInterface.testString);
        iCallLuaInterface.testFun();
    }
}
```

## 七、LuaTable 映射到 table

```cs
public class Lesson9_CallLuaTable : MonoBehaviour
{
    void Start()
    {
        LuaMgr.Instance.Init();
        LuaMgr.Instance.DoLuaFile("Main");
        //一般都不建议使用LuaTable和LuaFunction
        //效率低 且是 浅拷贝
        LuaTable luaTable = LuaMgr.Instance.Global.Get<LuaTable>("testLuaTable");
        Debug.Log(luaTable.Get<int>("testInt"));
        Debug.Log(luaTable.Get<int>("testFloat"));
        Debug.Log(luaTable.Get<int>("testBool"));
        Debug.Log(luaTable.Get<int>("testString"));
        luaTable.Get<LuaFunction>("testFun").Call();
        luaTable.Dispose();
    }

}
```
