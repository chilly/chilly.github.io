---
layout: post
title: '选择一定的日期oracle与mysql的实现'
date: 2006-08-30 09:35
comments: true
tags: ['oracle','mysql','date']
categories:
- [knowledge, database]
---

表中列的类型定义为TIMESTAMP

oracle的语句实现：
```sql
select * from table where to_char(CREATE_DATE,'yyyy-mm-dd') > '1998-07-12' and
to_char(CREATE_DATE,'yyyy-mm-dd') < '1999-12-20';

```
这里的table为表名，CREATE_DATE为时间的字段名。转换格式为'yyyy-mm-dd'，查找的是'1998-07-12'到 '1999-12-20'之间的记录。

mysql的语句实现：
```sql
select * from table where date_format(CREATE_DATE,'%Y-%m-%d')> '1998-07-12'
and  date_format(CREATE_DATE,'%Y-%m-%d')< '1999-12-20';
```
解释同上。

