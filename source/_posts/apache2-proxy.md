---
title: apache2-proxy
date: 2018.08.09 12:35:34
tags:
  - apache2 proxy
---

## apache2 proxy 说明

&nbsp;&nbsp;&nbsp;&nbsp;apache2 proxy的代理方便多个接口服务的管理，它可设置一台计算机中的某个一个端口可指向多个服务接口，该服务可存在于本地，也可存在于不同服务器上。
    

> apache2 proxy service 安装说明

&nbsp;&nbsp;&nbsp;&nbsp;下载安装apach2

> 开启服务

```
service apache2 restart
```

> 查看处理日志 

```
cat /var/log/apache2/error.log 
```

> 查看apache软件结构

```

chen@chen-T4 /etc/apache2 $ cat apache2.conf

```

部分结果如下：

```

/etc/apache2/
#       |-- apache2.conf
#       |       `--  ports.conf
#       |-- mods-enabled
#       |       |-- *.load
#       |       `-- *.conf
#       |-- conf-enabled
#       |       `-- *.conf
#       `-- sites-enabled
#               `-- *.conf
#

```

-- apache2.conf： 讲解了apache的目录结构，加载顺序<br/>

-- ports.conf: 使用的端口地址

-- mods-enabled: 加载的module

-- conf-enabled: 字符集的配置

-- sites-enabled: proxy service的配置文件

```
sudo nano 000-default.conf
```

>查看所有可安装的module

```
cd /usr/lib/apache2/modules/
```

>加载module

```

#加载module

a2enmod proxy

```

>配置代理所需要的module

```
ssl proxy proxy_connect proxy_balancer
```

>配置代理

```
#进入配置文件
cd /etc/apache2/sites-enabled

#添加访问http的代理
 ProxyPass /online http://192.111.12.25:8182/api

#添加访问https的代理
ProxyPass /portal https://192.111.12.25/portal

#项目中存在重定向的，需要再加上如下配置
ProxyPassReverse /portal https://yifenganxin.com/portal

```