---
title: linux command
date: 2018.06.011 10:58:34
tags: 
    - linux command
categories: linux
---

## linux指令

### linux下对文件的操作

#### nano

``` bash

#若不存在一个文件时，创建一个requirement.txt新文件，若文件存在，则直接打开文件
nano requirement.txt

```

#### 两台linux系统传送文件

``` bash
#scp local_file remote_username@remote_ip:remote_folder 

#将根目录下的t开头的sql传送到192.168.60.51的根目录下
scp ~/t*.sql root@192.168.60.51:~/

```

#### 远程连接sevicemix

``` bash
#连接smx
ssh smx@192.168.2.168 -p8101

```

#### 远程连接另一台linux

``` bash

#连接linux
ssh root@192.168.2.168 -p22

```

#### 查看内存使用情况

``` bash

free -h

```

#### 查看各进程内存使用情况

``` bash

top -o %MEM

```

####