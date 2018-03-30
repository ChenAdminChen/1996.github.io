---
title: linux cmd 总结
date: 2018.03.30 16:58:34
tags: 
    - linux cmd
categories: linux
---

## linux指令

### linux下运行python虚批环境

``` bash

#开启虚拟环境
root@root#opt/py35: source ./bin/activate

#退出
deactivate

# 后台运行python程序，将运行地址切换到需要运行的python文件下,将有日志打印在文件内
root@ nohup python main.py $

```

### servicemix的运行

``` bash
#进入servicemix的文件夹下
root@4xe7t:/opt/apache-servicemix-7.0.1# ./bin/servicemix


```

### tigase的运行

``` bash

#运行
root@4xe7t:/tigase-server-7.1.0-b4379/script# ./tigase.sh start ../etc/tigase.conf

#关闭
root@4xe7t:/tigase-server-7.1.0-b4379/script# ./tigase.sh stop ../etc/tigase.conf

```

### 查看进行进程

``` bash

#查看与python相关的进程
ps aux | grep python

#查看正在运行的进程
ps aux

#杀死某个进程
kill -9 id

```