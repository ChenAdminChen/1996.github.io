---
title: mysql表的列、外键与索引
date: 2017/11/28 20:36:45
reward: false
tags: 
    - mysql
    - SQL
    - DDL
---

### 查看表结构

``` bash
show create tabel table_name;

```

### 对列的操作

``` bash
#查看列
show full columns from table_name

#删除一列
alter table 表名 drop column 列名

#添加一列
alter table 表名 add 列名 varchar(20) default 0

```

### 对外键的操作

外键的使用条件：
<ul>
	<li>两个表必须是InnoDB表，MyISAM表暂时不支持外键（据说以后的版本有可能支持，但至少目前不支持）。</li>
	<li>外键列必须建立了索引，MySQL 4.1.2以后的版本在建立外键时会自动创建索引，但如果在较早的版本则需要显示建立，若外键不存在索引则会容易引起死锁。 </li>
	<li>外键关系的两个表的列必须是数据类型相似，也就是可以相互转换类型的列，比如int和tinyint可以，而int和char则不可以。</li>
</ul>

外键的好处：可以使得两张表关联，保证数据的一致性和实现一些级联操作。

``` bash
#增加表外键
# [ON DELETE {RESTRICT | CASCADE | SET NULL | NO ACTION | SET DEFAULT}]
# [ON UPDATE {RESTRICT | CASCADE | SET NULL | NO ACTION | SET DEFAULT}]

alter table table_name add constraint foreign_key_name  foreign key (column_name) references table_name(column_name) on delete on action;

```

该语法可以在 CREATE TABLE 和 ALTER TABLE 时使用，如果不指定CONSTRAINT symbol，MYSQL会自动生成一个名字。
ON DELETE、ON UPDATE表示事件触发限制，可设参数：
<ul>
	<li>RESTRICT（限制外表中的外键改动）</li>
	<li>CASCADE（跟随外键改动）</li>
	<li>SET NULL（设空值）</li>
	<li>SET DEFAULT（设默认值）</li>
	<li>NO ACTION（无动作，默认的）</li>
</ul>

``` bash
#删除外键
alter table table_name drop foreign key foreign_key_name;

drop foreign key foreign_key_name on table_name;

```

### 对索引的操作

``` bash
#查看表索引
show index from table_name;

#增加表唯一索引
ALTER TABLE table_name ADD UNIQUE INDEX `index_name` (`columns_name` ASC)
#增加表的主键
ALTER TABLE table_name ADD PRIMARY KEY (columns_name)

#删除表索引
alter table table_name drop index index_name;

drop index index_name on table_name

```
