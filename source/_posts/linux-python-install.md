---
title: linux python install
date: 2018/01/25 19:36:45
reward: false
tags:
    - linux
    - python
---

### 下载python3.6.4安装包

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[python3.6.4安装包下载地址](https://www.python.org/downloads/release/python-364/ "python downloads")
![ServerConfguration](https://github.com/ChenAdminChen/1996_imgs/blob/master/blog_img/python/python-download.png?raw=true)

### linux下解压Python-3.6.4.tgz

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;将下载好的包放入linux下的opt文件夹内，使用如下命令解压包,完成后将会出现一个Python-3.6.4文件夹

``` bash
root@virtual-machine:/opt# tar -zvxf Python-3.6.4.tgz
```

### 创建存Python环境的文件夹

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;个人将python安装在opt文件夹内，因此进行opt文件夹内创建python3文件夹

``` bash
root@virtual-machine:/opt# mkdir python3
```

### 入解压后的目录，编译安装(Python-3.6.4)

``` bash
#./configure --prefix 是设定软件安装到哪里。设置好参数，运行./configure，会生成makefile文件#
root@virtual-machine:/opt/Python3.6.4# ./configure --prefix=/opt/python3
```

### make 编译
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Configure 生成了makefile文件，运行make就可以完成编译。make是将读入所有由configure脚本程序建立的制作文件。这些制作文件会告诉make哪些文件需要被编译以及按照怎样的顺序对它们进行编译，因为可能会有上百个源程序文件。当make工作的时候，会在屏幕上显示出正在执行的每一个命令，以及与这个命令相关的全部参数。这些输出通常都是编译器的调用声明和所有传递给编译器的参数。如果编译器顺利地完成了工作，就不会出现什么错误信息。大多数编译器的错误信息十分清楚和明确，因此不用担心可能会漏掉一个错误。如果确实看到有一错误，也不用慌张。大多数错误信息并不反映出程序本身出现了一个问题，通常都是系统这里或者那里的问题。典型情况下，这些信息大多是因为文件访问权限不正确而产生的或者是因为文件没有找到

``` bash
root@virtual-machine:/opt/Python3.6.4# make
```

### make install

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;执行make install，这个命令将启动安装脚本程序。因为make命令会在执行每一个命令的时候把它显示出来，所以将会看到许许多多的文字掠过眼前。如果没有看到什么错误信息，就说明这个软件包安装好了。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;卸载：make uninstall

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;注意：如果下载的包里已经有了makefile 文件，就说明已经configure过了，直接安装就可以了。

``` bash
root@virtual-machine:/opt/Python3.6.4# make install
```

### 设置PATH路径

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;进入ect文件夹中的profile，添加PATH路径,如下图的内容

``` bash
#进入文件夹内
# i: 进行文件内编辑文件内容   
# 按ESC，输入:wq回车退出。
root@virtual-machine:/ect# vim profile
root@virtual-machine:/ect# source ./ect/profile   #对文件进行保存，并保证文件内容生效

```

![ServerConfguration](https://github.com/ChenAdminChen/1996_imgs/blob/master/blog_img/python/linux-python-1.png?raw=true)

### 检查python的版本信息

``` bash

root@virtual-machine:python3 -V

```

[学习地址](http://www.cnblogs.com/kimyeee/p/7250560.html "网址")

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;注： 另一种安装方式

``` bash
sudo add-apt-repository ppa:jonathonf/python-3.6
sudo apt-get update
sudo apt-get install python3.6
```