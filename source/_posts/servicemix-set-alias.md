---
title: servicemix set alias
date: 2018.4.17 09:36:45
reward: false
tags: 
    - servicemix
    - alias
---

### servicemix设置命令别名

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;servicemix服务器使用了OSGI协议，而OSGI的目的是模块化，就是为了将一个大的应用分解成较小的模块，这些模块物理上就是一个个的jar包，也就是OSGI bundle。OSGI规范就是指导怎么令这些bundle能更好的有高内聚性、有松藕性，能更好地被复用。至于被神化的“动态性”、“热插拔”的特性，则是OSGI规范带来的一种可能，并不是一定会有的。因为OSGI的特性，在servicemix中某个应用程序所依赖的bundle过多时，则需要快速查找到某一类型的bundle，在servicemix中可以用如下命令

``` bash

#查找与mybatis相关的bundle
la | grep mybatis

```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;结果如下图:

![servicemix](https://github.com/ChenAdminChen/1996_imgs/blob/master/blog_img/servicemix/pre_defined_alias.png?raw=true)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;若每次查找mybatis相关的bundle时，都需要输入该指令，则会相对复杂，好在servicemix可以自定义指令，若想定义 ‘my’ 指令用于代替la | grep mybatis，其操作如下：

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在etc/shell.init.script文件中配置如下指令:

![servicemix](https://github.com/ChenAdminChen/1996_imgs/blob/master/blog_img/servicemix/set_alias.png?raw=true)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;重启servicemix后使用指令 my 可得如下图:

![servicemix](https://github.com/ChenAdminChen/1996_imgs/blob/master/blog_img/servicemix/cure_defined_alias.png?raw=true)