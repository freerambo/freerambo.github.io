---
layout:     post
title:      "ThreadLocal in Java"
subtitle:   "JAVA 线程本地变量"
date:       2018-09-24 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - JAVA
    - Tech
    
---


> ThreadLocal 

　　按照传统的开发经验，如果某个对象是非线程安全的，在多线程环境下，对对象的访问必须采用synchronized进行线程同步。
参见Spring，Hibernate 等框架，其中很多模板类并未采用线程同步机制，因为线程同步会降低并发性，影响系统性能。此外，通过代码同步解决线程安全的挑战性很大，可能会增强好几倍的实现难度。
那么模板类究竟仰仗何种魔法神功，可以在无须线程同步的情况下就化解线程安全的难题呢？答案就是ThreadLocal！


　ThreadLocal，顾名思义，它不是一个线程，而是线程的一个本地化对象。
当工作于多线程中的对象使用ThreadLocal维护变量时，ThreadLocal为每个使用该变量的线程分配一个独立的变量副本。
所以每一个线程都可以独立地改变自己的副本，而不会影响其他线程所对应的副本。
从线程的角度看，这个变量就像是线程的本地变量，这也是类名中“Local”所要表达的意思。

　1、ThreadLocal不是线程，是线程的一个变量，你可以先简单理解为线程类的属性变量。

　2、ThreadLocal在类中通常定义为静态变量。

　3、每个线程有自己的一个ThreadLocal，它是变量的一个“拷贝”，修改它不影响其他线程。

　　既然定义为类变量，为何为每个线程维护一个副本（姑且称为“拷贝”容易理解），让每个线程独立访问？多线程编程的经验告诉我们，对于线程共享资源（你可以理解为属性），资源是否被所有线程共享，也就是说这个资源被一个线程修改是否影响另一个线程的运行，如果影响我们需要使用synchronized同步，让线程顺序访问。

　　**ThreadLocal适用于资源共享但不需要维护状态的情况，也就是一个线程对资源的修改，不影响另一个线程的运行；这种设计是‘空间换时间’，synchronized顺序执行是‘时间换取空间’。**


　从中我们可以发现这个Map的key是ThreadLocal变量，value为用户的值，并不是很多人认为的key是线程的名字或者标识。到这里，我们就可以理解ThreadLocal究竟是如何工作的了。

　　1. Thread类中有一个成员变量叫做ThreadLocalMap，它是一个Map，他的Key是ThreadLocal类

　　2. 每个线程拥有自己的申明为ThreadLocal类型的变量，所以这个类的名字叫ThreadLocal：线程自己的（变量）。

　　3. 此变量生命周期是由该线程决定的，开始于第一次初始化（get或者set方法）。

　　4. 由ThreadLocal的工作原理决定了：每个线程独自拥有一个变量，并非共享或者拷贝。


## Volatile & Atomic

Volatile for visibility

所有线程线程 
Atomic for atomicity 











---

### END

