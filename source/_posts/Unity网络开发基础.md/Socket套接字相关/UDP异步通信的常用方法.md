---
title: UDP异步通信的常用方法
tags:
  - Unity客户端
  - Unity网络开发
categories:
  - [Unity客户端, Unity网络开发]
author:
  - nightstardawn
---

# UDP异步通信的常用方法

</br>这些方法和TCP的异步方法使用类似
</br>使用方法参考TCP的异步方法
## 一、Begin相关的方法

### 1.BeginSendTo
```csharp
byte[] bytes = Encoding.UTF8.GetBytes("Hello World");
EndPoint ipPoint = new IPEndPoint(IPAddress.Parse("127.0.0.1"), 8080);
socketUdp.BeginSendTo(bytes, 0, bytes.Length, SocketFlags.None,ipPoint, SendOver, socketUdp);

private void SendOver(IAsyncResult result)
{
    try
    {
        Socket socket = (result.AsyncState as Socket);
        socket.EndSendTo(result);
        print("发送成功");
    }
    catch (SocketException e)
    {
        Console.WriteLine(e);
        throw;
    }
}

```
### 2.BeginReceiveTo
```csharp
private byte[] cacheBytes = new byte[512];

EndPoint ipPoint2 = new IPEndPoint(IPAddress.Any, 0); 
socketUdp.BeginReceiveFrom(cacheBytes, 0, cacheBytes.Length, SocketFlags.None, ref ipPoint2, ReceiveOver, (socketUdp,ipPoint2));

private void ReceiveOver(IAsyncResult result)
    {
        try
        {
            (Socket socket, EndPoint ipPoint)  info = ((Socket, EndPoint) )result.AsyncState;
            //返回值 就是接收到了多少个字节
            int num = info.socket.EndReceiveFrom(result, ref info.ipPoint);
            //处理消息
            
            //继续接收消息
            info.socket.BeginReceiveFrom(cacheBytes, 0, cacheBytes.Length, SocketFlags.None, ref info.ipPoint, ReceiveOver, info);
        }
        catch (SocketException e)
        {
            Console.WriteLine(e);
            throw;
        }
    }
```
## 二、Async相关的方法
### 1.SendToAsync

```csharp
SocketAsyncEventArgs args = new SocketAsyncEventArgs();
//设置发送的数据
args.SetBuffer(bytes, 0, bytes.Length);
//设置发送完成后的执行的事件
args.Completed += SendToOverAsync;
socketUdp.SendToAsync(args);


private void SendToOverAsync(object socket, SocketAsyncEventArgs args)
{
    if (args.SocketError == SocketError.Success)
    {
        print("发送成功");
    }
    else
    {
        print("发送失败");
    }
}
```

### 2.ReceiveToAsync

```csharp
SocketAsyncEventArgs args2 = new SocketAsyncEventArgs();
//设置发送的数据
args2.SetBuffer(cacheBytes, 0, cacheBytes.Length);
//设置发送完成后的执行的事件
args2.Completed += RecieveToOverAsync;
socketUdp.SendToAsync(args2);


private void RecieveToOverAsync(object s, SocketAsyncEventArgs args)
{
    if (args.SocketError == SocketError.Success)
    {
        print("接收成功");
        //具体接收了多少字节
        //args.BytesTransferred
        
        //可以通过一下两种方式获取收到的字节数组内容
        //args.Buffer;
        //cacheBytes;
        
        //解析消息
        Socket socket = s as Socket;
        //设置从第几个开始接和接多少
        args.SetBuffer(0, cacheBytes.Length);
        socket.ReceiveFromAsync(args);
    }
    else
    {
        print("接收失败");
    }
}
```
