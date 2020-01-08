---
layout:     post
title:      "What is innovation?"
subtitle:   ""
date:       2019-11-20 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - sharing
    
---

Syntax
The syntax for the IN condition in Oracle/PLSQL is:

expression IN (value1, value2, ... value_n);
OR

expression IN (subquery);
Parameters or Arguments
expression
The value to test.
value1, value2, ... value_n
The values to test against expression.
subquery
This is a SELECT statement whose result set will be tested against expression. If any of these values matches expression, then the IN condition will evaluate to true.

Example - With Character
Let's look at an Oracle IN condition example using character values.

The following is an Oracle SELECT statement that uses the IN condition to compare character values:

```sql
SELECT *
FROM customers
WHERE customer_name IN ('IBM', 'Hewlett Packard', 'Microsoft');

```
This Oracle IN condition example would return all rows where the customer_name is either IBM, Hewlett Packard, or Microsoft. Because the * is used in the SELECT, all fields from the customers table would appear in the result set.

The above IN example is equivalent to the following SELECT statement:

```sql
SELECT *
FROM customers
WHERE customer_name = 'IBM'
OR customer_name = 'Hewlett Packard'
OR customer_name = 'Microsoft';
As you can see, using the Oracle IN condition makes the statement easier to read and more efficient.
```



mysql.server stop
mysql.server start

1、UNIX时间戳转换为日期用函数： FROM_UNIXTIME()

select FROM_UNIXTIME(1156219870);



2、日期转换为UNIX时间戳用函数： UNIX_TIMESTAMP()

    Select UNIX_TIMESTAMP('2006-11-04 12:23:00');
    
    
---

### END

