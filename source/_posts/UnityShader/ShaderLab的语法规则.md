---
title: ShaderLab 的语法规则
tags:
  - Shader
  - ShaderLab
  - SubShader
  - 备用 Shader
categories:
  - Unity技术美术
author:
  - nightstardawn
---

# ShaderLab 的语法规则

## 一、shader 的名字

1. 直接修改 shader 文件中 Shader 后面的名字
2. Shader 后面的名字决定了在材质面板的选择路径
   **注意:**
   1. 不要使用中文路径
   2. Shader 的文件名和在文件中的命名建议保持一致

## 二、Shader 的属性 Properties

### 1. Shader 属性的作用

在 shader 编写时我们经常会用到不同类型的变量或贴图等资源，为了==增加 shader 的可调节性==，有些==变量==不会直接在 shader 程序中写死，而是==作为开放的属性显示在我们的材质面板上==，供我们使用时调节，而这些开放的属性就是通过属性来定义的

### 2. Shader 属性的特点

1.  可以在材质面板进行编辑
2.  可以作为后续输入变量提供给所有子着色器使用

### 3.Shader 属性的基本语法

#### 1.） 属性的基本语法

```cs
_Name("Display Name",type) =defaultValue[{options}]

//_Name:属性名 规则是在名字前加下划线 方便后续获取
//Display Name； 材质面板上显示的名字 ！！！不要为中文！！！
//type：属性的类型
//defaultValue：将Shader指定给某个材质时初始化的默认值
```

#### 2.） Unity Shader 中的属性主要分为三大类

1. 数值
2. 颜色和向量
3. 纹理贴图

##### A.）数值类型

数值类型有三种

1. 整形 `_Name("Display Name",Int) =number`
2. 浮点型 `_Name("Display Name",Float) =number`
3. 范围浮点型 `_Name("Display Name",Range(min,max)) =number`

   **注：**
   UnityShader 中的==数值类型属性==就基本都是==浮点类型==（Float）数据
   虽然提供了很多整形（int），但是编译时最终都会转为浮点型
   因此 我们更多的使用的还是 Float 类型

##### B.） 颜色和向量

颜色（RGBA）和向量（XYZW）均是由四个分量代表
均由四个数组成的类型标表示

1. 颜色

```cs
_Name("Display Name",color) =(num1,num2,num3,num4)
//注意 ：颜色值中的RGBA的取值范围 是 0~1 （0~255）
```

2. 向量

```cs
_Name("Display Name",Vector) =(num1,num2,num3,num4)
//注意 ：向量的取值没有限制
```

##### C.） 纹理贴图

纹理的类型分为四种

- **2D 纹理**（最常用的纹理，漫反射贴图，法线贴图都属于 2D 纹理）
  `_Name("Display Name",2D) ="defaulttexture"{}`
- **2DArray 纹理** （纹理数组，允许在纹理中存储多层图像数据，每一层看作一个 2D 图像，一般使用脚本创建，使用较少）
  `_Name("Display Name",2DArray) ="defaulttexture"{}`
- **Cube map texture 纹理** 纹理（立方体纹理，由于前后左右上下有联系的 2D 贴图拼成的立方体，比如天空盒和反射探针）
  `_Name("Display Name",Cube) ="defaulttexture"{}`
- **3D 纹理**（一般由脚本创建，极少使用）
  `_Name("Display Name",Cube) ="defaulttexture"{}`

  **注：**

  1. 关于 defaulttexture 默认值取值
     1. 不写:默认贴图为空 1
     2. white:默认白色贴图(RGBA:1,1,1,1)
     3. black:默认黑色贴图(RGBA:0,0,0,1)1
     4. gray:默认灰色贴图(RGBA:0.5,0.5,0.5,1)
     5. bump:默认凸贴图(RGBA:0.5,0.5,1,1),一般用于法线贴图默认贴图
     6. red:默认红色贴图(RGBA:1,0,0,1)
  2. 关于默认值后面的 {},固定写法(老版本中括号内可以控制固定函数纹理坐标的生成，但是新版本中没有该功能了)

## 三、Shader 的子着色器 SubShaderder

### 1、SubShader 的作用

SubShader 中包含对了最终渲染相关代码，决定了最终的渲染效果
每一个 Shader 中至少包含一个 SubShader，当 Unity 想要显示一个物体时，就会去文件中检测 SubShader，选择第一个能在当前显卡运行的 SubShader
因此，实现高级效果时，为了避免某些设备上无法执行，可能会存在多个 SubShader，用于适配低端设备

### 2、基本构成

SubShader 最主要有三个部分构成

- 渲染标签：通过标签来确定什么时候以及如何对物体进行渲染
- 渲染状态：通过状态来确定渲染时的剔除方式、深度测试方法、混合内容等等
- 渲染通道：具体实现着色器代码的地方（至少有一个，允许有多个）

### 3、基本格式

```cs
SubShader
{
  //1.渲染标签
  Tags{"标签一"="标签值1" "标签二="标签值2"...}
  //2.渲染状态
  //......
  //3.渲染通道
  Pass
  {
    //第一个渲染通道
  }
  Pass
  {
    //第二个渲染通道
  }
}

```

**注：** SubShader 中每定义一个 Pass，就会让物体执行一次渲染，所以我们在不影响效果的情况下，要尽可能的减少 Pass。

### 4、SubShadre 构成的具体内容

#### 1.） 渲染标签

##### A.） 渲染队列 Queue

1. 作用：确定物体的渲染顺序
2. 格式：`Tags{"Queue"="标签值"}`
3. 常用 Unity 预先定义好的队列
   1. **BackGrand**(背景)(队列号：1000)
      最早渲染的物体队列，一般用于渲染天空盒和背景
      `Tags{"Queue"="BackGrand"}`
   2. **Geometry**(几何)（队列号：2000）
      不透明的几何体通常使用该队列，当没有声明渲染队列时，Unity 默认会使用这个队列
      `Tags{"Queue"="Geometry"}`
   3. **AlphaTest**(透明测试)(队列号：2450)
      有透明通道的，需要进行 Alpha 测试的几何体会使用该队列
      注：当所有 Geometry 队列实体都绘制完成后再绘制 AlphaTest 队列，效率更高
      `Tags{"Queue"="AlphaTest"}`
   4. **Transparent**(透明的)(队列号：3000)
      队列中几何体按照由近到远的顺序进行绘制，半透明的物体的渲染队列，所有进行透明混合的几何体都应该使用该队列
      比如：玻璃材质、粒子特效等
      `Tags{"Queue"="Transparent"}`
   5. **Overlay**(覆盖)(队列号：4000)
      放在最后的渲染队列，于叠加渲染的效果
      比如镜头光晕等
      `Tags{"Queue"="Overlay"}`
4. 自定义队列
   基于 Unity 预先定义好的这些渲染队列标签来进行加减运算来定义自己的渲染队列
   比如：
   `Tags{"Queue"="Geometry+1"}`
   `Tags{"Queue"="Transparent-1"}`
   主要用于特殊情况下使用，例如：
   一些水的渲染 想要在不透明物体之后，半透明之前进行渲染，就可以自定义
   **注：**
   1. 自定义队列只能基于预先定义好的各类型进行计算，不能再 Shader 中直接赋值数字
   2. 如果需要直接赋值数字，可以材质面板中进行设置
   3. 运算符之间不用空格

##### B.）渲染类型 RenderType

1. 作用：对着色器进行分类，之后用于着色器的替换功能
   摄像机上由专门的 API，可以指定这个渲染类型来替换成别的着色器
2. 格式：`Tags{"RenderType"="标签值"}`
3. 常用 Unity 预先定义好的渲染类型标签值
   1. **Opaque**(不透明的)
      用于普通的 Shader，比如：不透明、自发光、反射
   2. **Transparent**(透明的)
      用于半透明的 Shader，比如透明粒子
   3. **TransparentCutout**(透明切割)
      用于透明测试的 Shader，比如植物的叶子
   4. **BackGround**(背景)
      用于天空盒的 Shader
   5. **Overlay**(覆盖)
      用于 GUI 纹理、Halo(光环)、Flare(光晕)
4. 不常用的渲染类型标签值
   1. Treeopaque
      用于地形系统中的树干
   2. reeTransparentCutout
      用于地形系统中的树叶
   3. TreeBillboard
      用于地形系统中的 Billboarded 树
   4. Grass
      用于地形系统中的草
   5. GrassBillboard
      用于地形系统中的 Billboarded 草

##### C.）禁用批处理

1. 主要作用：当使用批处理时，模型会被变换到世界空间中，模型空间会被丢弃，这可能会导致某些使用模型空间顶点数据的 shader 最终无法实现想要的结果，可以通过开启禁用批处理来解决该问题
2. 格式：`Tags{"DisableBatching'= "True"}`
3. 了解内容
   LOD 效果激活时才会禁用批处理，主要用于地形系统上的树
   格式：`Tags{"DisableBatching"="LODFading"}`

##### D.）禁用投射阴影

1. 主要作用：控制该 subshader 的物体禁用投射阴影
2. 格式：`Tags{"ForceNoshadowCasting'= "True"}`

##### D.）禁用投影机

1. 主要作用：物体是否忽略 Projector(投影机)的投射，一般半透明 Shader 需要开启该标签
2. 格式：`Tags{"IgnoreProjector"="True"}`

##### E.）注意事项 ： 以上的标签均只能在 SubShader 中使用声明

#### 2.）渲染状态

##### A.）渲染状态的语法结构

1. 基本结构：**渲染状态 状态类型**
2. 渲染状态 是通过**渲染状态关键词+空格+状态类型**决定的
   如果存在多个类型，就通过空行隔开

##### B.）剔除方式

1. 主要作用：设置多边形的剔除方式，有背面剔除，正面剔除，不剔除
   剔除，即不渲染
2. 状态语法
   |代码|意义|
   |---|---|
   |Cull Back|背面剔除|
   |Cull Front|背面剔除|
   |Cull Off|不剔除|
   - 默认背面剔除

##### C.）深度缓冲

1. 主要作用 ： 是否写入深度缓冲
2. 深度缓冲（Depth Buffer）定义：
   深度缓冲是一个与屏幕像素对应的缓冲区，用于存储每个像素的深度值(距离相机的距离)，在渲染场景之前，深度缓冲被初始化为最大深度值，表示所有像素都在相机之外。最后留在深度缓冲中的信息会被渲染
3. 状态语法
   |代码|意义|
   |---|---|
   |ZWrite On|写入深度缓冲|
   |ZWrite Off|不写入深度缓冲|
   - 不设置默认写入
4. 一般我们做透明效果时，会设置的为不写入

##### D.）深度测试

1. 深度测试的定义：
   深度测试的主要目的是确保在渲染时，像素按照正确的深度(距离相机的距离)顺序进行绘制，从而创建正确的遮挡关系和透视效果，渲染场景之前，深度缓冲被初始化为最大深度值，表示所有像素都在相机之外。在渲染过程中，对于每个像素，深度测试会将当前像素的深度值与深度缓冲中对应位置的值进行比较。
2. 深度测试流程图
   ![深度测试流程图](https://s2.loli.net/2024/07/25/Itiv4AKkgNFnLxT.png)
3. 主要作用：
   设置深度测试的对比方式
4. 状态语法
5. | 代码           | 意义                                                     |
   | -------------- | -------------------------------------------------------- |
   | ZTest Less     | 小于当前深度缓冲中的值，就通过测试，写入到深度缓冲中     |
   | ZTest Greater  | 大于当前深度缓冲中的值，就通过测试，写入到深度缓冲中     |
   | ZTest LEqual   | 小于等于当前深度缓冲中的值，就通过测试，写入到深度缓冲中 |
   | ZTest GEqual   | 大于等于当前深度缓冲中的值，就通过测试，写入到深度缓冲中 |
   | ZTest Equal    | 等于当前深度缓冲中的值，就通过测试，写入到深度缓冲中     |
   | ZTest NotEqual | 不等于当前深度缓冲中的值，就通过测试，写入到深度缓冲中   |
   | ZTest Always   | 始终通过深度测试写入深度缓冲中                           |
   - 不设置的话，默认为 LEqual 小于等于
6. 一般情况下，我们只有在实现一些特殊效果时，才会修改深度测试的方法
   例如：透明物体渲染会了修改为 Less，描边会修改为 Greater 等

##### E.）混合方式

1. 混合流程
   ![混合流程图](https://s2.loli.net/2024/07/25/BcwdNtan7kZsy8Q.png)
2. 主要作用
   设置渲染图像的混合方式
3. 状态语法
   |状态|名称|
   |---|---|
   |Blend One One|线性减淡|
   |Blend SrcAlpha OneMinusSrcAlpha|正常透明混合|
   |Blend OneMinusDstColor One|滤色|
   |Blend DstColor Zero|正片叠底|
   |Blend DstColor SrcColor X|光片效果|
   |Blend One OneMinusScrAlpha|透明度混合|
   - 等等
   - 不设置默认不进行混合
4. 一般情况下，我们需要多种颜色叠加渲染时，就需要设置混合方式。具体情况具体处理

##### F.）其他渲染状态

1. LOD 控制 LOD 等级 在不同距离下使用不同渲染
2. ColorMask 设置颜色通道的写入蒙版，默认蒙版为 RGBA
3. ...

##### 3.）渲染状态的注意事项

以上这些状态不仅可以在 subshader 语句块中声明，之后讲解的 Pass 渲染通道语句块中也可以声明这些渲染状态，如果在 subshader 语句块中使用会影响之后的所有渲染通道 Pass，如果在 Pass 语句块中使用只会影响当前 Pass 渲染通道，不会影响其他的 Pass

#### 3.）Pass 渲染通道

##### A.） 语法结构

```cs
Pass
{
   1. Name 名称名
   2. 渲染标签
   3. 渲染状态
   4. 其他着色器代码
}
```

##### B.）具体部分介绍

###### a.）Pass 的名字

1.  主要作用：可以利用 UsePass 命令在其他 Shader 中当作复用该 Pass 的代码
2.  使用格式：UsePass "Shader 路径/Pass 名"
3.  注意：Unity 内部会把 Pass 名转化为大写字母，所以使用 UsePass 时，Pass 必须大写

###### b.）渲染标签

使用方法和 SubShader 中的渲染标签一致
格式：`Tags{"标签名"="标签值"...}`
**注意：** 之前 SubShader 标签不可以在 Pass 中使用，Pass 有自己的专门的渲染标签

1.  `Tag{"LightMode"="标签值"}`

    1.  主要作用：
        指定了该 Pass 应该在那个阶段执行
        可以将着色器代码分配给适当的渲染阶段，以实现所需效果
    2.  标签值

        1.  主要使用的标签值
            |标签值|意义|
            |---|---|
            |Always|始终渲染;不应用光照|
            |ForwardBase|在前向渲染中使用;应用环境光、主方向光、顶点/SH 光源和光照贴图|
            |ForwardAdd|在前向渲染中使用;应用附加的每像素光源(每个光源有一个通道)|
            |Deferred|在延迟渲染中使用;渲染 G 缓冲区|
            |ShadowCaster|将对象深度渲染到阴影贴图或深度纹理中|
            |MotionVectors|用于计算每对象运动矢量|
            |PrepassBase|在旧版延迟光照中使用;渲染法线和镜面反射指数|
            |PrepassFinal|在旧版延迟光照中使用;通过组合纹理、光照和反光来渲染最终颜色|
            |Vertex|当对象不进行光照贴图时在旧版顶点光照渲染中使用;应用所有顶点光源|
            |VertexLMRGBM|在光照贴图为 RGBM 编码的平台上(PC 和游戏主机)当对象不进行光照贴图时在旧版顶照渲染中使用|
            |VertexLM|当对象不进行光照贴图时在旧版顶点光照渲染中使用:在光照贴图为双 LDR 编码的平台上(移动平台)|
        2.  关于向前渲染、延迟渲染、旧版光照等[概念了解](https://docs.unity.cn/cn/2019.4/Manual/RenderingPaths.html)

2.  `Tag{"RequireOptions"="标签值"}`
    1. 主要作用：
       指定满足某些条件才渲染该 Pass
    2. 目前 Unity 仅仅支持"SoftVegetation"
       当 Quality 窗口开启了 SoftVegetation，才渲染通道
3.  `Tag{"PassFlags"="标签值"}`

    1. 主要作用：
       一个渲染通道 Pass 可以指示一些标志，来更改渲染管线向 Pass 传递数据的方式
    2. 目前 Unity 仅仅支持"OnlyDirectionnal"
       在 ForwardBase 向前渲染的通道类型中使用时，此标志的作用是仅允许主方向光和环境光/光照探针数据传递到着色器
       这意味着非重要光源的数据将不会传递到顶点光源或球谐函数着色器变量

###### c.）Pass 的渲染状态

1. 同 SubShader 的中的渲染状态
2. 例如：
   - 剔除方式 决定了模型的正面背面是否渲染
   - 深度缓冲和深度测试 决定了景深关系的确定以及透明效果的表达
3. **注意**：
   1. 在 SubShader 语句块中使用渲染状态 会影响之后的所有渲染通道 Pass
   2. 在 Pass 中使用渲染状态 只会影响当前的渲染通道 Pass

###### d.）其他着色器代码

其他代码部分就是实现着色器的核心代码
可能会用到 CG 或者 HLSL 等着色器语言来进行逻辑书写

###### e.）GrabPass 命令

1. 主要作用：
   把即将绘制对象时的屏幕抓取到纹理之中，在后续的通道中即可使用此纹理，从而就执行基于此图像的高级效果
2. 基本格式
   ```cs
   GrabPass
   {
      "变量名"
      "_BackgroudTexture"
   }
   ```
3. **注意**
   该命令一般写在某个 Pass 前，在之后的 Pass 代码中可以利用该变量进行处理

## 四、备用 Shader

1. 主要作用
   防止设备在执行渲染时，所有的 SubShader 都无法正常渲染
   起一个保险作用，让物体能够使用一个最低级的 Shader 去渲染
2. 语法结构

```cs
Fallback "Shader名"
//或者
Fallback Off
```
