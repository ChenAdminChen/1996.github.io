---
title: servicemix long distance debug
date: 2018.4.17 10:36:45
reward: false
tags: 
    - servicemix
    - debug
---

### servicemix设置命令别名

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ServiceMix是一个建立在JBI (JSR 208)语法规则和APIs上的开源ESB(Enterprise Service Bus:企业服务总线)。 它包括一个完整的JBI容器，其主要是由标准化信息服务和路由器，JBI管理MBeans，JBI配置单元和Ant任务（安装组 件和管理容器）组成。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;版本 ：Apache ServiceMix 7.0.0 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;调试工具：IntelliJ IDEA 2017.1.4 x64

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;servicemix控制台设置:
找到本地安装的servicemix文件夹，找到bin目录，cmd执行命令servicemix.bat debug即可，可以看到端口号 5005

![servicemix](https://github.com/ChenAdminChen/1996_imgs/blob/master/blog_img/servicemix/pre_defined_alias.png?raw=true)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这样，servicemix这里就设置好了，这个端口号是可以在servicemix.mix可以设置的，打开bin目录下servicemix.bat文件
![servicemix](https://github.com/ChenAdminChen/1996_imgs/blob/master/blog_img/servicemix/set_alias.png?raw=true)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;配置好了，只要servicemix启动之后，运行刚刚设置的Remote，就可以远程调试了
![servicemix](https://github.com/ChenAdminChen/1996_imgs/blob/master/blog_img/servicemix/set_alias.png?raw=true)
