---
layout:     post
title:      "Database SQL practices"
subtitle:   "Database SQL practices On MYSQL"
date:       2017-10-23 11:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - 技术
---

> 求学至北方，混迹于南洋. 欢迎来到自由兰博的世界


##  Create a test table 

1. create table


>  ``` sql
    create table employee(id int primary key auto_increment,name varchar(50),salary bigint,deptid int);
    ```
    
2. fill in sample data 
  
>  ``` sql
        insert into employee921 values(null,'zs',1000,1),(null,'ls',1100,1),(null,'ww',1100,1),(null,'zl',900,1) ,(null,'zl',1000,2), (null,'zl',900,2) ,(null,'zl',1000,2) , (null,'zl',1100,2);
    ```
      
## Practices


1. 列出各个部门中工资高于本部门的平均工资的员工数和部门号，并按部门号排序

    a. 关联查询 
>  ``` sql
select count(t.id), t.deptid from 
	(SELECT a.id, a.name,a.salary, a.deptid   FROM test.employee a, (select avg(salary) salary, deptid from employee group by deptid) b
	 where  a.deptid = b.deptid and a.salary > b.salary) as t
 group by t.deptid
 ```

    
     b. SQl  比较
    
>  ``` sql
SELECT a.id, a.name,a.salary, a.deptid  FROM test.employee a where
a.salary > ((select avg(salary) salary from employee where deptid = a.deptid));
```


2 避免扫描 

> 看mysql帮助文档，例如，根据扫描的原理，下面的子查询语句要比第二条关联查询的效率高：
  1.  select e.name,e.salary where e.managerid=(select id from employee where name='zxx');   m + n
  2.   select e.name,e.salary,m.name,m.salary from employees e,employees m where
   e.managerid = m.id and m.name='zxx';   m*n


Here I took an example of weather data table. From method 2, I got a table size with 17.55 MB. 


Through select count I obtain the row numbers *164164*. Hence, the average size of each row is 112 Byte. 


In case of we have 1000 devices collecting data per second, each device has 10 parameters we need to store. So we are going to handle `10 * 1000 * 24 * 60 * 60`  
864 Million rows. If multiply 112 Byte then total data size is 92.3 GB data per day. 




> 2017年10月11日12:27:52 跟IT大佬曹政老师交流后，更新 

朱渊博，特普通一人，渊为深，博为广，一直努力做一个有深度有广度的人。多混迹于Github, Stackoverflow, ITeye, 知乎, Linkedin 等地带。蹉跎中练就了一身码农好本领。 酷爱编程，但绝不宅。爱生活，爱思考，性情温和但充满激情，做事有主见，信奉读万卷书行万里路。
<http://www.freerambo.com/about>

---

### END

