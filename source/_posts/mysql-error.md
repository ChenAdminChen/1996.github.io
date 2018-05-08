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

## 数据导入出错

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当A表的数据由B表的数据插入时用触发器生成时，若向A导入数据时可能会出现错：

触发器可能会导致主键重复的错误，原因是A表中的数据由此触发器插入，但数据导入脚本也导入了已有数据，导致冲突。

因此，在触发器中增加对用户自定义变量 @disable_triggers 的检查，此变量为null时才执行动作。当需要禁止触发器动作时，设置此变量为1即可。

然后修改数据导入脚本，在前、后分别增加 禁止触发器 和 恢复触发器 的设置。