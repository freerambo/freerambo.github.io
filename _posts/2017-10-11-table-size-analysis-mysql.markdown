---
layout:     post
title:      "Table size analysis in mysql Database"
subtitle:   "Table size analysis on Mysql"
date:       2017-10-11 11:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - 技术
---

> 求学至北方，混迹于南洋. 欢迎来到自由兰博的世界


##  show table size on mysql

1. run this query to get the sizes of all tables in a mysql database:


>  ``` sql
    show table status from `DBname`
    ```
    
2. You can use this query to show the size of a table (although you need to substitute the variables first):
   
  
>  ``` sql
    SELECT 
        table_name AS `Table`, 
        round(((data_length + index_length) / 1024 / 1024), 2) `Size in MB` 
    FROM information_schema.TABLES 
    WHERE table_schema = "$DB_NAME"
        AND table_name = "$TABLE_NAME";
    ```
      
## Analysis

From method 2, I got a table size with 17.55 MB. 
Through select count I obtain the row numbers *164164*. Hence, the average size of each row is 112 Byte. 

In case of we have 1000 devices collecting data per second, each device has 20 parameters we need to store. So we are going to handle `20 * 1000 * 24 * 60 * 60`  
1.728 Billion rows. If multiply 112 Byte then total data size is 1.65 GB data per day. 



> 2017年10月11日12:27:52 跟IT大佬曹政老师交流后，更新 

朱渊博，特普通一人，渊为深，博为广，一直努力做一个有深度有广度的人。多混迹于Github, Stackoverflow, ITeye, 知乎, Linkedin 等地带。蹉跎中练就了一身码农好本领。 酷爱编程，但绝不宅。爱生活，爱思考，性情温和但充满激情，做事有主见，信奉读万卷书行万里路。
<http://www.freerambo.com/about>

---

### END

