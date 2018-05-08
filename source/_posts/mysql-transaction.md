---
title: mysql transaction
date: 2018.05.03 09:50:34
tags:
  - mysql
  - transaction
categories: mysql
---

### mysql四种隔离级别

#### read committed

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;只能读取到已经提交的数据。Oracle等多数数据库默认都是该级别 (不重复读)

#### read uncommitted

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;允许脏读，也就是可能读取到其他会话中未提交事务修改的数据

### book结构
``` bash

create table book(
  id int(11),
  name varchar(11)
);

```

#### 脏读

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;脏读就是指当一个事务正在访问数据，并且对数据进行了修改，而这种修改还没有提交到数据库中，这时，另外一个事务也访问这个数据，然后使用了这个数据。在read uncommitted可脏话

``` bash
#session A -> read uncommitted
start transaction;

set session transaction isolation level read uncommitted;

insert into book(id, name) values(3,'javaScript');

mysql> select * from book
+------+------------+
| id   | name       |
+------+------------+
|    1 | java       |
|    2 | python     |
|    3 | javaScript |
+------+------------+
3 rows in set (0.00 sec)

commit;

#session B -> read uncommitted
start transaction;

set session transaction isolation level read uncommitted;

#before session A commit
mysql> select * from book
+------+------------+
| id   | name       |
+------+------------+
|    1 | java       |
|    2 | python     |
|    3 | javaScript |
+------+------------+
3 rows in set (0.00 sec)


#session C -> repeatable read

#before session A commit
mysql> select * from book;

+------+--------+
| id   | name   |
+------+--------+
|    1 | java   |
|    2 | python |
+------+--------+
2 rows in set (0.00 sec)

#after session A commit
mysql> select * from book
+------+------------+
| id   | name       |
+------+------------+
|    1 | java       |
|    2 | python     |
|    3 | javaScript |
+------+------------+
3 rows in set (0.00 sec)

```

#### 不可重复读
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;是指在一个事务内，多次读同一数据。在这个事务还没有结束时，另外一个事务也访问该同一数据。那么，在第一个事务中的两次读数据之间，由于第二个事务的修改，那么第一个事务两次读到的的数据可能是不一样的。这样就发生了在一个事务内两次读到的数据是不一样的，因此称为是不可重复读。

#### repeatable read

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;可重复读。在同一个事务内的查询都是事务开始时刻一致的，InnoDB默认级别。在SQL标准中，该隔离级别消除了不可重复读，但是还存在幻象读

#### serializable

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;完全串行化的读，每次读都需要获得表级共享锁，读写相互都会阻塞

### 操作当前会话的隔离级别

隔离级别 | 脏读（Dirty Read） | 不可重复读（NonRepeatable Read） | 幻读（Phantom Read） 
 | - | - | - | - | 

未提交读（Read uncommitted） | 可能 | 可能 | 可能

已提交读（Read committed） | 不可能 | 可能 | 可能

可重复读（Repeatable read） | 不可能 | 不可能 | 可能

可串行化（Serializable ） | 不可能 | 不可能 | 不可能

``` bash
#查看当前会话的隔离级别
select @@session.tx_isolation;

#修改当前会话的隔离级别
set [session / global] transaction isolation level [read committed / read uncommitted / repeatable read /serializable]

```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认的行为（不带session和global）是为下一个（未开始）事务设置隔离级别。如果你使用GLOBAL关键字，语句在全局对从那点开始创建的所有新连接（除了不存在的连接）设置默认事务级别。你需要SUPER权限来做这个。使用SESSION 关键字为将来在当前连接上执行的事务设置默认事务级别。 任何客户端都能自由改变会话隔离级别（甚至在事务的中间），或者为下一个事务设置隔离级别。

## 数据导入出错

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当A表的数据由B表的数据插入时用触发器生成时，若向A导入数据时可能会出现错：

触发器可能会导致主键重复的错误，原因是A表中的数据由此触发器插入，但数据导入脚本也导入了已有数据，导致冲突。

因此，在触发器中增加对用户自定义变量 @disable_triggers 的检查，此变量为null时才执行动作。当需要禁止触发器动作时，设置此变量为1即可。

然后修改数据导入脚本，在前、后分别增加 禁止触发器 和 恢复触发器 的设置。