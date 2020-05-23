---
layout:     post
title:      "What is TCC"
subtitle:   ""
date:       2020-04-22 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - Tech
    - Interview
    
---


## TCC-transaction


TCC事务 即：Try-Confirm-Cancel 。它是基于业务层面的事务定义，把事务运行过程分成 Try、Confirm / Cancel 两个阶段。在每个阶段的逻辑由业务代码控制。每一个初步操作，最终都会被确认或取消。因此，针对一个具体的业务服务，TCC事务机制需要业务系统提供三段业务逻辑：初步操作Try、确认操作Confirm、取消操作Cancel。

Try 从执行阶段来看，与传统事务机制中业务逻辑相同。但从业务角度来看，却不一样。TCC机制中的Try仅是一个初步操作，它和后续的确认一起才能真正构成一个完整的业务逻辑。TCC机制将传统事务机制中的业务逻辑一分为二，拆分后保留的部分即为初步操作（Try）；而分离出的部分即为确认操作（Confirm），被延迟到事务提交阶段执行。

完成所有业务检查( 一致性 )
预留必须业务资源( 准隔离性 )

Confirm 是对 Try 操作的一个补充。当TCC事务管理器决定commit全局事务时，就会逐个执行Try操作指定的Confirm操作，将Try未完成的事项最终完成。

Cancel 是对Try操作的一个回撤。当TCC事务管理器决定rollback全局事务时，就会逐个执行Try操作指定的Cancel操作，将Try操作已完成的事项全部撤回。


TCC优缺点
优点：让应用自己定义数据库操作的粒度，使得降低锁冲突、提高吞吐量成为可能。
不足：
对应用的侵入性强。业务逻辑的每个分支都需要实现try、confirm、cancel三个操作，应用侵入性较强，改造成本高。
实现难度较大。需要按照网络状态、系统故障等不同的失败原因实现不同的回滚策略。为了满足一致性的要求，confirm和cancel接口必须实现幂等。

Ref. http://lingo0.github.io/2018/10/15/TCC-transaction%E5%AD%A6%E4%B9%A0/
### END

