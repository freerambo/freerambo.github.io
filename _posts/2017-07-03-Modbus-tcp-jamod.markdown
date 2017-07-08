---
layout:     post
title:      "Read data through Modbus in JAVA (Part 1. TCP)"
subtitle:   "Use jamod to operate slaves of Modbus TCP"
date:       2017-07-02 12:00:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true
tags:
    - Modbus
    - JAVA
---

> This post reports how to use java to communicate with slaves via Modbus TCP。
 


## 项目需求

最近做一个项目需要和各种主流工业协议通讯，而且要求可以添加特定协议的各类设备。添加完设备后还需要我们和该设备针剂通过协议读取数据和控制。其中Modbus是最主要的协议之一。

该项目对灵活性和通用型要求非常高，对我们的设计自然是复杂度也比较难，后续我会讲述如何设计的。这里我主要讲述如何通过`jamod`([Java Modbus Library](http://jamod.sourceforge.net/)) 库在JAVA环境下与Modbus TCP 和RTU通信的。[源码下载地址](https://github.com/freerambo/jamod.git)


## 测试环境
虽然我们要求与实际设备联动，但在开发阶段我想模拟出多个Modbus测试环境，网上搜索后发现modbustools提供的Master和Slave工具非常不错，
 [`modbustools`](http://www.modbustools.com/)，可以根据自己的需求下载相关的模拟器，这里我首先要模拟一个modbus设备，所以下载了Slave端程序。我的开发环境是windows，现在安装后根据需求配置slave即可。
 无奈没有注册码只能试用三十天。 绿色版下载[地址](http://www.hifreud.com/2016/06/02/modbus-02-simulation-environment-setup/) 

### modbustools slave 协议选择 

![协议选择](/img/post/modbustcp-set.png)

### modbustools slave 客户端配置 

![测试环境](http://images.cnitblog.com/blog/578798/201502/231633065184693.jpg)

## Modbus 分析 

Supported Modbus Functions:

- 01: Read coil status
- 02: Read input status
- 03: Read holding registers
- 04: Read input registers
- 05: Force single coil
- 06: Preset single register
- 15: Force multiple coils
- 16: Preset multiple registers


以读数据为例个人的理解就是:

- 01 可以读写的布尔类型
- 02 只能读的布尔类型
- 03 只能读的数字类型
- 04 可以读写的数字类型

jamod也提供了操作每种不同类型使用不同的类，这里给出个表格。

| 功能 | 请求类 | 响应类 |
|01 只能读的布尔类型 |ReadCoilsRequest|ReadCoilsResponse|
|02 可以读写的布尔类型 |ReadInputDiscretesRequest|ReadInputDiscretesResponse|
|03 只能读的数字类型 |ReadMultipleRegistersRequest|ReadMultipleRegistersResponse|
|04 可以读写的布尔类型|ReadInputRegistersRequest|ReadInputRegistersResponse|

## 代码分析

> Talk is cheap, 直接整代码

### F01 读取Coill类型数据

这里需要的参数分别是 `Ip地址` `端口号`，`参数地址`，`设备地址`. 在这里我们只读取一位数据，所以默认数据长度设置为1，在实际中也可以给多位，则要修改返回数据列表处理时。
```
ReadCoilsRequest req = new ReadCoilsRequest(ref, count);  这里count给的是1 
```

代码如下

```java

	public static int readDiscretesInput(String ip, int port, int ref, int slaveId) {
		int data = 0;

		try {
			InetAddress addr = InetAddress.getByName(ip);

			// 1 build the connection with ip and port 
			TCPMasterConnection con = ModbusConnection.openTcpConnection(ip, port);

			// 2 the 1st: the ref of register; 2nd is the number of register will be read
			ReadCoilsRequest req = new ReadCoilsRequest(ref, 1);

			// 3 set the Slave Id (deviceId)
			req.setUnitID(slaveId);

            // 4 open a transaction and set the request params
			ModbusTCPTransaction trans = new ModbusTCPTransaction(con);
    
			trans.setRequest(req);

			// 5 execute the search
			trans.execute();

			// 6 get the result
			ReadCoilsResponse res = ((ReadCoilsResponse) trans.getResponse());

			if(res.getDiscretes().getBit(0)){
				data = 1;
			}
			// 7 close the connection
			con.close();

		} catch (Exception e) {
			e.printStackTrace();
		}

		return data;
	}

```


### F02 读取DiscretesInput类型数据 

```java
public static int readDiscretesInput(String ip, int port, int ref, int slaveId) {
		int data = 0;

		try {
			InetAddress addr = InetAddress.getByName(ip);

			// build the connection
			TCPMasterConnection con = ModbusConnection.openTcpConnection(ip, port);

			

			// the 1st: the ref of register; 2nd is the number of register will be read
			ReadInputDiscretesRequest req = new ReadInputDiscretesRequest(ref, 1);

			// the Slave Id
			req.setUnitID(slaveId);

			ModbusTCPTransaction trans = new ModbusTCPTransaction(con);

			trans.setRequest(req);

			// execute the search
			trans.execute();

			// get the result
			ReadInputDiscretesResponse res = (ReadInputDiscretesResponse) trans.getResponse();

			if(res.getDiscretes().getBit(0)){
				data = 1;
			}

			// 关闭连接
			con.close();

		} catch (Exception e) {
			e.printStackTrace();
		}

		return data;
	}
```



### F03 读取HoldingRegister类型数据 

```java
public static int readRegister(String ip, int port, int ref,
			int slaveId) {
		int data = 0;
		try {
			TCPMasterConnection con = ModbusConnection.openTcpConnection(ip, port);
			con.connect();
			ReadMultipleRegistersRequest req = new ReadMultipleRegistersRequest(ref, 1);
			req.setUnitID(slaveId);

			ModbusTCPTransaction trans = new ModbusTCPTransaction(con);

			trans.setRequest(req);

			trans.execute();

			ReadMultipleRegistersResponse res = (ReadMultipleRegistersResponse) trans.getResponse();
			
			data = res.getRegisterValue(0);

			con.close();
		} catch (Exception e) {
			e.printStackTrace();
		}

		return data;
	}
```

### F04 读取InputRegister类型数据 

```java
public static int readRegister(String ip, int port, int ref,
			int slaveId) {
		int data = 0;
		try {
			TCPMasterConnection con = ModbusConnection.openTcpConnection(ip, port);
			con.connect();
			ReadMultipleRegistersRequest req = new ReadMultipleRegistersRequest(ref, 1);
			req.setUnitID(slaveId);

			ModbusTCPTransaction trans = new ModbusTCPTransaction(con);

			trans.setRequest(req);

			trans.execute();

			ReadMultipleRegistersResponse res = (ReadMultipleRegistersResponse) trans.getResponse();
			
			data = res.getRegisterValue(0);

			con.close();
		} catch (Exception e) {
			e.printStackTrace();
		}

		return data;
	}
```

### F05 写Discretes Coil 类型数据 

```java
public static void writeDigitalOutput(String ip, int port, int slaveId,
			int ref, int value) {

		try {
			TCPMasterConnection connection = ModbusConnection.openTcpConnection(ip, port);

			connection.connect();

			ModbusTCPTransaction trans = new ModbusTCPTransaction(connection);

			boolean val = true;

			if (value == 0) {
				val = false;
			}

			WriteCoilRequest req = new WriteCoilRequest(ref, val);

			req.setUnitID(slaveId);
			trans.setRequest(req);

			System.out.println("ModbusSlave: FC" + req.getFunctionCode()
			+ " ref=" + req.getReference() + " value="
			+ val);
			
			trans.execute();
			connection.close();
		} catch (Exception ex) {
			System.out.println("writeDigitalOutput Error in code");
			ex.printStackTrace();
		}
	}
```

### F06 写Input Register 类型数据 

```java
public static void writeRegister(String ip, int port, int slaveId,
			int ref, int value) {

		try {
			TCPMasterConnection connection = ModbusConnection.openTcpConnection(ip, port);
			connection.connect();

			ModbusTCPTransaction trans = new ModbusTCPTransaction(connection);

			SimpleRegister register = new SimpleRegister(value);

			
			WriteSingleRegisterRequest req = new WriteSingleRegisterRequest(
					ref, register);

			req.setUnitID(slaveId);
			trans.setRequest(req);

			System.out.println("ModbusSlave: FC" + req.getFunctionCode()
					+ " ref=" + req.getReference() + " value="
					+ register.getValue());
			trans.execute();

			connection.close();
		} catch (Exception ex) {
			System.out.println("Error in code");
			ex.printStackTrace();
		}
	}
```

## 程序测试 

### 测试环境客户端配合

![客户端配置](/img/post/modbust-config.png)

### 测试结果

配置完成模拟器，即可测试读写各类型数据。测试结果如下：

![测试结果](/img/post/modbus-test.png)

## 总结
本文成功测试了Modbus TCP的六种functions的读写。 所有源代码都在github上， 欢迎关注，欢迎讨论。
源码地址 <https://github.com/freerambo/jamod.git> 
接下来我将讲述如何用`jamod`实现`modbust RTU`的通信。
