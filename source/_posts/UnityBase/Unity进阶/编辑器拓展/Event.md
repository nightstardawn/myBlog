---
title: Event
tags:
  - Unity客户端
  - Unity进阶
  - 编辑器拓展
categories:
  - [Unity客户端, 编辑器拓展]
author:
  - nightstardawn
---

# Event

## 一、Event公共类的作用

主要是提供了许多属性和方法，允许你检查和处理用户输入
主要用于在Unity编辑器拓展开发中

因为Input相关输入检，需要在运行条件下才能监听
而Event专门提供给编辑器模式下使用，可以帮助我们检测鼠标输入等事件相关操作
在OnGUI和OnScreenView中都能使用

## 二、重要api
| 重要api                       | 含义                                           |
|-----------------------------|----------------------------------------------|
| Event.Current               | 获取当前事件                                       |
| Event.current.alt           | alt键是否按下                                     |
| Event.current.shift         | shift键是否按下                                   |
| Event.current.control       | control键是否按下                                 |
| Event.current.isMouse       | 是否是鼠标事件                                      |
| Event.current.button        | 判断鼠标左中右键<br>(0,1,2分别代表左中右)                   |
| Event.current.mousePosition | 鼠标位置(基于窗口的位置)                                |
| Event.current.isKey         | 判断键盘是否输入                                     |
| Event.current.character     | 获取键盘输入的字符                                    |
| Event.current.keyCode       | 获取键盘输入对应的KeyCode                             |
| Event.current.type          | 判断输入的类型<br>使用EventType枚举进行比较即可               |
| Event.current.capsLock      | 是否锁定大写                                       |
| Event.current.command       | Win键或者command键是否按下                           |
| Event.current.commandName   | 判断是否触发了对应的键盘事件(例如：Copy、Paste、Cut)<br>可以自定义名字 |
| Event.current.delta         | 鼠标间隔移动距离                                     |
| Event.current.functionKey   | 判断功能键是否输入(例如：方向键、backspace等)                 |
| Event.current.numberic      | 小键盘是否开启                                      |
| Event.current.Use()         | 避免组合键冲突                                      |

## 三、其他api

查看官网