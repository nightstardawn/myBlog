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

## 四、调用拓展方法

- 注意：一定要加特性 [LuaCallCSharp]
- 建议：Lua 中 要使用的类 都加上该特性 可以提升性能
- 原因：如果不加该特性 除了拓展方法对应的类 其他类的使用 都不会报错
  但是 lua 是通过反射的机制去调用的 C# 效率低

CS 中

```cs
[LuaCallCSharp]
public static class Tools
{
    public static void Move(this Lesson4 obj)
    {
        Debug.Log(obj.name + "移动");
    }
}
public class Lesson4
{
    public string name = "tang";
    public void Speak(string str)
    {
        Debug.Log(str);
    }
    public static void Eat()
    {
        Debug.Log("吃东西");
    }
}
```

Lua 中

```lua
Lesson4 = CS.Lesson4
--使用静态方法
Lesson4.Eat()
--成员方法
local obj = Lesson4()
obj:Speak("hahhh")
--拓展方法
--使用方法和成员方法一致
obj:Move()
```

## 五、调用参数为 ref 和 out 的函数

```cs
public class Lesson5
{
    public int RefFun(int a, ref int b, ref int c, int d)
    {
        b = a + d;
        c = a - d;
        return 100;
    }
    public int OutFun(int a, out int b, out int c, int d)
    {
        b = a;
        c = d;
        return 200;
    }
    public int RefOutFun(int a, out int b, ref int c)
    {
        b = a * 10;
        c = a * 20;
        return 300;
    }
}
```

### 1.ref

ref 参数 会以多返回值的形式返回给 lua
如果函数存在返回值 那第一个值就是返回值
之后的值 就是 ref 的返回值 从左到右一一对应
ref 参数传入一个默认值 作为占位
以上面 RefFun 函数为例子：

- a 函数默认返回值
- b 第一个 ref
- c 第二个 ref
- d 最后一个参数

```lua
local a,b,c = obj:RefFun(1,0,0,1)
print(a)
print(b)
print(c)
```

### 2.out

out 参数 会以多返回值的形式返回给 lua
如果函数存在返回值 那第一个值就是返回值
之后的值 就是 out 的返回值 从左到右一一对应
out 参数 不需要传参数占位置

```lua
local a,b,c = obj:OutFun(20,30)
print(a)
print(b)
print(c)
```

### 3.ref 和 out 混合

ref 和 out 混合的规则
对应各个的规则即可

```lua
local a,b,c = obj:RefOutFun(20,1)
print(a)
print(b)
print(c)
```

## 六、函数重载

```cs
public class Lesson6
{
    public int Calc()
    {
        return 100;
    }
    public int Calc(int a, int b)
    {
        return a + b;
    }
    public int Calc(int a)
    {
        return a;
    }
    public float Calc(float a)
    {
        return a;
    }

}
```

```lua
local obj = CS.Lesson6()
print(obj:Calc())
print(obj:Calc(15,1))
--lua自己本身不支持重载
--但是lua支持调用C#中的重载函数

print(obj:Calc(10))
print(obj:Calc(10.2))

--lua虽然支持调用用c#重载函数
--但是因为 Lua中的数值类型只有number
--在C#中如果出现多精度的重载 可能会有出现问题
--解决方法：
--xlua提供了解决方案 ————反射Type
--这种方法只做了解 尽量别用 效率低

--得到函数的指定信息
local m1 = typeof(CS.Lesson6):GetMethod("Calc",{typeof(CS.System.Int32)})
local m2 = typeof(CS.Lesson6):GetMethod("Calc",{typeof(CS.System.Single)})

--通过xlua提供的方法 把他转成lua函数来使用
--一般我们转一次之后 反复使用
local f1 = xlua.tofunction(m1)
local f2 = xlua.tofunction(m2)
--成员方法：第一个参数 为 调用对象
--静态方法：不用传对象
print(f1(obj,10))
print(f2(obj,10.2))

```

## 七、委托和事件

```cs
public class Lesson7
{
    public UnityAction del;
    public event UnityAction eventAction;

    public void DoEvent()
    {
        eventAction?.Invoke();
    }
    public void ClearEvent()
    {
        eventAction = null;
    }
}
```

```lua

local obj = CS.Lesson7()

--委托是用来装函数的
--使用C#中的委托 就是用来装lua中的函数的
local fun = function ()
    print("Lua函数fun")
end

--lua中没有符合函数 不能+=
--注意：如果第一次往委托中加函数 因为是nil 不能+
--所以第一次 要先=
obj.del = fun
obj.del = obj.del + fun
obj.del = obj.del - fun
obj.del()
--清空委托
obj.del = nil
-- 事件+—函数 和 委托完全不一样
--要将添加事件当作成员方法来使用
obj:eventAction("+",fun)
obj:eventAction("-",fun)
obj:DoEvent()
--事件不能直接设置为空
--只能在C#中通过成员方法 包裹一层 置空方法
obj:ClearEvent()
```

## 八、特殊问题

### 1.二维数组

```cs
public class Lesson8
{
    public int[,] array = new int[,] { { 1, 2, 3 }, { 4, 5, 6 } };
}
```

```lua
local obj = CS.Lesson8()

--获取长度
print("行："..obj.array:GetLength(0))
print("列："..obj.array:GetLength(1))
--获取元素
--注意
--不能通过obj.array[0，0] obj.array[0][0]的方式访问
--通过F12访问数组 可知 数组内有GetValue和SetValue的方法
print(obj.array:GetValue(0,0))
print(obj.array:GetValue(1,0))
print("************************")
for i = 0, obj.array:GetLength(0) - 1 do
    for j = 0, obj.array:GetLength(1) - 1 do
       print(obj.array:GetValue(i,j))
    end
end
```

### 2.null 和 nil 的比较

```lua
--场景：
--往场景对象上添加脚本 如果存在就不添加 不存在再添加

GameObject = CS.UnityEngine.GameObject
Rigidbody = CS.UnityEngine.Rigidbody

local obj = GameObject("测试脚本")
--获取身上的刚体组件 如果没有就加 有就不管
local rig = obj:GetComponent(typeof(Rigidbody))
--注意在lua中nil和null无法 == 比较
-- 所以我们通过Equals方法进行比较
--但对象本来就是nil，就会出问题
--综上：一般我们会自己写一个全局方法来判空
function IsNull(obj)
    if obj==nil or obj:Equals(nil) then
        return true
    end
    return false
end
if rig:Equals(nil) then
    rig = obj:AddComponent(typeof(Rigidbody))
end
if IsNull(rig) then
    rig = obj:AddComponent(typeof(Rigidbody))
end
```

## 九、协程函数

```lua
--xlua 提供的工具表
--一定是要require调用之后 才能使用
util = require("util")
--C#中协程启动 都是继承了Mono的类 通过里面的启动函数StartCoroutine

GameObject = CS.UnityEngine.GameObject
WaitForSeconds =CS.UnityEngine.WaitForSeconds
--在场景中新建一个空物体 然后挂载一个脚本去 继承mono使用它来开启协程
local obj = GameObject("Coroutine")
local mono = obj:AddComponent(typeof(CS.LuaCallCS))

fun = function ()
    local a = 1
    while true do
        --lua中不能直接使用 c#中的yield reture
        --就使用lua中的协程返回
        coroutine.yield(WaitForSeconds(1))
        print(a)
        a = a + 1
        if a>10 then
            mono:StopCoroutine(b)
        end
    end
end
--注意：
--我们不能将lua函数 传入到C#中的开启协程函数中


--使用C#中的开启协程函数，必须先调用xlua中的util.cs_generator
b = mono:StartCoroutine(util.cs_generator(fun))
```
