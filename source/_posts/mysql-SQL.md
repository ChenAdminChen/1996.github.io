---
title: mysql SQL定义
date: 2017/11/28 20:36:45
reward: false
tags: 
    - mysql
    - SQL
---

### TCL（Transaction Control Language）

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;事务控制语言

``` bash
SAVEPOINT  #设置保存点 
ROLLBACK  #回滚 
START TRANSACTION   #开始事务
COMMIT  #提交事务
``` 

### DML（data manipulation language）

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据操作语言 它们是SELECT、UPDATE、INSERT、DELETE，这4条命令是用来对数据库里的数据进行操作的语言

``` bash
1.CREATE - to create objects in the database   #创建
2.ALTER - alters the structure of the database   #修改
3.DROP - delete objects from the database   #删除
4.TRUNCATE - remove all records from a table, including all spaces allocated for the records are removed
```

### DDL（data definition language）

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;DDL比DML要多，主要的命令有CREATE、ALTER、DROP等，DDL主要是用在定义或改变表（TABLE）的结构，数据类型，表之间的链接
和约束等初始化工作上，他们大多在建立表时使用

``` bash
1.SELECT - retrieve data from the a database           #查询
2.INSERT - insert data into a table                    #添加
3.UPDATE - updates existing data within a table    #更新
4.DELETE - deletes all records from a table, the space for the records remain   #删除
5.CALL
6.EXPLAIN PLAN 
7.LOCK TABLE - control concurrency     #锁，用于控制并发

```

### DCL（Data Control Language）

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;是数据库控制功能。是用来设置或更改数据库用户或角色权限的语句，包括（grant,deny,revoke等）语句。在默认状态下，只有sysadmin,dbcreator,db_owner或db_securityadmin等人员才有权力执行DCL

``` bash
GRANT  #授权

REVOKE #取消授权
```

### mysql远程连接

``` bash
 mysql -h 192.168.1.168 -u test -p -D specialist
```
