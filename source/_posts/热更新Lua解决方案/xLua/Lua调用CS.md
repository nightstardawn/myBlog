---
title: Lua调用C#
tags:
  - 程序语言
  - 热更新
  - xLua
categories:
  - [热更新, AssetBundle]
author:
  - nightstardawn
---

# Lua 调用 C#

## 一、调用 C#中的类

**注意：**
Lua 没办法直接访问 C# 一定是先从 C# 调用 Lua
才把核心逻辑交给 Lua 编写

### 1.基本使用方法

`CS.命名空间.类名`

例如：
`CS.UnityEngine.GameObject`

### 2.如何调用

#### 1.）实例化一个类对象

Lua 中没有 new 所以直接 类名() 就是实例化对象
默认调用的 就相当于无参构造

```lua
local obj1 = CS.UnityEngine.GameObject()
local obj2 = CS.UnityEngine.GameObject("有参生成")
```

**注意：**
为方便使用 和 节约性能 定义全局变量 用于存储 C#中的类

```lua
GameObject = CS.UnityEngine.GameObject
local obj3 = GameObject("优化性能生成")
```

#### 2.）调用成员变量

得到类对象中的成员变量直接`.` 调用即可

```lua
local obj4 = GameObject("调用成员变量")
print(obj4.transform.position)
```

#### 3.）调用静态函数

C# 类中的静态函数直接 `.` 调用即可

```lua
local obj5 = GameObject.Find("静态函数调用")
```

#### 4.）调用成员方法

C# 类中的成员方法一定要`：`调用！！！

```lua
Vector3 = CS.UnityEngine.Vector3
Debug = CS.UnityEngine.Debug

local obj6 = GameObject("调用成员方法")
obj6.transform:Translate(Vector3.right)
Debug.Log(obj6.transform.position)
```

#### 5.）自定义类的调用

使用方法相同 只是命名空间不同

UnityCS 脚本中

```cs
public class Test
{
    public void Speak(string str)
    {
        Debug.Log("Test1" + str);
    }
}
namespace MrTang
{
    public class Test2
    {
        public void Speak(string str)
        {
            Debug.Log("Test2" + str);
        }
    }
}
```

Lua 脚本

```cs
local t = CS.Test()
t:Speak("testSpeak")
local t2 = CS.MrTang.Test2()
t2:Speak("test2Speak")
```

#### 6.）继承 Mono 的类

继承 Mono 的类是不能 new 的

```Lua
local obj5 = GameObject("加脚本测试")
-- 通过GameObject的AddComponent添加脚本
-- xLua提供了重要方法 type 可以得到类的Type
-- xLua中不支持 无参泛型函数 所以使用传Type的重载
obj5:AddComponent(typeof(CS.LuaCallCS))
```

## 二、调用 C# 中的枚举

### 1.如何调用

和类一样 的调用方法
`CS.命名空间.枚举名.枚举成员`

```Lua
PrimitiveType = CS.UnityEngine.PrimitiveType
GameObject = CS.UnityEngine.GameObject

local obj = GameObject.CreatePrimitive(PrimitiveType.Cube)

--自定义枚举
E_MyEnum = CS.E_MyEnum

local c = E_MyEnum.Idle
print(c)
```

### 2.枚举的转换

```Lua
--数值转枚举
local a = E_MyEnum.__CastFrom(1)
print(a)
--字符串转枚举
local b = E_MyEnum.__CastFrom("Atk")
print(b)
```
