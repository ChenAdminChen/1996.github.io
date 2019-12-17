---
title: linux-crontab 
date: 2019.12.17 10:35:34
tags:
  - linux
---

## linux 定时任务

### 定时执行的任务

> automatic-refund.sh
```shell script
#!/bin/bash

#获取系统当前时间
start=`date -d yesterday +"%Y-%m-%d"`
end=`date -d today +"%Y-%m-%d %H:%M:%S"`
echo ${start} - ${end}

result=`curl -X POST 'http://localhost:8080/automatic-refund?start='+${start}+'&end='+${end} -H 'Content-Type: application/json' -v`
# curl -d "start=${start}&end=${end}" http://localhost:8080/automatic-refund -v

#输出日志到日志文件
echo ${end}:${result} >> /home/chen/work/autorefund.log


```

### 设定定时器

> crontab -e
```shell script
# Edit this file to introduce tasks to be run by cron.
# 
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
# 
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').# 
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
# 
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
# 
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
# 
# For more information see the manual pages of crontab(5) and cron(8)
# 
# m h  dom mon dow   command

#基本格式 :
#*　　*　　*　　*　　*　　command
#分　时　日　月　周　命令

#脚本所在的位置 写绝对路径最好
#每天的15点 35分 
35 15 * * * bash /home/chen/work/automatic-refund.sh

```

### 查看定时任务
>crontab -l

### 错误解决

若定时任务设定好后，没有按设定的执行时，可检查系统时间是否存在不一致的问题