---
title: Lua解析器
tags:
  - 程序语言
  - 热更新
  - AssetBundle
categories:
  - [热更新, AssetBundle]
author:
  - nightstardawn
---

# Lua 解析器

## 一、定义

引用命名空间：using XLua；
Lua 解析器能够让我们在 Unity 中执行 Lua

## 二、Lua 解析器的使用

```cs
//Lua解析器 一般保持唯一性
LuaEnv evn = new LuaEnv();

//执行Lua语言
evn.DoString("print('你好，世界')");
evn.DoString("print('你好，世界')","Lesson1_LuaEnv");
//DoString函数
//参数一：Lua语言
//参数二：Lua语言报错时，返回报错脚本位置
//执行Lua脚本
evn.DoString("require("Main")");
//**注意**
//默认的Lua脚本放在Resources文件下下 且要加上txt后缀才能使用执行
//帮助我们清除Lua中我们没有手动释放的对象 垃圾回收
evn.Tick();
//销毁解析器
evn.Dispose();
```

## 三、Lua 文件加载重定向

xLua 提供的路径重定向的方向的方法
`evn.AddLoader()`
允许我们自定义加载 Lua 文件的规则
当我们执行 Lua 语言 require 时 相当于执行一个 Lua 脚本
它就会执行 我们自定义传入的这个函数
![Snipaste_2024-09-09_16-40-41.png](https://s2.loli.net/2024/09/09/cJf3tkVlyZWNiw8.png)

```cs
LuaEnv evn = new LuaEnv();
evn.AddLoader(MyCustomLoader);
evn.DoString("require('Main')");
private byte[] MyCustomLoader(ref string fliePath)
{
  //传入的参数是 require 执行脚本的文件名
  //通过函数中的逻辑去加载Lua文件
  string path = Application.dataPath + "/Lua/" + fliePath + ".lua";
  //判断文件是否存在
  if (File.Exists(path))
  {
    return File.ReadAllBytes(path);
  }
  else
  {
    Debug.LogError("重定向失败,文件名是" + fliePath);
  }
  return null;
}
```
