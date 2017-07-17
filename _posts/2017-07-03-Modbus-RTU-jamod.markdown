---
layout:     post
title:      "Read data through Modbus in JAVA (Part 2. RTU)"
subtitle:   "Use jamod to operate slaves of Modbus RTU"
date:       2017-07-03 12:00:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true
tags:
    - Modbus
    - JAVA
---

> This post introduces how to use java to communicate with slaves via Modbus RTU。
 
继上篇文章讲述Modbus TCP如何读取数据之后，本文重点研究如何使用Modbus RTU和设备通讯。首先我们来看一下Modbus协议的介绍和特点。来自百度百科。

## Modbus协议介绍
- Modbus是由Modicon（现为施耐德电气公司的一个品牌）在1979年发明的，是全球第一个真正用于工业现场的总线协议。

- ModBus网络是一个工业通信系统，由带智能终端的可编程序控制器和计算机通过公用线路或局部专用线路连接而成。其系统结构既包括硬件、亦包括软件。它可应用于各种数据采集和过程监控。

- ModBus网络只有一个主机，所有通信都由他发出。网络可支持247个之多的远程从属控制器，但实际所支持的从机数要由所用通信设备决定。采用这个系统，各PC可以和中心主机交换信息而不影响各PC执行本身的控制任务。

- 为更好地普及和推动Modbus在基于以太网上的分布式应用，目前施耐德公司已将Modbus协议的所有权移交给IDA（Interface for Distributed Automation，分布式自动化接口）组织，并成立了Modbus-IDA组织，为Modbus今后的发展奠定了基础。在中国，Modbus已经成为国家标准GB/T19582-2008。据不完全统计：截止到2007年，Modbus的节点安装数量已经超过了1000万个。

- Modbus协议是应用于电子控制器上的一种通用语言。通过此协议，控制器相互之间、控制器经由网络（例如以太网）和其它设备之间可以通信。它已经成为一通用工业标准。有了它，不同厂商生产的控制设备可以连成工业网络，进行集中监控。此协议定义了一个控制器能认识使用的消息结构，而不管它们是经过何种网络进行通信的。它描述了一控制器请求访问其它设备的过程，如何回应来自其它设备的请求，以及怎样侦测错误并记录。它制定了消息域格局和内容的公共格式。

- 当在一Modbus网络上通信时，此协议决定了每个控制器须要知道它们的设备地址，识别按地址发来的消息，决定要产生何种行动。如果需要回应，控制器将生成反馈信息并用Modbus协议发出。在其它网络上，包含了Modbus协议的消息转换为在此网络上使用的帧或包结构。这种转换也扩展了根据具体的网络解决节地址、路由路径及错误检测的方法。

- 此协议支持传统的RS-232、RS-422、RS-485和以太网设备。许多工业设备，包括PLC，DCS，智能仪表等都在使用Modbus协议作为他们之间的通讯标准。
 
## Modbus协议特点

-标准、开放，用户可以免费、放心地使用Modbus协议，不需要交纳许可证费，也不会侵犯知识产权。目前，支持Modbus的厂家超过400家，支持Modbus的产品超过600种。

-Modbus可以支持多种电气接口，如RS-232、RS-485等，还可以在各种介质上传送，如双绞线、光纤、无线等。

-Modbus的帧格式简单、紧凑，通俗易懂。用户使用容易，厂商开发简单。

## Modbus 协议介绍 TCP Vs. RTU Vs. PLUS

> 以太网：MODBUS TCP 异步串行传输：MODBUS RTU或者MODBUS ASCII 高速令牌传递网络：MODBUS PLUS

- 标准的Modicon控制器使用RS232C实现串行的Modbus。Modbus的ASCII、RTU协议规定了消息、数据的结构、命令和就答的方式，数据通讯采用Maser/Slave方式。

- Modbus协议需要对数据进行校验，串行协议中除有奇偶校验外，ASCII模式采用LRC校验，RTU模式采用16位CRC校验.

- ModbusTCP模式没有额外规定校验，因为TCP协议是一个面向连接的可靠协议。

- MODBUS TCP和MODBUS RTU的差别不是很大。二者相同的地方是应用数据单元是一致的。差别是MODBUS TCP是传输在TCP/IP网络上的，多了一个报文头，少了CRC校验，采用TCP的502端口，RTU多了设备地址和CRC校验

- Modbus Plus则是一种典型的令牌环网，完整定义了通讯协议、网络结构、连接电缆（或者光缆）以及安装工具等方面的性能指标。Modbus+网络中的设备通过‘令牌’的方式实现数据的交换， 严格定义了令牌的传递方式，数据校验以及通讯短口等方面的技术参数。MODBUS PLUS比MODBUS的性能更好，通讯速率快，从协议开发上来说区别较大，modbus比较简单。

## 测试环境
虽然我们要求与实际设备联动，但在开发阶段我想模拟出多个Modbus测试环境，网上搜索后发现modbustools提供的Master和Slave工具非常不错，
 [`modbustools`](http://www.modbustools.com/)，可以根据自己的需求下载相关的模拟器，这里我首先要模拟一个modbus设备，所以下载了Slave端程序。我的开发环境是windows，现在安装后根据需求配置slave即可。
 无奈没有注册码只能试用三十天。 绿色版下载[地址](http://www.hifreud.com/2016/06/02/modbus-02-simulation-environment-setup/) 


### 模拟基于RTU的MODBUS协议

- a) 创建虚拟端口
- b) 打开vspd，如下图所以，例如可以在右侧ManagePorts中选中First Port为COM10，Second Port为COM11，点击Add pair

![测试环境](http://www.hifreud.com/images/blog/modbus/modbus-02-simulation-environment-setup/06.png)

### modbustools slave 选择协议RTU 

- d) 模拟Server搭建
- e) 打开modbus_slave，依次选中Toolbar中的Connection，点击Connect，在Port中选择Port 10，按照如下配置
![协议选择](http://www.hifreud.com/images/blog/modbus/modbus-02-simulation-environment-setup/07.png)


## Modbus RTU 所使用的classes  

jamod中提供了对应的类来实现Modbus RTU Master操作.
- Connection: SerialConnection
- Transaction: ModbusSerialTransaction
- Request: ModbusRequest (respectively it's direct known subclass ReadInputRegistersRequest)
- Response: ModbusResponse (respectively it's direct known subclass ReadInputRegistersResponse)
- Parameters: SerialParameters



## 实现流程


![请求流程](http://jamod.sourceforge.net/images/serialmaster_sequential.png)

首先我们构建一个SerialConnection, 接着配置请求参数，发送请求并由一个ModbusSerialtransaction来handle (交由ModbusSerialTransport来处理)。得到返回后并处理。最后关闭连接。


## 代码分析

一读取holding register类型为例分析。

###  设置serial connection 所需参数 端口，波特率，编码 校验 停止位等
```java

//1. Setup serial parameters

    SerialParameters params = new SerialParameters();
    params.setPortName(portname); //the name of the serial port to be used
    params.setBaudRate(9600); 
    params.setDatabits(8);
    params.setStopbits(1);
    params.setParity("even");
    params.setEncoding("rtu");
            	
```


###  设置request parameters 请求参数

```java

//2. Setup the request parameters         

    	int unitid = 2; //the unit identifier we will be talking to
    	int ref = 12; //the reference, where to start reading from
    	int count = 2; //the count of IR's to read
```


### 建立连接，发送请求

```java
    //3. Open the connection
    con = new SerialConnection(params);
    con.open();
    
    //4. Prepare a request
    req = new ReadInputRegistersRequest(ref, count);
    req.setUnitID(unitid);
    req.setHeadless();
    
    //5. Prepare a transaction
    trans = new ModbusSerialTransaction(con);
    trans.setRequest(req);
```


### 处理返回数据，关闭连接

```java
	//6. Execute the transaction repeat times
    	
     trans.execute();
     res = (ReadInputRegistersResponse) trans.getResponse();

    //7. Close the connection
    con.close();  

```


## 程序测试 

根据所设置的端口和参数，结课测试Modbus RTU通信。 


## 总结
本文成功在模拟环境下测试了Modbus RTU的通信。 所有源代码都在github上， 欢迎关注，欢迎讨论。
Github上对Modbus RTU的操作做了封装。抽象了一个ModbusSerialUtil类可以直接调用。
源码地址 <https://github.com/freerambo/jamod.git>
