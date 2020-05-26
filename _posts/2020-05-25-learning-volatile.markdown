---
layout:     post
title:      "learning volatile"
subtitle:   ""
date:       2020-05-20 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - Tech
    - Interview
    
---


## learning volatile

Java Object

markword 8 btes - lock info, hashcode, thread lockrecord ref. 
class reference 4 bytes
instances - 
loss to next object alignment(make times of 8 bytes - 65 bit JVM)

Example of ClassLayout

```java
import org.openjdk.jol.info.ClassLayout;

Object o = new Object();
System.out.println(ClassLayout.parseInstance(o).toPrintable());
```

```
java.lang.Object object internals:
 OFFSET  SIZE   TYPE DESCRIPTION                               VALUE
      0     4        (object header)                           01 00 00 00 (00000001 00000000 00000000 00000000) (1)
      4     4        (object header)                           00 00 00 00 (00000000 00000000 00000000 00000000) (0)
      8     4        (object header)                           e5 01 00 f8 (11100101 00000001 00000000 11111000) (-134217243)
     12     4        (loss due to the next object alignment)
Instance size: 16 bytes
Space losses: 0 bytes internal + 4 bytes external = 4 bytes total
```

HashSet 的底层实现 基于 HashMap

TreeSet is built on top of NavigableMap (SortedMap)


Concurrenthashmap - Thread-safe 1. segment Entry 2. CAS JDK1.7+    

HashTable - Thread safe - built with Map + Synchronized

synchronized vs. lock
Jvm, api leve. 

公平锁 (put into queue)和非公平锁 

Java 读写锁 -  可以同时读，又一个获得写锁则block其他线程

事务的四大特性（ACID）
原子性（atomicity）：一个事务必须视为一个不可分割的最小单元，整个事务中的所有操作要么全部提交成功，要么全部失败回滚，对于一个事务来说，不可能只执行成功其中的一部分操作，这就是事务的原子性。

一致性（consistency）：数据总是从一个一致性的状态转换到另一个一致性的状态。

隔离性（isolation）：一个事务所做的修改在最终提交以前，对其他事务是不可见的，多个并发事务之间是相互隔离的。关于事务的隔离性，MySQL提供了四种隔离级别。

持久性（durability）：一旦事务提交，所做的修改会永久保存到数据库中。即使系统崩溃，修改的数据也不会丢失。


关于事务的隔离性，MySQL提供了四种隔离级别：

Serializable（串行化）：可避免脏读、不可重复读、幻读的发生。（级别最高）
Repeatable-read（可重复读）：可避免脏读、不可重复读的发生。
Read-committed（读已提交）：可避免脏读的发生。
Read-uncommitted（读未提交）：最低级别，任何情况都无法保证。（级别最低）

```sql

select @@tx_isolation;

set transaction isolation level 隔离级别名称;
或
set tx_isolation=’隔离级别名称’;
```




### END

