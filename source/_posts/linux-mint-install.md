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

#### nomodeset single进入无图形化界面

##### 检查nouveau驱动是否被禁用

如果有任何输出信息，表明nouveau驱动被启用。

##### 禁用nouveau驱动（必要）

> 创建文件/etc/modprobe.d/blacklist-nouveau.conf，内容如下：

```
blacklist nouveau
options nouveau modeset=0
```

> 重新生成kernel initramfs，终端输入：

```
sudo update-initramfs -u
```

##### nomodeset模式下安装nvidia驱动

1） CUDA官方文档上说，如果要安装nvidia显卡驱动，那么必须保证nouveau驱动被禁用。可是nvidia驱动还没安装上，那岂不是没有显卡驱动了吗？幸运的是，这里可以让系统临时进入nomodeset模式，它采用了一种”软显示“模式。

重启系统进入nomodeset模式：参考https://www.linuxmint.com/rel_serena_cinnamon.php里的Solving freezes部分。

2） 在nomodeset模式下，先按步骤1检查nouveau驱动是否被禁用，确保其禁用。再安装nvidia驱动

#### 

#### 

### 安装好重启电脑

> grub rescue

[学习地址](https://askubuntu.com/questions/635052/how-do-i-use-grub-rescue '3')
```
#获得分区信息
grub rescue>ls 
(hd1,msdos7) (hd1,msdos8) ....

grub rescue>ls (hd1,msdos7)
.... /boot   //则(hd1,msdos7)为启动盘

grub rescue>set prefix=(hd1,msdos7)/boot/grub
grub rescue>set root=(hd1,msdos7)

grub rescue>insmod normal
grub rescue>normal

grub>insmod linux
grub> linux /boot/vmlinuz-3.13.0-29-generic root=/dev/sda1
grub> initrd /boot/initrd.img-3.13.0-29-generic
grub> boot

```

> 自动进行修复
[学习地址](https://help.ubuntu.com/community/Boot-Repair '4')
```
sudo add-apt-repository ppa:yannubuntu/boot-repair
sudo apt-get update
sudo apt-get install -y boot-repair && boot-repair
```

> 重启电脑无法选择window系统
    
    linux系统下使用apt下载os-prober
```
apt install os-prober

sudo os-prober
```    

> 重启电脑后发现无法自动进入系统，出现error not find device

 linux系统使用下面指令,[学习地址](https://askubuntu.com/questions/143667/boot-error-no-such-device-grub-rescue '1')
 
 ```
    grub-install /dev/sda  -- 硬盘
    grub-install /dev/sdb  -- 机器硬盘
    update-grub
```

    

