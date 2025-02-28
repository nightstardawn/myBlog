---
title: Socket
tags:
  - Unity客户端
  - Unity网络开发
categories:
  - [Unity客户端, Unity网络开发]
author:
  - nightstardawn
---

# Socket

## 一、Socket的作用

它是c#提供给我们用于网络通信的一个类(在其它语言当中也有对应的socket类)
- 类名:Socket
- 命名空间:System.Net.sockets 

socket套接字是支持**TCP/IP网络通信的基本操作单位**
</br>一个套接字对象包含以下关键信息
1. 本机的IP地址和端口
2. 对方主机的IP地址和端口
3. 双方通信的协议信息

</br>一个Sccket对象表示一个本地或者远程套接字信息
</br>它可以被视为一个数据通道
</br>这个通道连接与客户端和服务端之间
</br>数据的发送和接受均通过这个通道进行
</br>一般在制作长连接游戏时，我们会使用socket套接字作为我们的通信方案//我们通过它连接客户端和服务端，通过它来收发消息//你可以把它抽象的想象成一根管子，插在客户端和服务端应用程序上，通过这个管子来传递交换信息

## 二、Socket的分类

Socket套接字有3种不同的类型
1. 流套接字
   </br>主要用于实现TCP通信, 提供了面向连接、可靠的、有序的、数据无差错且无重复的数据传输服务
2. 数据报套接字
   </br>主要用于实现UDP通信，提供了无连接的通信服务，数据包的长度不能大于32K8，不提供正确性检查，不保证顺序，可能出现重发、丢失等情况
3. 原始套接字(不常用，不深入讲解)
   </br>主要用于实现IP数据包通信，用于直接访问协议的较低层，常用于侦听和分析数据包

## 三、Socket常用构造函数参数解释

常用构造函数：
</br>`public Socket(AddressFamily addressFamily, SocktType socketType, ProtocolType protocolType);`

### 1.AddressFamily

参数 : AddressFamily : 网络寻址 枚举类型，决定寻址方案
1. 常用:
   1. InterNetwork IPv4寻址
   2. InterNetwork6 IPv6寻址
2. 做了解 
   1. UNIX ： UNIX本地到主机地址
   2. ImpLink ： ARPANETIP地址
   3. IpX ： IPX或SPX他址
   4. Iso ： ISO协议的地址OSI协议的地址NetBios地t上本机ATM服务地址
   5. Osi ： OSI协议的地址
   7. NetBios ： NetBios地址
   8. Atm ： 本机ATM服务地址

### 2.SocktType

参数 : SocketType : 套接枚举类型，决定使用的套接字类型
1. 常用
   1. Dgram : 支持数据报，最大长度固定的无连接、不可靠的消息(主要用于UDP通信)
   2. Stream : 支持可靠、双向、基于连接的字节流(主要用于TCP通信)
2. 做了解:
   1. Raw : 支持对基础传输协议的访问
   2. Rdm : 支持无连接、面向消息、以可靠方式发送的消息
   3. Seqpacket : 提供排序字节流的面向连接且可靠的双向传输

### 3.ProtocolType

参数 ： ProtocolType 协议类型 枚举类型，决定套接字使用的通讯协议类型
1. 常用:
   1. TCP : TCP传输控制协议
   2. UDP : UDP用户数据报协议
2. 做了解：
   1. IP ： IP网际协议
   2. Icmp ： Icmp网际消息控制协议
   3. Igmp ： Igmp网际组管理协议
   4. GgP ：  网关到网关协议
   5. IPv4 ： Internet协议版本4
   6. Pup ： PARC通用数据包协议
   7. Idp ： Internet数据报协议
   8. Raw ： 原始IP数据包协议
   9. Ipx ：Internet数据包交换协议
   10. Spx ：  顺序包交换协议用于
   11. IcmpV6 ： IPv6的Internet控制消息协议

### 4.常用搭配：
1. SocktType.Dgram + ProtocolTypr.Udp = UDP通讯协议（常用）
   </br>UDP数据报套接字
   </br>`Socket socketUdp = new Socket(AddressFamily.InterNetwork, SocketType.Dgram, ProtocolType.Udp);`
2. SocktType.Stream + ProtocolTypr.Tcp = TCP通讯协议（常用）
   </br>TCP流套接字
   </br>`Socket socketTcp = new Socket(AddressFamily.InterNetwork, SocketType.stream, ProtocolType.Tcp);`
3. SocktType.Raw + ProtocolTypr.Icmp = Internet控制报文协议（了解）
4. SocktType.Raw + ProtocolTypr.Raw = 简单的IP通讯报文（了解）

## 四、Socket的常用属性

|常用属性| 使用示例                     |
|---|--------------------------|
|Socket的链接状态| socketTcp.Conneted       |
|Socket的寻址方案| socketTcp.AddressFamily  |
|Socket的类型| socketTcp.SocketType     |
|Socket的协议类型| socket.ProtocolType      |
|准备读取的数据数据量| socketTcp.Available      |
|获取本机EndPoint| socketTcp.LocalEndPoint  |
|获取远程的EndPoint| socketTcp.RemoteEndPoint |
 
## 五、了解Socket的常用方法

这里以同步方法为例子，这些方法都有同样作用的异步方法

### 1.主要用于服务端

#### 1.）绑定ip和端口

```csharp
IPEndPoint ipPoint = new ipPoint(IPAddress.Parse("127.0.0.1"),8080);
socketTcp.Bind(ipPoint);
```
#### 2.）设置客户端链接的最大数量

```csharp
socketTcp.Listen(10);
```

#### 3.）等待客户端的链接

```csharp
//注意：这是阻塞式的方法，会阻断当前线程的运行
socketTcp.Accept();
```

### 2.主要用于客户端

#### 1.）链接远程服务端

```csharp
//这个方法有很多重载
socketTcp.Connet(ipPoint);
```
### 3.客户端和服务端都会用到

#### 1.）同步发送和接收消息
```csharp
socketTcp.Send();
socketTcp.SendTo();
socketTcp.Receive();
```
#### 2.）异步发送和接收消息
#### 3.）释放连接并关闭Socket，先于Close调用
```csharp
socketTcp.Shutdown();
```
#### 4.）关闭连接，释放所有Socket关联资源
```csharp
socketTcp.Close();
```



















