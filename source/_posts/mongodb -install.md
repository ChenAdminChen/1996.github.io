---
title: mongodb 安装
tags: mongodb
categories: mongodb
date: 2017/11/18 22:51:34
---

### mongodb的下载
（1）从[官网地址](https://www.mongodb.com/download-center#community)上找到系统相应的安装文件，然后下载
（2）安装运行

### mongodb的安装
(1)下载服务器，安装运行
(2)自行创建存储数据的文件夹，一般用data\db作名字
(3)创建存储日志的文件

### 将mongodb做成window系统中内置的服务
（1）用管理的身份运用命令行
（2）将mongodb运行于window服务内，命令如下：
``` bash
mongod.exe --install --logpath D:\data\log.txt --serviceName mongodb 
```
（3)将启动mongodb服务
## 运行mongodb
(1)运行mongo


