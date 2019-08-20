---
title: mysql-context-search
date: 2018.11.22 16:00:00
reward: false
tags: 
    - mysql
    - full-text
---

## mysql context search
   mysql的表中的字段进行全文搜索,(学习地址)[https://dev.mysql.com/doc/refman/8.0/en/fulltext-search.html]
   
   
### 建立指定需要进行全文搜索的字段的的索引

>索引创建
```mysql
ALTER TABLE `user` 
    ADD FULLTEXT INDEX `ft_user` (`name`, `email`, `phone`),
    ADD INDEX `index9` (`name` ASC, `email` ASC, `phone` ASC);
    ;
```

>查询
```mysql
#无法查询一个字段
SELECT id,name,phone,email from user where match(name,email,phone) against("张" IN BOOLEAN MODE);

#查询成功
SELECT id,name,phone,email from user where match(name,email,phone) against("张小" IN BOOLEAN MODE);

```
    
### window解决问题

>在my.ini配置文件中添加如下

```
innodb_ft_min_token_size=1
ngram-token-size=1
```

>重启mysql server

>reset index

```mysql
ALTER TABLE `yfaf`.`user` 
ADD FULLTEXT INDEX `index9` (`name`, `phone`, `email`) WITH PARSER ngram;
;

```

### linux解决问题

>在/etc/mysql/mysql.conf.d/mysqld.cnf配置文件中添加如下

注意项：需要在[mysqld]的组下添加
```
[mysqld]
.....
innodb_ft_min_token_size=1
ngram-token-size=1
```

>重启mysql server

>reset index

```mysql
ALTER TABLE `yfaf`.`user` 
ADD FULLTEXT INDEX `index9` (`name`, `phone`, `email`) WITH PARSER ngram;
;
