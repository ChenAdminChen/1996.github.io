---
title: python venv
date: 2018/1/26 15:33:45
tags: 
    - python
    - venv
    - virtualenv
---

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;基于python，创建一个virtual environments,用于存入特定项目所需要的包，方便快速搭建一个指定的python环境

## venv

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;python中自带一个venv，可以创建虚拟环境[学习地址](https://docs.python.org/3/library/venv.html "python venv")

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;按官网文档的要求，可以直接使用如下指令（本人将所有的虚拟环境创建在python35文件夹的venv内）

``` bash
D:\Users\Python\Python35\venv> python -m venv py35
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;按上面的指令进行创建时，可能会出现如下错误
![ServerConfguration](https://github.com/ChenAdminChen/1996_imgs/blob/master/blog_img/python/python-venv-1.png?raw=true)
若出来如上的错误时，可以将--without-pip命令添加进去，若并未出来如下错误，则说明成功

``` bash
D:\Users\Python\Python35\venv> python -m venv --without-pip py35
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;将命令--without-pip添加进去时，则添加了一个不带pip指令的虚拟环境，因此需要自行安装pip指令，若不带--without-pip命令时，则不需要安装pip指令
![ServerConfguration](https://github.com/ChenAdminChen/1996_imgs/blob/master/blog_img/python/python-venv-2.png?raw=true)

### 开启虚拟环境

#### window
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;进入刚创建好的py35/Scripts下运行activate.bat,如下图,开启虚拟环境
![ServerConfguration](https://github.com/ChenAdminChen/1996_imgs/blob/master/blog_img/python/python-venv-3.png?raw=true)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在虚拟环境下退出deactivate.bat

#### linux
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;进入刚创建好的py35,运行如下指令:
``` bash
root@root#opt/py35: source ./bin/activate   #开启虚拟环境
deactivate  #退出

```

### pip指令的安装

[pip指令安装学习地址](https://pip.pypa.io/en/stable/ "python pip")

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;下载get-pip.py放入指定的虚拟环境内的Scripts文件夹中，开启虚拟环境，运行python get-pip.py,如图：
![ServerConfguration](https://github.com/ChenAdminChen/1996_imgs/blob/master/blog_img/python/python-pip-1.png?raw=true)

## virtualenv

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;使用第三方库创建虚拟环境时,需要安装virtualenv包

``` bash
pip install virtualenv
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;安装成功后可,使用virtualenv进行创建虚拟环境

``` bash
 virtualenv –-no-site-packages py35  # --no-site-packages 用于创建一个不带任何第三方包干净的python虚拟环境
 virtualenv py35
```

## 虚拟环境中的包导入导出

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;进入指定的虚拟环境内运行pip freeze > requirements.txt,将该环境内的所有的第三方库导出到requirements.txt文件内,可以使用pip list输出的包与requirements.txt文件的包对比一下

``` bash
pip freeze > requirements.txt

```

![ServerConfguration](https://github.com/ChenAdminChen/1996_imgs/blob/master/blog_img/python/python-pip-2.png?raw=true)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;进入指定的虚拟环境内运行pip install -r requirements.txt,将该文件内的第三方库安装到该虚拟环境内

``` bash
pip install -r requirements.txt

```