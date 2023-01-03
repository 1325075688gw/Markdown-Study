## 文章目录

[TOC]

### 一、知识点来源

#### 1.作者信息

| 作者                                              | 视频链接                                                     | 备注 |
| ------------------------------------------------- | ------------------------------------------------------------ | ---- |
| [遇见狂神说](https://space.bilibili.com/95256449) | [gRPC](https://www.bilibili.com/video/BV1S24y1U7Xp?p=1&vd_source=58acbf449edd771737ee43a78ffdabf4) |      |

#### 2.本文主要内容

> 本文主要介绍gPRC通信技术以及交互协议Proto，并辅以go和Python gRPC实战项目加深了解

### 二、gRPC介绍

#### 1.产生背景

##### 1.单体架构

> 单体架构的弊端

1. 一旦某个服务宕机，会引起整个应用不可用，隔离性差
2. 只能整体应用进行伸缩，浪费资源，可伸缩性差（比如我们支付模块需要3个服务器、下单模块需要2个服务器，但是在单体架构下，不能完成）
3. 代码耦合度高，可维护性差

##### 2.微服务架构

> 微服务架构的弊端

1. 代码冗余（底层的代码需要再不同服务写几份）
2. 服务于服务之间需要通过HTTP或者其他方式进行调用（有网络延时）

##### 3.微服务架构下的解决方案

- 单体服务拆分后，服务与服务之间发生的是进程和进程之间的调用（服务器和服务器之间的调用）
- 那么就需要发起网络调用，常用的网络调用方式有HTTP，但是在微服务架构中，HTTP虽然实现起来方便便捷，但是性能较低，这时候我们可以使用RPC（远程过程调用），通过自定义协议发起TCP调用，加快传输效率

#### 2.gRPC参考资料

##### 1.中文文档

- http://doc.oschina.net/grpc?t=56831

##### 2.kuangshen-bilibili

- https://www.bilibili.com/video/BV1S24y1U7Xp?p=1&vd_source=58acbf449edd771737ee43a78ffdabf4

#### 3.gRPC实现

##### 1.Python实现

- https://blog.csdn.net/bocai_xiaodaidai/article/details/103958468

##### 2.Go实现

- https://blog.csdn.net/weixin_43143310/article/details/125064588


#### 3.RPC介绍

- RPC的全程是==Remote Procedure Call==（远程过程调用），这是一种协议，是用来屏蔽分布式计算机中的各种调用细节，使得你可以像调用本地方法一样调用远程的函数

- ![3f704c34389b8aa4f6d8e9dfc8e8587e.png](https://img-blog.csdnimg.cn/img_convert/3f704c34389b8aa4f6d8e9dfc8e8587e.png)

  ```sh
  Stub处理客户端和服务端约定好的语法、语义的封装和解封装
  RPCRuntime负责最底层的网络传输。
  ```

- ![image-20230101164733971](/Users/gongwei/Library/Application Support/typora-user-images/image-20230101164733971.png)

##### 1.RPC调用过程

```python
调用者（客户端Client）以本地调用的方式发起调用；
Client stub（客户端存根）收到调用后，负责将被调用的方法名、参数等打包编码成特定格式的能进行网络传输的消息体；
Client stub将消息体通过网络发送给服务端；（调用RPCRRuntime）
Server stub（服务端存根）收到通过网络接收到消息后按照相应格式进行拆包解码，获取方法名和参数；
Server stub根据方法名和参数进行本地调用；

被调用者（Server）本地调用执行后将结果返回给server stub；
（调用RPCRRuntime）
Server stub将返回值打包编码成消息，并通过网络发送给客户端；
Client stub收到消息后，进行拆包解码，返回给Client；
Client得到本次RPC调用的最终结果。

```

##### 2.RPC解决的问题

- 基于RPC模式，一个 RPC 框架基本需要解决 协议约定、网络传输、服务发现这三个问题。
  1. 协议约定问题（Stub） 指的是怎么规定远程调用的语法，怎么传参数等。用上面的类比，你怎么告诉你的朋友要玩什么游戏？是直接说游戏的名字，王者荣耀，绝地求生，还是说简称，王者，吃鸡，或者用 1 代表王者，2 代表吃鸡，只说 1 或 2。
  2. 传输协议问题（RPCRuntime） 指的是在网络发生错误、重传、丢包或者有性能问题时怎么办？用上面的类比，你打电话时，刚说了打什么游戏，但是还没有收到对方回复，电话信号不好断了，这时候怎么处理？
  3. 服务发现问题（插件比如:etcd） 指的是如何知道服务端有哪些服务可以调用，从哪个端口访问？服务端可能实现多个远程调用，在不同的进程上，随机监听端口，客户端要怎么才能知道这些端口呢？

#### 4.gRPC介绍

- `gRPC`是由 `google`开发的一个高性能、通用的开源`RPC`框架，主要面向移动应用开发且基于`HTTP/2`协议标准而设计，充分利用 HTTP/2 stream 的特性，从而有助于节省带宽、降低 TCP 的连接次数、节省CPU的使用等。同时支持大多数流行的编程语言，比如（Golang、Python、Java等）。

- gRPC如何解决RPC的三个问题

  1. 协议约定。gRPC 的协议是 Protocol Buffers，是一种压缩率极高的序列化协议，Google 在 2008 年开源了 Protocol Buffers，支持多种编程语言，所以 gRPC 支持客户端与服务端可以用不同语言实现。
  2. 传输协议。gRPC 的数据传输用的是 Netty Channel， Netty 是一个高效的基于异步 IO 的网络传输架构。Netty Channel 中，每个 gRPC 请求封装成 HTTP 2.0 的 Stream。
  3. 服务发现。gRPC 本身没有提供服务发现的机制，需要通过其他组件。

  > grpc是一种实现了rpc协议的框架，并且分别通过protocol buffer、netty channel 以及服务发现组件解决rpc的协议约定、传输协议、服务发现三大问题。

#### 5.RPC和gRPC的区别

- RPC是一种远程过程调用协议
- grpc是一种实现了rpc协议的框架，并且分别通过protocol buffer、netty channel 以及服务发现组件解决rpc的协议约定、传输协议、服务发现三大问题。



### Protobuf学习

#### 1.Protobuf介绍

##### 1.Protobuf介绍

- Protobuf是谷歌开源的一种数据格式，适合高性能、对响应速度又要求的数据传输场景，因为Protobuf是二进制数据格式，需要编码和解码，数据本身不具有可读性，因此只能通过反序列化后才能得到正真的数据

##### 2.Protobuf优缺点

- 优点
  1. 序列化后体积比json和XML小很多，适合网络传输
  2. 支持生成多种语言的Protobuf文件
  3. 序列化和反序列化速度很快
- 缺点
  1. 二进制格式导致可读性差
  2. 缺乏自描述

#### 2.Protobuf规则学习

##### 1.字段规则

```protobuf
required
optional（默认）
repeated
```

##### 2.消息号

```
每个字段都有一个消息号，不同字段的消息号不能重复（推荐从1开始计数）
```

##### 3.嵌套消息

```protobuf
message PersonInfo {
	message Person {
		string name = 1;
		int64 age = 2;
	}
	repeated Person person = 1;
}
```



### gRPC实现

#### 1.Python实现gRPC

##### 1.安装依赖

```python
pip install grpcio #gRPC 的安装 
pip install protobuf  #ProtoBuf 相关的 python 依赖库
pip install grpcio-tools   #python grpc 的 protobuf 编译工具
```

##### 2.在IDEA中安装proto插件

这样语法可以高亮

##### 3.生产路径

```
https://zhuanlan.zhihu.com/p/305974542
```

##### 4.安装protobuf

```sh
可以不安装

brew install protobuf
```

##### 5.编写protobuf文件

```protobuf
syntax = "proto3";
package base_package;


service FormatData {   //定义服务,用在rpc传输中
  rpc say_hello(HelloRequest) returns (HelloResponse);
}
message HelloRequest {
    string request_name = 1;
}
message HelloResponse {
    string response_msg = 1;
}
```

##### 6.生成protobuf文件和gRPC文件

```sh
本来安装brew install protobuf后，可以直接这么运行
protoc -I . --python_out ./ --grpc_python_out ./ ./hello.proto 
(但是因为brew install protobuf安装失败，所以找不到protoc命令)

所以这么运行：d
python -m grpc_tools.protoc -I . --python_out ./ --grpc_python_out ./ ./hello.proto

-I等价于--proto_path:指定proto文件位置，但是最后还是要制定proto文件位置（./hello.proto）
```

#### 2.Go实现gRPC

##### 1.安装依赖

```
protoc -I . --go_out ./ --grpc_go_out ./ ./hello.proto 
```

https://blog.csdn.net/qq_42062052/article/details/120650979

https://www.cnblogs.com/liuzhanghao/p/16512232.html



go get -u github.com/golang/protobuf/protoc-gen-go废弃：

go: module github.com/golang/protobuf is deprecated: Use the "google.golang.org/protobuf" module instead.

