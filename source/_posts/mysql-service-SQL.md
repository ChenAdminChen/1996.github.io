---
title: mysql SQL (二)
date: 2018.04.26 09:54:45
reward: false
tags:
    - mysql
    - SQL
---

### mysql lock

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;每张表有锁定时间超时的设置，下面是将超时时间设置为600

``` bash
SHOW VARIABLES LIKE '%innodb_lock_wait%';

SET innodb_lock_wait_timeout=600;

```
