---
title: mysql error 总结
date: 2017/11/22 09:35:34
tags:
  - mysql
  - mysql-error
categories: mysql
---

## ERROR 1728 (HY000)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color=red>ERROR 1728 (HY000) CANNOT LOAD FROM MYSQL.PROC. THE TABLE IS PROBABLY CORRUPTED</font>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在对mysql软件进行升级后，可能存在mysql内的内置表没有升级成功，因此需要自动更新数据库，进入cmd内，执行如下代码：

``` bash
mysql_upgrade -u root -p passward
``` 

