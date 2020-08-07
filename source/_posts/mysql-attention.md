---
title: MySQL常用语句及注意事项
toc: true
categories: 与代码
tags: 
	- MySql 
date: 2018-03-08
---
### MySQL的一些常用语句及注意事项
#### 1.SQL 使用单引号来环绕文本值（大部分数据库系统也接受双引号）。如果是数值，请不要使用引号。

#### 2.可以在不删除表的情况下删除所有的行。（不要加*）这意味着表的结构、属性和索引都是完整的

``` mysql
DELETE  FROM table_name  
```

#### 3.修改表名称

``` mysql
alter table test2 rename to test3;
rename table test3to test4;
```

#### 4.`Between A and B`

Between A and B (边界A是包括的B是不包括的，不同的数据库对边界的处理不一样)，我实际操作中两边都是包括的

#### 5.表的复制： 

- 既复制表结构也复制表内容的SQL语句：
``` mysql
CREATE TABLE tab_new AS SELECT * FROM tab_old;  （as可以不写）
```

- 只复制表结构不复制表内容的SQL语句：
``` mysql
CREATE TABLE tab_new AS SELECT * FROM tab_old WHERE 1=2; （创建一个结构一样的表，as可以不写）
```

- 将一个表的部分内容复制到另一个表中：
``` mysql
insert into B(id,name,rate) select id,name,rate from A ;
```

- 查询内容插入另一张表：
``` mysql
insert into tablename(id,poNum,poCd,poType) select * from test limit 2600,100;
```

> limit：第一个数是从某个数的下一个（2601）开始，不写就是前多少条，第二个数是限制取多少条.MySQL不支持top关键字而是用limit来实现top的功能

#### 6.`UNION`
UNION 操作符用于合并两个或多个SELECT 语句的结果集。UNION 内部的 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每条 SELECT 语句中的列的顺序必须相同。UNION命令只会选取不同的值。如果允许重复的值，请使用UNION ALL。

``` mysql
SELECT column_name(s) FROM table_name1
UNION ALL
SELECTcolumn_name(s) FROM table_name2
```

#### 7.`SELECT INTO` 和 `IN`

`SELECT INTO` 语句从一个表中选取数据，然后把数据插入另一个表中。`IN` 子句可用于向另一个数据库中拷贝表：

``` mysql
SELECT *
INTO Persons_backup IN 'Backup.mdb'
FROM Persons
```

#### 8.CREATE 中的 char varchar

`CREATE DATABASE my_db` ，SQL中char容纳固定长度的字符串,varchar容纳可变长度的字符串.
``` mysql
CREATE TABLE persion
(
id bigint(20)  NOT NULL AUTO_INCREMENT,
last_name varchar(255),
first_name varchar(255),
address varchar(255),
city varchar(255),
PRIMARY KEY (`id`)
)
```

#### 9.`Constraints`约束
`Constraints`约束，约束用于限制加入表的数据的类型。可以在创建表时规定约束（通过` CREATE TABLE` 语句），或者在表创建之后也可以（通过 `ALTER TABLE` 语句）。主要的约束有`NOT NULL`、`UNIQUE`、`PRIMARY KEY`、`FOREIGN KEY`、`CHECK`、`DEFAULT`

#### 10.约束
每个表可以有多个 `UNIQUE` 约束，但是每个表只能有一个 `PRIMARY KEY` 约束。可以给约束起名字，创建表后添加约束,
``` mysql
ALTER TABLE Persons  ADD CONSTRAINT uc_PersonID UNIQUE (Id_P,LastName)
```
不同数据库的操作语句不太一样

#### 11.添加和删除主键
``` mysql
  alter table person addprimary key  (id);  alter table person drop primary key;
```

#### 12.表已经存在设置默认值
``` mysql
ALTER TABLE Persons  ALTERCity SET DEFAULT 'SANDNES'；
```
删除默认值
``` mysql
ALTER TABLE Persons ALTER City DROP DEFAULT（MySQL为例）
ALTER TABLE Persons ALTER COLUMN City DROP DEFAULT（其他数据库）
```

#### 13.索引、表、库操作
- 创建索引：
``` mysql
CREATE INDEX poNo ON Person (LastName DESC)；//索引在DDL中是用key表示
```
- 删除索引：
``` mysql
alter table Person drop index poNo;
```
- 删除表和数据库
``` mysql
drop table name；
drop database name；
```

#### 14.添加、修改、删除列、改变列属性
``` mysql
Alter table Person add birthday date(50)
Alter table person change birthday birth date(50)；
Alter table person drop column birthday；
Alter table Person modify column birthday varchar(50);
```
#### 15.`COUNT(DISTINCTcolumn_name)` 函数返回指定列的不同值的数目
``` mysql
SELECT COUNT(DISTINCT column_name) FROM table_name
```
#### 16.HAVING

在 SQL 中增加 HAVING 子句原因是 WHERE 关键字无法与合计函数一起使用。 
视图view的作用（为了安全性，可以隐藏不必要显示的数据）

#### 17.分组查询
``` mysql
SELECT  
  sum(CASE when score>=0 and score<60 then 1 else 0 end)   AS '不及格',  
  sum(CASE when score>=60 and score<70 then 1 else 0 end)   AS '及格',  
  sum(CASE when score>=70 and score<80 then 1 else 0 end)   AS '一般',  
  sum(CASE when score>=80 and score<90  then 1 else 0 end)   AS '良好',
  sum(CASE when score>=90 and score<=100 then 1 else 0 end)   AS '优秀'   
from student_score;  
```
#### 18.一天前（24小时前）数据
``` mysql
select * from person where create_time < NOW() - interval 1 day;
select * from person where create_time < NOW() - interval 24 hour;
```
#### 19.显示表结构，显示建表语句
``` mysql
desc tablename;
SHOW CREATE TABLE table_name;
```
#### 20.显示索引
``` mysql
show index from person;
```
#### 21.查询区分大小写
``` mysql
select  * from  table_name where  binary  name like  'a%';   
```
#### 22.判断字段中是否含有中文，查出带汉字的数据
``` mysql
SELECT * FROM person WHERE length(`name`) <> CHAR_LENGTH(`filename`)
```
`length ()`函数计算包含字节的长度，一个汉字是3个字节，一个数字或字母是一个字节；
`char_length()`是计算包含字符长度， 汉字，数字和字母都是一个字符。 
所以查询条件中，长度相等，则字段中无汉字；长度不相等，则有汉字。

#### 23.查看执行计划
``` mysql
explain select * from person where ....
```

#### 24.超多分页查询优化
``` mysql
select a.* from tabelname a,(select id from tablename where ... limit 10000,20) b where a.id = b.id;
```

#### 25.用户相关
- 创建用户
``` mysql
use mysql; 
grant all on *.* to root@'%' identified by '123' with grant option; 
commit;
```
- 修改密码
``` mysql
grant all on *.* to xing@'localhost' identified by '123456' with grant option; 
update user set password = password('newpwd') where user = 'xing' and host='localhost'; 
flush privileges;
```

#### 26.备份与恢复
- 备份
``` mysql
mysqldump -h 192.168.3.143 -u root -p pwd -x --default-character-set=gbk >C:\testdb.sql
```
- 恢复
``` mysql 
ysql -u root -pleizhimin testdb <C:\testdb.sql
```
