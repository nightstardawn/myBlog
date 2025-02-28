---
title: FTB关键类
tags:
  - Unity客户端
  - Unity网络开发
  - FTB相关
categories:
  - [Unity客户端, Unity网络开发]
author:
  - nightstardawn
---

# FTB关键类

## 一、NetworkCredential类

- 命名空间：System.Net
- NetworkCredential 通信凭证类
- 主要作用：用于Ftp文件传输时，设置账号密码

`NetworkCredential cred = new NetworkCredential("nightstardawn", "night123");`
## 二、FtpWebRequest类
- 命名空间：System.Net
- FtpWebRequest Ftp文件传输协议客户端操作类
- 主要作用：上传、下载、删除服务器上的文件

### 1.重要方法
#### 1.）Create

创建新的WebRequest,用于进行Ftp相关操作
</br>`FtpWebRequest req = FtpWebRequest.Create(new Uri("ftp://127.0.0.1/Test.txt"))as FtpWebRequest;`

#### 2.）Abort

如果正在进行文件传输，用此方法可以终止传输
</br>`req.Abort();`

#### 3.）GetRequestStream
用于获取上传的流

```csharp
Stream stream = req.GetRequestStream()
stream.Write();
```
#### 4.）GetResponse

返回FTP服务器的响应
</br>`FtpWebResponse res = req.GetResponse() as FtpWebResponse;`

### 2.重要成员

#### 1.）Credentials
通信凭证，设置为NetworkCredential对象
```csharp
NetworkCredential cred2 = new NetworkCredential("nightstardawn", "night123");
req.Credentials = cred2;
```

#### 2.）KeepAlive 

bool值，当完成请求时是否关闭到FTP服务器的控制连接（默认为true，不关闭）
</br>`req.KeepAlive = true;`

#### 3.）Method 
操作命令设置

| ftp命令                | 含义           |
|----------------------|--------------|
| WebRequestMethods    | Ftp类中的操作命令属性 |
| DeleteFile           | 删除文件         |
| DownloadFile         | 下载文件         |
| UploadFile           | 上传文件         |
| ListDirectory        | 获取文件的简短列表    |
| ListDirectoryDetails | 获取文件详细列表     |
| MakeDirectory        | 创建目录         |
| RemoveDirectory      | 删除目录         |


使用示例：
```csharp
req.Method = WebRequestMethods.Ftp.DeleteFile;
```
#### 4.）UseBinary

是否使用2进制传输
</br>`req.UseBinary = true;`
#### 5.）RenameTo

重命名
</br>`req.RenameTo = "RenameTest.txt";`

## 三、FtpWebResponse类

- 命名空间：System.Net
- 主要作用；
  - 它是用于封装FTP服务器对请求下载的响应
  - 它提供了操作状态以及服务器下载数据
- 通过FtpWebRequest对象中的`GetResponse()`获取
- 注意：使用完毕时，要使用Close释放

</br>FtpWebRequest设置好后
</br>通过它来真正的获取内容
```csharp
FtpWebResponse res = req.GetResponse() as FtpWebResponse;
```

### 1.）重要方法
1. Close 释放所有资源
2. GetResponseStream 返回从FTP服务器下载的数据流

### 2.）重要成员
| 成员名              | 含义                   |
|------------------|----------------------|
| ContentLength    | 接收到数据的长度             |
| ContentType      | 接收数据的类型              |
| StatusCode       | FTP服务器下发的最新状态码       |
| StatusDescription | FTP服务器下发的状态代码的文本     |
| BanerMessage     | 登录前建立连接时FTP服务器发送的消息  |
| ExitMessage      | FTP会话结束时服务器发送的消息     |
| LastModified     | FTP服务器上的文件的上次修改日期和时间 |