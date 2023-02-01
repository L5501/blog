---
title: 常用sql语句
description: 经常会用到，但是经常忘记的sql语句
data: 2022-12-5 20:32:00
updata: 2022-12-5 20:32:00
tags: 
  - java
categories:
  - java笔记
---
##### 展示所有数据库

```sql
show databases
```

##### 创建一个student_grade表

```sql
CREATE TABLE student_grade(
id int,
name VARCHAR(20),
enligsh DECIMAL(3,1),
math DECIMAL(3,1),
chinese DECIMAL(3,1)
)
```

*此处enligsh为故意写错，方便下面修改示范*

##### 插入两个数据

```sql
INSERT INTO student_grade ( id, NAME, enligsh, math, chinese )
VALUES
	(1,'李狗蛋',0,0,0),
	(2,'聪明的李某人',99,99,99);
```

##### 查看表结构

```sql
desc student_grade
```

此时发现enligsh拼写错误

##### 修改字段名

参考例句：

```sql
ALTER TABLE 表名 CHANGE 旧字段名 新字段名 新数据类型;
```

此处所用：

```sql
ALTER TABLE student_grade CHANGE enligsh  english DECIMAL(3,1);
```

这个方法也可以用来修改指定字段的数据类型，***但是修改字段类型需要保证改后的数据类型能兼容之前的数据***，例如此处我修改name的长度

```sql
ALTER TABLE student_grade CHANGE name name VARCHAR(100);
```