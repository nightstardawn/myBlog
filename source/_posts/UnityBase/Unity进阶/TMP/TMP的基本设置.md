---
title: TMP的基本设置
tags:
  - Unity客户端
  - Unity进阶
  - TextMeshPro
categories:
  - [Unity客户端, TextMeshPro]
author:
  - nightstardawn
---

# TMP 的基本设置

Project Setting 中 对 TextMeshPro 的基本设置
![ 2024-10-22 162230.png](https://s2.loli.net/2024/10/22/m3OGBLki8aZgRpy.png)

## 一、Default Font Asset

**默认字体设置**
![ 2024-10-22 162316.png](https://s2.loli.net/2024/10/22/L1zuPO67BgM3JpQ.png)

- Path：
  字体资源的存储位置

## 二、Fallback Font Assets

**默认备用字体资源**
![ 2024-10-22 162517.png](https://s2.loli.net/2024/10/22/7f9mTNDG8kRzglA.png)

## 三、Fallback Material Settings

**备用材质设置**
![ 2024-10-22 162517.png](https://s2.loli.net/2024/10/22/7f9mTNDG8kRzglA.png)
Match Material Presets：
匹配材质预设
启用后备用字体中的字形与主字体的样式匹配
让主字体和后备字体看起来类似

## 四、Dynamic Font System Setting

![ 2024-10-22 162600.png](https://s2.loli.net/2024/10/22/u6obKcSJwIidMyj.png)

- Get Font Features at Runtime：
  运行时获取字体功能
- Missing Character Unicode：
  当找不到字符时使用的替代字符
  默认 0 表示是一个正方形的轮廓
- Disable warnings
  启用后可避免 Unity 为每个缺失字形记录的警告

## 五、Text Container Default Setting

**文本容器默认设置**
![ 2024-10-22 162711.png](https://s2.loli.net/2024/10/22/R7iHrXCPqmhZ2BG.png)

- TextMeshPro：3D 文本默认大小
- TextMeshPro UI：UI 文本默认大小
- Enable Raycast Target：是否默认为 文本对象启用摄像目标检测
- Auto Size Text Container：启用后会自动调整文本容器大小
- Is Object Scale Static：是否是静态缩放

## 六、Text Component Default Setting

**文本组件默认设置**
![ 2024-10-22 162716.png](https://s2.loli.net/2024/10/22/9hZuD7eWPsvEzNk.png)

- Default Font Size：默认字体大小
- Text Auto Size Ratios：文本自动调整大小比例
- Word Wrapping：是否启用自动换行
- Kerning：是否启用字偶间距调整
- Extra Padding：是否额外进行填充，可以降低字符在 sprite 的边界处被截断的几率
- Tint All Sprites：为所有精灵图片着色
- Parse Escape Sequence：是否解析转义字符

## 七、Default Sprite Asset：

**默认精灵图片资源**
![ 2024-10-22 163014.png](https://s2.loli.net/2024/10/22/usJDNmbkzfgWd9C.png)

- Missing Sprite Unicode：
  精灵缺失时采用的替代付，默认为方形轮廓
- IOS Emoji Support：
  是否支持 IOS 表情符号
- Path：
  精灵图片资源存储位置

## 八、Default Style Sheet：

**默认样式表**
![ 2024-10-22 163018.png](https://s2.loli.net/2024/10/22/nkoT7AmBQPyqZGJ.png)

- Path：
  样式表资源存储位置

## 九、Color Gradient Presets：

**默认颜色渐变预设**
![ 2024-10-22 163023.png](https://s2.loli.net/2024/10/22/BLw6uV2stUEFYIc.png)

- Path：
  颜色渐变预设存储位置

## 十、Line Breaking for asian languages：

**用于处理亚洲语言的换行规则**
![ 2024-10-22 163027.png](https://s2.loli.net/2024/10/22/2LGJrAsKYMoT6Cz.png)

## 十一、Korean Language Options

**韩语相关设置**
