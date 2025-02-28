---
title: FTB上传
tags:
  - Unity客户端
  - Unity网络开发
  - FTB相关
categories:
  - [Unity客户端, Unity网络开发]
author:
  - nightstardawn
---

# FTB上传

## 一、基本思路

1. 创建一个FTB连接
2. 设置通信凭证(如果不支持匿名，就必须设置这一步)
   </br>请求完毕后，必须关闭控制连接，如果想要关闭，可以设置成false
3. 设置操作命令
4. 指定传输类型
5. 得到用于上传的流对象
6. 开始上传

## 二、实现示例

```csharp
//1.创建Ftp连接
FtpWebRequest req = FtpWebRequest.Create(new Uri("ftp://127.0.0.1/pic.png")) as FtpWebRequest;
//2.设置通信凭证
NetworkCredential n = new NetworkCredential("nightstardawn", "night123");
//3.设置操作命令
req.Credentials = n;
//传输完成后是否关闭连接
req.KeepAlive = false;
//4设置传输命令
req.Method = WebRequestMethods.Ftp.UploadFile; //设置命令操作为上传文件
//5.指定传输类型
req.UseBinary = true;
//6.得到用于上传的流对象
Stream upDateStream = req.GetRequestStream();
//7.开始上传
using (FileStream file = File.OpenRead(Application.streamingAssetsPath + "/test.png"))
{
    //这里我们可以一点一点的把这个文件中的字节数据读取出来 然后存入到上传流中
    byte[] bytes = new byte[1024];
    //返回值 是真正从文件中读取了多少字节
    int contetntLenght = file.Read(bytes, 0, bytes.Length);
    //不停的去读取文件中的字节 除非读取完毕了 不然一直读 并且写入到上传流中
    while (contetntLenght != 0)
    {
        //写入上传流中
        //Write内部自己会记录写入位置，这里的偏移和长度是写入字节数组的
        upDateStream.Write(bytes, 0, contetntLenght);
        //写完了继续读
        contetntLenght = file.Read(bytes, 0, bytes.Length);
    }
    //出循环就证明 写完了
    file.Close();
    upDateStream.Close();
    //上传完毕
    print("上传结束");
}
```