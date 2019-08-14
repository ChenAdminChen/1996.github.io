---
title: linux command
date: 2018.06.011 10:58:34
tags: 
    - linux command
categories: linux
---

## linux指令


### linux下运行python虚批环境

开启虚拟环境
>root@root#opt/py35: source ./bin/activate

退出
>deactivate

后台运行python程序，将运行地址切换到需要运行的python文件下,将有日志打印在文件内
>root@ nohup python main.py $

### servicemix的运行

进入servicemix的文件夹下
>root@4xe7t:/opt/apache-servicemix-7.0.1# ./bin/servicemix

### tigase的运行

运行
>root@4xe7t:/tigase-server-7.1.0-b4379/script# ./tigase.sh start ../etc/tigase.conf

关闭
>root@4xe7t:/tigase-server-7.1.0-b4379/script# ./tigase.sh stop ../etc/tigase.conf

### linux path 加临时性代理

以下方式为session会话，关闭cmd则消息

>export HTTP_PROXY=127.0.0.1:1080

>export HTTPs_PROXY=127.0.0.1:1080

### linux path 永久设置

    ./profile文件内修改才行

### 查看进行进程

``` bash

#查看与python相关的进程
ps aux | grep python

#查看正在运行的进程
ps aux

#杀死某个进程
kill -9 id

```

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

#从服务器上将文件下载下来
scp -r root@192.168.60.51:~/web /home/chen/web

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

#### linux 传送文件到window
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;需要window开启ftp协议，并且共享某个文件夹

<pre><code>
chen@chen-T4 ~/work/code/yfaf $ ftp 192.168.22.168
Connected to 192.168.22.168.
220 Microsoft FTP Service
Name (192.168.22.168:chen): cc
331 Password required for cc.
Password:
530 User cannot log in.
Login failed.
Remote system type is Windows_NT.
ftp> ls
530 Please login with USER and PASS.
ftp: bind: Address already in use
ftp> type binary  //转换成 binary
200 Type set to I.
ftp> put data.jar data.jar //将data.jar 传上去 需要定义新文件的名字
</code></pre>

#### linux 上传文件到linux

```
#将yfaf*.sql 的所有文件上传到175.6.56.51服务器上
scp yfaf*.sql root@175.6.56.51:~/
```

#### linux history

