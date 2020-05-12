---
layout:     post
title:      "What is digital certificate"
subtitle:   ""
date:       2020-04-21 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - Tech
    - Interview
    
---


## 分布式一致性与共识算法

区块链技术是近几年逐渐变得非常热门的技术，以比特币为首的密码货币其实已经被无数人所知晓，但是却很少有人会去研究它们的底层技术，也就是作为一个分布式网络比特币等加密货币是如何工作的。

无论是 Bitcoin、Ethereum 还是 EOS，作为一个分布式网络，首先需要解决分布式一致性的问题，也就是所有的节点如何对同一个提案或者值达成共识，这一问题在一个所有节点都是可以被信任的分布式集群中都是一个比较难以解决的问题，更不用说在复杂的区块链网络中了。

### 分布式一致性

在一个分布式系统中，如何保证集群中所有节点中的数据完全相同并且能够对某个提案（Proposal）达成一致是分布式系统正常工作的核心问题，而共识算法就是用来保证分布式系统一致性的方法。

然而分布式系统由于引入了多个节点，所以系统中会出现各种非常复杂的情况；随着节点数量的增加，节点失效、故障或者宕机就变成了一件非常常见的事情，解决分布式系统中的各种边界条件和意外情况也增加了解决分布式一致性问题的难度。

在一个分布式系统中，除了节点的失效是会导致一致性不容易达成的主要原因之外，节点之间的网络通信收到干扰甚至阻断以及分布式系统的运行速度的差异都是解决分布式系统一致性所面临的难题。

### CAP


在 1998 年的秋天，加州伯克利大学的教授 Eric Brewer 第一次发布了 CAP 理论，在 1999 年论文 
[Brewer’s Conjecture and the Feasibility of Consistent, Available, Partition-Tolerant Web Services](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.67.6951&rep=rep1&type=pdf)
正式发布，其中总结了 Eric Brewer 提出的 CAP 理论。

Consistency: This means the database mostly remains adherent to its rules (constraints, triggers,
and so on) after the execution of each operation and that any future transaction will see the effects
of the earlier transactions committed. For example, after executing an update, all the clients see the
same data.


Availability: Each operation is guaranteed a response—a successful or failed execution. This, in
practice, means no downtime.

Partition tolerance: This means the system continues to function even if the communication among
the servers is temporarily unreliable

这篇论文证明了两个非常有意思的理论，首先是在异步的网络模型中，所有的节点由于没有时钟仅仅能根据接收到的消息作出判断，这时完全不能同时保证一致性、可用性和分区容错性，每一个系统只能在这三种特性中选择两种。

不过这里讨论的一致性其实都是强一致性，也就是所有节点接收到同样的操作时会按照完全相同的顺序执行，被一个节点提交的更新操作会立刻反映在其他通过异步或部分同步网络连接的节点上，如果想要同时满足一致性和分区容错性，在异步的网络中，我们只能中心化存储所有数据，通过其他节点将请求路由给中心节点达到这两个目的。


[https://draveness.me/consensus/](https://draveness.me/consensus/)

### END

