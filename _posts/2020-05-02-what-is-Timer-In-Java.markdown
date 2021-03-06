---
layout:     post
title:      "What is Timer in Java"
subtitle:   ""
date:       2020-05-02 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - Tech
    - Interview
    
---


## Java Sleep 实现原理


两个问题
假设现在是 20018-12-03 12:00:00.000，如果我调用一下 Thread.Sleep(1000) ，在 20018-12-03 12:00:01.000 的时候，这个线程会不会被唤醒？
某人的代码中用了一句看似莫明其妙的话：Thread.Sleep(0) 。既然是 Sleep 0 毫秒，那么他跟去掉这句代码相比，有啥区别么？
对于第一个问题，答案是：不一定。因为你只是告诉操作系统：在未来的1000毫秒内我不想再参与到CPU竞争。那么1000毫秒过去之后，这时候也许另外一个线程正在使用CPU，那么这时候操作系统是不会重新分配CPU的，直到那个线程挂起或结束；况且，即使这个时候恰巧轮到操作系统进行CPU 分配，那么当前线程也不一定就是总优先级最高的那个，CPU还是可能被其他线程抢占去。
Thread.Sleep(0)的作用，就是“让出cpu，会触发操作系统立刻重新进行一次CPU竞争”。竞争的结果也许是当前线程仍然获得CPU控制权，也许会换成别的线程获得CPU控制权。

sleep的底层实现
sleep()：进程、线程或任务(Linux中不区分进程与线程，都称为task)可以sleep，这会导致它们暂停执行一段时间，直到等待的时间结束才恢复执行或在这段时间内被中断。
sleep()在OS中的实现的大概流程：

挂起进程（或线程）并修改其运行状态
用sleep()提供的参数来设置一个定时器。
当时间结束，定时器会触发，内核收到中断后修改进程（或线程）的运行状态。例如线程会被标志为就绪而进入就绪队列等待调度。
可变定时器(variable timer)一般在硬件层面是通过一个固定的时钟和计数器来实现的，每经过一个时钟周期将计数器递减，当计数器的值为0时产生中断。内核注册一个定时器后可以在一段时间后收到中断。

综上所述，内核的sleep()函数是在挂起原语的基础上利用定时器实现的。


Ref. https://www.jianshu.com/p/74becd7ffcf6

### END

