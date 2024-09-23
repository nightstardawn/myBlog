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

## 三、调用数组，List，字典

数组，List，字典 在使用上，三个都遵循 C# 的规则

### 1.数组

```Lua
--C#怎么用 Lua怎么用
--1.长度 userdata
print(obj.array.Length)
--2.访问元素
print(obj.array[0])
--3.遍历
--注意： 虽然Lua中索引从1开始
--但是数组C#那边的规则
--注意最大值要-1
for i = 0, obj.array.Length-1 do
    print(i)
end

--4.Lua中创建C#的数组
--数组的本质：
--Array类
--创建C#数组 使用Array类中的静态方法
local array2 = CS.System.Array.CreateInstance(typeof(CS.System.Int32),10)
print(array2.Length)
```

### 2.List

```Lua
-- 1.添加元素
-- 注意调用成员方法 用冒号
obj.list:Add(1)
obj.list:Add(2)
obj.list:Add(3)
-- 2.长度
print(obj.list.Count)

--3.遍历
for i = 0, obj.list.Count-1 do
    print(obj.list[i])
end

--4.Lua中创建List

--老版本：< v2.1.12
local list2 = CS.System.Collections.Generic["List`1[System.String]"]()
print(list2)
list2:Add("123")
print(list2[0])
--新版本
--这里相当于得到了 字符串类型的List类型
local List_String = CS.System.Collections.Generic.List(CS.System.String)
local list3 = List_String()
print(list3)
list3:Add("123")
print(list3[0])
```

### 3.字典

```Lua
--1.添加元素
obj.dic:Add(1,"123")
print(obj.dic[1])
--2.遍历
for key, value in pairs(obj.dic) do
    print(key.. ""..value)
end
--3.Lua中创建字典
--这里相当于得到了对应类型
local dic2_String_Vector3 = CS.System.Collections.Generic.Dictionary(CS.System.String,CS.UnityEngine.Vector3)
local dic2 = dic2_String_Vector3()
dic2:Add("123",CS.UnityEngine.Vector3.right)
for key, value in pairs(dic2) do
    print(key,value)
end
--注意：在Lua中创建的字典 直接通过中括号 得到的时 nil
print(dic2["123"])
--如果要在Lua中创建的字典 得到对应的值 只能通过 get_Item方法
print(dic2:get_Item("123"))
--如果要在Lua中创建的字典 改变对应的值 只能通过 set_Item方法
dic2:set_Item("123",nil)
print(dic2:get_Item("123"))
```
