---
layout:     post
title:      "Preflight Request"
subtitle:   ""
date:       2018-11-27 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - 技术
    - Conference
    
---



　　


## Background

often we saw two requests when call a backend API. 


其实第一次发送的就是preflight request(预检请求)，那么这篇文章将讲一下，为什么要发预检请求，什么时候会发预检请求，预检请求都做了什么

##  为什么要发预检请求

CORS需要浏览器和服务器同时支持。目前，所有浏览器都支持该功能，IE浏览器不能低于IE10。

整个CORS通信过程，都是浏览器自动完成，不需要用户参与。对于开发者来说，CORS通信与同源的AJAX通信没有差别，代码完全一样。浏览器一旦发现AJAX请求跨源，就会自动添加一些附加的头信息，有时还会多出一次附加的请求，但用户不会有感觉。

因此，实现CORS通信的关键是服务器。只要服务器实现了CORS接口，就可以跨源通信。


我们都知道浏览器的同源策略，就是出于安全考虑，浏览器会限制从脚本发起的跨域HTTP请求，像XMLHttpRequest和Fetch都遵循同源策略。
浏览器限制跨域请求一般有两种方式：

浏览器限制发起跨域请求
跨域请求可以正常发起，但是返回的结果被浏览器拦截了
一般浏览器都是第二种方式限制跨域请求，那就是说请求已到达服务器，并有可能对数据库里的数据进行了操作，但是返回的结果被浏览器拦截了，那么我们就获取不到返回结果，这是一次失败的请求，但是可能对数据库里的数据产生了影响。

为了防止这种情况的发生，规范要求，对这种可能对服务器数据产生副作用的HTTP请求方法，浏览器必须先使用OPTIONS方法发起一个预检请求，从而获知服务器是否允许该跨域请求：如果允许，就发送带数据的真实请求；如果不允许，则阻止发送带数据的真实请求。

## 什么时候发预检请求
HTTP请求包括： 简单请求 和 需预检的请求

1. 简单请求
简单请求不会触发CORS预检请求，“简属于
单请求”术语并不属于Fetch(其中定义了CORS)规范。
若满足所有下述条件，则该请求可视为“简单请求”：

使用下列方法之一：
GET
HEAD
POST
Content-Type: (仅当POST方法的Content-Type值等于下列之一才算做简单需求)
text/plain
multipart/form-data
application/x-www-form-urlencoded


2.需预检的请求
“需预检的请求”要求必须首先使用OPTIONS方法发起一个预检请求到服务区，以获知服务器是否允许该实际请求。“预检请求”的使用，可以避免跨域请求对服务器的用户数据产生未预期的影响。

当请求满足下述任一条件时，即应首先发送预检请求：

使用了下面任一 HTTP 方法：
PUT
DELETE
CONNECT
OPTIONS
TRACE
PATCH
人为设置了对 CORS 安全的首部字段集合之外的其他首部字段。该集合为：
Accept
Accept-Language
Content-Language
Content-Type
DPR
Downlink
Save-Data
Viewport-Width
Width
Content-Type的值不属于下列之一:
application/x-www-form-urlencoded
multipart/form-data
text/plain
如下是一个需要执行预检请求的HTTP请求：


https://www.jianshu.com/p/b55086cbd9af

http://www.ruanyifeng.com/blog/2016/04/cors.html



---

### END

