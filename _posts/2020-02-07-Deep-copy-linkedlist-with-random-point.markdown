---
layout:     post
title:      "Deep copy of a linkedlist with random pointer"
subtitle:   ""
date:       2020-02-07 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - Tech
    - innovation
    
---


## 深拷贝带随机指针的链表



三遍遍历。 O(n) time, O(n) space

分成3步：

1. 复制节点，如A-B-C => A-A’-B-B’-C-C’
2. 依次遍历节点A,B,C，将A’B’C’这些节点的随机指针与其一致 (A'.rand -> A.rand.next)
3. 分离成 A-B-C 和 A’-B’-C’，A’-B’-C’便是所求链表


---

### END

