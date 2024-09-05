---
title: AB包操作基础
tags:
  - 程序语言
  - 热更新
  - AssetBundle
categories:
  - [热更新, AssetBundle]
author:
  - nightstardawn
---

# AB 包操作基础

## 一、AB 包操作界面

### 1.Configure

![ 2024-09-05 195624.png](https://s2.loli.net/2024/09/05/zTBwmQAhEKRV743.png)

#### 主要作用

AB 包中内容信息

### 2.Build 页签

![ 2024-09-05 190216.png](https://s2.loli.net/2024/09/05/ZFCbkREGi15rQhd.png)

#### 参数相关

- BuildTarget:目标平台
- Output Path:目标输出路径
- Clear Folders:是否清空文件夹 重新打包,
- Copy To StreamingAssets:是否拷贝到 StreamingAssets
- Compression

  - NoCompression 不压缩，解压快，包较大 不推荐
  - LZMA 压缩最小。缺点:用一个资源 要解压所有 ，解压慢
  - LZ4 压缩，相对 LZMA 大一点点 用什么解压什么，内存占用低，建议使用

- Exclude Type Information 在资源包中 不包含资源的类型信息
- Force Rebuild 重新打包时需要重新构建包和
- ClearFolders 不同，它不会删除不再存在的包
- Ignore Type Tree Changes 增量构建检查时，忽略类型数的更改
- Append Hash 将文件哈希值附加到资源包名上
- Strict Mode 严格模式，如果打包时报错了，则打包直接失败无法成功
- Dry Run Build 运行时构建

### 3.Inspect 页签

![ 2024-09-05 193624.png](https://s2.loli.net/2024/09/05/caMtQREW657YyZG.png)

#### 主要作用

查看对应文件以及文件夹的 AB 包相关属性信息

## 二、AB 包资源加载

**注意：** 生成的 AssetBundle 文件夹，在项目打包时不会一起打包，所以一般，我们都会拷贝到 StreamingAssets 中

总体步骤：

1. 加载 AB 包
2. 加载 AB 包内的资源

## 1.同步加载

```cs
//加载AB包
AssetBundle ab = AssetBundle.LoadFromFile(Application.streamingAssetsPath + "/" + "test");
//AB包中资源 泛型加载
GameObject obj1 = ab.LoadAsset<GameObject>("Cube");
Instantiate(obj1);
//AB包中资源 使用tpye加载(后续使用lua代码时常用)
GameObject obj2 = ab.LoadAsset("Cube", typeof(GameObject)) as GameObject;
Instantiate(obj2);
```

## 2.异步加载

```cs
StartCoroutine(LoadABPackageAsset("test", "Cube"));

IEnumerator LoadABPackageAsset(string ABName, string assetName)
{
  //加载AB包
  AssetBundleCreateRequest abqr = AssetBundle.LoadFromFileAsync(Application.streamingAssetsPath + "/" + "test");
  yield return abqr;
  //加载AB包中的资源
  AssetBundleRequest abr = abqr.assetBundle.LoadAssetAsync(assetName, typeof(GameObject));
  yield return abr;
  //实例化
  Instantiate(abr.asset as GameObject);
}
```

## 3.资源卸载

```cs
//卸载所有加载的AB包 参数为ture 会把通过AB包加载的资源也卸载了
AssetBundle.UnloadAllAssetBundles(false);
//自己卸载
ab.UnLoad(flase);
```

## 二、AB 包的依赖关系

### 1.定义

> 一个资源身上用到了别的 AB 包中的资源 这个时候 如果只加载自己的 AB 包
> 通过它创建对象，会出现资源丢失的情况
> 这种时候 需要把依赖包 一起加载了 才能正常

### 2.加载相关依赖包的方法

```cs
//第一步 加载 AB包
AssetBundle ab = AssetBundle.LoadFromfile(Application.streamingAssetsPath + "/" + "model");
//依赖包的关键知识点一利用主包 获取依赖信息
//加载主包
AssetBundle abMain = AssetBundle.LoadFromfile(Application.streamingAssetsPath + "/" + "pc");
//加载主包中的固定文件
AssetBundleManifest abManifest = abMain.LoadAsset<AssetBundleManifest>("AssetBundleManifest");
//从固定文件中 得到依赖信息
string[] strs = abManifest.GetAllDependencies("model");
//得到了 依赖包的名字

//for循环加载包即可
```
