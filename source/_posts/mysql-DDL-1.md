---
title: mysql SQL DDL
date: 2017/11/28 20:36:45
reward: false
tags: 
    - mysql
    - SQL
    - DDL
---



### 对索引的操作

``` bash
#查看表索引
show index from table_name;

#增加表索引
ALTER TABLE table_name ADD UNIQUE INDEX `index_name` (`columns_name` ASC)
ALTER TABLE table_name ADD PRIMARY KEY (columns_name)

#删除表索引
alter table table_name drop index index_name;

drop index index_name on table_name

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