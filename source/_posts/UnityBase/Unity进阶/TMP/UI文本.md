---
title: UI文本控件
tags:
  - Unity客户端
  - Unity进阶
  - TextMeshPro
categories:
  - [Unity客户端, Unity进阶，TextMeshPro]
author:
  - nightstardawn
---

# UI 文本控件 Text(TextMeshPro - Text(UI))

## 一、文本输入相关

![ 2024-10-17 135119.png](https://s2.loli.net/2024/10/17/noIR8DJsBGL25yl.png)

### 1.输入框

### 2.Enable RTL Editor

启用 RTL 编辑器
开启该选项后，可以从右向左显示文本
并且会在下方看到一个额外的输入框 RTL Text Input
可以在其中查看反转的文本并直接对它进行编辑
![ 2024-10-17 135242.png](https://s2.loli.net/2024/10/17/sav8TGf9Y1BezF6.png)

### 3.Text Style (文本样式)

可以设置文本控件的样式

- H1、H2、H3：代表标题级别，数字越大，表示级别越低
- C1、C2、C3：代表颜色级别，用于定义不同文本的颜色，通常用于区分信息
- Quote：代表引用文本样式，一般表示引用别人的内容
- Link：超链接文本格式样式
- Title：标题样式
- Normal：普通正文文本样式

## 二、字体相关

![ 2024-10-17 135432.png](https://s2.loli.net/2024/10/17/XrLe8ap9QOlFNgf.png)

### 1.FontAsset

**字体资源**

### 2.MaterialPreset

**材质预设**

TMP 字体的显示，都是基于材质球
我们可以切换材质球来达到不同的渲染效果
本质就是材质球上使用的 Shader 着色器不同
![ 2024-10-17 135846.png](https://s2.loli.net/2024/10/17/y7CkPMDYFVqgIpc.png)

### 3.Font Style

**字体的样式**

- B：加粗
- I：斜体
- U：下划线
- S：删除线
- ab：小写文本
- AB：大写文本
- SC：大写文本，但是以实际输入的字母大小显示

### 4.AutoSize

**自动调节大小**

消耗较大，尽量少用
勾选后出现更多设置
![ 2024-10-17 135720.png](https://s2.loli.net/2024/10/17/G13bKIRrSkUaEVT.png)

- Min：字体最小大小
- Max：字体最大大小
- WD%：水平挤压字符，使它们更高，可以在一行中显示更多内容
- Line：减少行间距，只能为负数

## 三、颜色相关

![ 2024-10-17 141546.png](https://s2.loli.net/2024/10/17/oT5UJMFnNcbv8AE.png)

### 1.Vertex Color

**顶点颜色**

材质和纹理的颜色会乘以该颜色
决定字体颜色

### 2.ColorGradient

**颜色渐变**
勾选后会出现新的参数
![ 2024-10-17 141758.png](https://s2.loli.net/2024/10/17/3VzPyQS1TxBpb9g.png)

- Color Preset 颜色预设
- Color Mode 颜色模式
  - Single：和顶点颜色叠加
  - Horizontal Gradient：双色水平渐变
  - Vertical Gradient：双色垂直渐变
  - Four Corners Gradient：四色四角渐变
- Colors 根据颜色模式决定有几个颜色设置

### 3.Override Tags

启用此选项可忽略任何富文本标记更改文本颜色
一般不勾选

## 四、间距相关

![ 2024-10-17 142013.png](https://s2.loli.net/2024/10/17/8WjnVDhEHZPtm47.png)

- Character 字符间距
- Word 单词间距
- Line 行间距(自动换行)
- Paragraph 段落间距(段落由显示换行符定义)

## 五、对齐相关

![ 2024-10-17 142213.png](https://s2.loli.net/2024/10/17/FYEOIf1h5tDgS6Q.png)

## 六、包裹、溢出相关

![ 2024-10-17 150729.png](https://s2.loli.net/2024/10/17/slP8ZrStdQ6zn9o.png)

### 1.Warpping

**包裹**

- Disabled：禁用后，不会因为控件大小的变化而自动换行了
- Enabled：启用后，当控件大小变化时，会自动换行去适应它（比如自动换行）

### 2.Overflow

**溢出**

当文本不适合显示区域时
应该如何处理

- Overflow (溢出)
  扩展到显示区域外（但是如果启用了包裹，会自动换行）
- Ellipsis (省略)
  阶段文本可视范围外内容用省略号代替
- Masking (遮罩)
  和 overflow 类似，但是会隐藏显示区域外的所有内容
- Truncate (截断)
  超出范围的内容不再显示
- Scroll Rect (滚动矩形)
  此选项仅用于与较旧的 TextMesh Pro 项目兼容。对于新项目，请改用遮罩模式
- Page (分页)
  将文本剪切成多个页面，每个页面都适合显示区域
  您可以选择要显示的页面
- Linked (联系)
  将文本扩展到您选择的另一个 TextMesh Pro 游戏对象
  该 Text 显示不完，在另一个联系的 Text 对象中显示

## 七、UV 映射相关

![ 2024-10-17 151158.png](https://s2.loli.net/2024/10/17/Ku1Yxgk5dicjt9L.png)

**主要选择参数：**

- Character： 在每个文字上水平拉伸纹理
- Line： 每条线的整个宽度上水平拉伸纹理
- Paragraph： 整个文本中水平拉升纹理
- Match Aspect： 水平缩放纹理，保持纵横比，不变形

Line Offset：
Horizontal Mapping 设置为 线段(Line)、段落(Paragraph)、匹配模式(Match Aspect)时，会出现该参数，用于控制偏移位置

## 八、额外设置

![ 2024-10-17 151552.png](https://s2.loli.net/2024/10/17/gqVbT6vIm1p9OlH.png)

### 1.Margins

可以设置**文本和文本容器之间的间距**
负值可以让文本超出边框
也可以直接在 Scene 窗口中直接操作黄色边框

### 2.Geometry Sorting

**几何排序**
决定 TMP 重合时如何进行排序

- Normal
  按照显示顺序排序
  当四边形重合时，靠摄像机近的显示在前面
- Reverse
  按相反的顺序绘制四边形
  重合时，离摄像机远的显示在前面

### 3.Is Scale Static

告诉 TMP 文本系统该文本不会发生缩放相关变化
TMP 会跳过与缩放相关的计算，从而减少 CPU 和 GPU 的负担，提升性能

适用场景：适用于在场景中缩放比例固定的文本对象，可以开启该选项

### 4.Rich Text

是否开启富文本，默认开启
开启后可以识别富文本相关的关键字

### 5. Raycast Target

**摄像检测目标**
决定文本是否能被点击、触摸等事件响应
关闭后，触摸和点击会“穿透”
如果希望文本响应点击等事件，需要勾选

### 6.Maskable

**是否能被遮罩裁剪**
勾选时，TMP 会被 Mask 组件裁剪
取消勾选，不会被裁剪

### 7.Parse Escape Characters

**是否解析转意字符**
如果开启文本会解析转义字符，否则无用

### 8.Visible Descender

**可见下降**
使用脚本缓慢显示文本时，启用该选项
启用它可现实底部文本，并在显示新行时向上移动
需要把垂直对齐改为 Bottom 下部对齐

### 9.Sprite Asset

**Sprite 资源**
允许文本中嵌入精灵 2D 图片，用于处理图文混排时很重要
比如嵌入表情符号等等

### 10.Style Sheet Asset

管理和应用文本样式，使文本格式更加高效和统一
TMP_Style Sheet 文件可以定义多种文本样式，并在多个文本组件中重复使用，确保一致性

### 11.Kerning

**自动调整字符间距**
提高文本可读性和美感

### 12.Extra Padding

**额外填充**
为文本的边界添加额外填充，避免文本与边界过于靠近，提升视觉效果

## 九、脚本控制

### 1. 脚本获取 TMP UI 组件

```cs
public TextMeshProUGUI tmpUIText;
```

### 2.常用变量

```cs
//常用属性
//1.文本内容
tmpUIText.text = "123abc";
//2.字体文件
//tmpUIText.font
//3.字体大小
tmpUIText.fontSize = 18;
//4.颜色
tmpUIText.color = Color.black;
//5.对其方式
tmpUIText.alignment = TextAlignmentOptions.Center;
//6.行间距
tmpUIText.lineSpacing = 4;
//7.是否启用富文本
tmpUIText.richText = true;
//其他具体参考Inspector窗口中的内容
```

### 3.常用方法

```cs
//常用方法
//1.修改内容 支持富文本
tmpUIText.SetText("KFCV50");
//2.强制重新构建文本网格，通常在文本内容或样式更改后使用
tmpUIText.Rebuild(UnityEngine.UI.CanvasUpdate.Prelayout);
//UnityEngine.UI.CanvasUpdate枚举的含义
//Playerout 布局前调用
//Layout 布局时调用
//PostLayout 布局后调用
//PreRender 渲染前调用
//LatePreRender 渲染后调用
//MaxUpdateValue 最后调用
//3.运行时动态更改文本时， 强制更新文本网格
tmpUIText.ForceMeshUpdate();
//4.获取文本中的字符串
int lenght = tmpUIText.text.Length;
```

### 4.事件监听

通过 EventTrriger 的方式添加监听

```cs
EventTrigger eventTrigger = gameObject.AddComponent<EventTrigger>();
EventTrigger.Entry entry = new EventTrigger.Entry();
entry.eventID = EventTriggerType.PointerUp;
entry.callback.AddListener(MyPointerUpEvent);
eventTrigger.triggers.Add(entry);


private void MyPointerUpEvent(BaseEventData arg)
{

}
```
