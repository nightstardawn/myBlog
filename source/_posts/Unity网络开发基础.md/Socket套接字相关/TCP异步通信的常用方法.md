---
title: TCP异步通信的常用方法
tags:
  - Unity客户端
  - Unity网络开发
categories:
  - [Unity客户端, Unity网络开发]
author:
  - nightstardawn
---

# TCP异步通信的常用方法

## 一、异步方法和同步方法的区别
- 同步方法:
  </br>方法中逻辑执行完毕后，通继续执行后面的方法
- 异步方法:
  </br>方法中逻辑可能还没有执行完毕，就继续执行后面的内容
  
</br>异步方法的本质 : 往往异步方法当中都会使用多线程执行某部分逻辑
</br>因为我们不需要等待方法中逻辑执行完毕就可以继续执行下面的逻辑了
</br>**注意:**unity中的协同程序中的某些异步方法，有的使用的是多线程,有的使用的是迭代器分步执行(关于协同程序可以回顾Unity基础当中讲解协同程序原理的知识点)
## 二、举例说明异步方法的原理

### 1.线程回调
```csharp
void CountDownAsync(int seconds,UnityAction callback)
{
    Thread t = new Thread(() =>
    {
        while (true)
        {
            print(seconds);
            Thread.Sleep(1000);
            --seconds;
            if(seconds == 0)
                break;
        }
        callback?.Invoke();
    });
    t.Start();
    print("开启倒计时");
}
```

### 2.async和await
</br>会等待线程执行完毕 继续执行后面的逻辑
</br>相对第一种方式，可以让函数分布执行

```csharp
public async void CountDownAsync(int seconds)
{
    print("倒计时开始");
    await Task.Run(() =>
    {
        while (true)
        {
            print(seconds);
            Thread.Sleep(1000);
            --seconds;
            if(seconds == 0)
                break;
        }
    });
    print("倒计时结束");
} 
```
## 三、Socket TCP通信中的异步方法

### 1.Begin开头的方法

</br>这种方法是内部开多线程，通过以回调的形式返回结果，需要和End相关方法配合使用
</br>回调函数参数`IAsyncResult`
- AsyncState 调用异步方法时传入的参数 需要转化
- AsyncWaitHandle 用于同步等待

| 服务器相关        | 客户端相关        | 两者通用                    |
|--------------|--------------|-------------------------|
| BeginAccecpt | BenginConnect | BeginReceive、EndReceive |
| EndAccept    | EndConnect   | BeginSend、EndSend       |



**使用示例：**
```csharp
Socket socketTcp = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
socketTcp.BeginAccept((result) =>
{
    //获取传入参数
    Socket s = result.AsyncState as Socket;
    Socket clientSocket = s.EndAccept(result);
},socketTcp);
```

```csharp
socketTcp.BeginReceive(resultBytes,0,resultBytes.Length,SocketFlags.None, ReceiveCallback,socketTcp);
void ReceiveCallback(IAsyncResult result)
{
    Socket s = result.AsyncState as Socket;
    //这个返回值就是 接收到的字节数
    int num = s.EndReceive(result);
    //进行消息处理
    //...
    //继续接收
    s.BeginReceive(resultBytes, 0, resultBytes.Length, SocketFlags.None, ReceiveCallback, s);
}
```
### 2.Async结尾的方法

</br>这种方法是内部躲开线程，通过回调形式返回结果，依赖SocketAsyncEventArgs对象配合使用

</br>关键变量：`SocketAsyncEventArgs`
</br>他会作为Async异步方法的传入值
</br>我们需要通过它来进行一些关键参数的赋值

| 服务端          | 客户端          | 服务端和客户端通用              |
|--------------|--------------|------------------------|
| AccecptAsync | ConnectAsync | SendAsync、ReceiveAsync |

使用示例

```csharp
SocketAsyncEventArgs e = new SocketAsyncEventArgs();
e.Completed += (socket, args) =>
{
    if (args.SocketError == SocketError.Success)
    {
        //获取连入的客户端
        Socket clientSocket = args.AcceptSocket;
        //继续监听接入
        (socket as Socket).AcceptAsync(args);
    }
    else
    {
        print(args.SocketError);
    }
};
socketTcp.AcceptAsync(e);
```
```csharp
SocketAsyncEventArgs e2 = new SocketAsyncEventArgs();
//设置字节容器，偏移位置，容量
e2.SetBuffer(resultBytes, 0, resultBytes.Length);
e2.Completed += (socket, args) =>
{
    if(args.SocketError == SocketError.Success)
        print("发送成功");
    else
    {
        
    }
};
socketTcp.SendAsync(e2);
```







