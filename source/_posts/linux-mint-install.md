---
title: linux mint install
date: 2018.06.06 11:58:34
tags: 
    - linux mint
categories: linux
---

## linux mint系统

### linux mint安装

#### 制做rufus的启动盘
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;进入[官网下载软件](https://rufus.akeo.ie/ "refus")

#### 使用rufus的启动盘引导安装

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;由于本人的电脑为双显卡，因此在装双系统时，用refus引到盘进行安装mint时，无法进入linux系统内，原因是电脑在启动时无法正确使用显卡，因此在进入系统前，将配置信息成不使用显卡,其需要添加配置信息，可查看[CUDA官方文档](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#runfile "cuda")和[Linux Mint 18.1官方文档](https://www.linuxmint.com/rel_serena_cinnamon.php "linuxmint")

``` bash

 nomodeset

```
#### 安装linux mint时的指导文档

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[博客1](https://www.howtoing.com/install-linux-mint-18-alongside-windows-10-or-8-in-dual-boot-uefi-mode "1")<br/>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[博客2](http://www.cnblogs.com/qxym2016/p/6337036.html "2")<br/>

#### 启动linux系统时，不使用图形化界面

``` bash

#与添加nomodeset的方法一样，添加如下命令
single

```

#### 安装好后无法访问window下面的磁盘

