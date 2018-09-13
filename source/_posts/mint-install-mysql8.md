---
title: mysql-8.11 install
date: 2018.09.06 12:35:34
tags:
  - mysql install
  - 
---

# download mysql deb

    [下载地址](https://dev.mysql.com/downloads/repo/apt/)

# install deb

    [文档地址](https://dev.mysql.com/doc/mysql-apt-repo-quick-guide/en/)
    
# apt install mysql-server

# get root password

登录时不需要使用密码，提示找不到 UNIX socket

```
chen@chen-YangTianT4900c-00:~/download$ sudo mysqld_safe --skip-grant-tables --skip-syslog --skip-networking

Logging to '/var/lib/mysql/chen-YangTianT4900c-00.err'.
2018-09-06T02:56:11.619644Z mysqld_safe Directory '/var/run/mysqld' for UNIX socket file don't exists.

chen@chen-YangTianT4900c-00:~/download$ sudo -u mysql mysqld_safe --skip-grant-tables --skip-syslog --skip-networking

2018-09-06T02:56:18.987750Z mysqld_safe Logging to '/var/lib/mysql/chen-YangTianT4900c-00.err'.
2018-09-06T02:56:18.993047Z mysqld_safe Directory '/var/run/mysqld' for UNIX socket file don't exists.

```

创建mysqld文件夹，给与权限

```
sudo mkdir /var/run/mysqld
chen@chen-YangTianT4900c-00:~/download$ sudo chmod 777 /var/run/mysqld
```

登录
```
mysql -u root
```

修改密码,密码可从其他数据库上拷
```
update mysql.user set plugin='mysql_native_password', authentication_string='*8B98A17FF7D791BC9F0216DDC7658432C4B866FB' where user='root';
 FLUSH PRIVILEGES;
```

```
thanks to @thusharaK I could reset the root password without knowing the old password.

On ubuntu I did the following:

sudo service mysql stop
sudo mysqld_safe --skip-grant-tables --skip-syslog --skip-networking
Then run mysql in a new terminal:

mysql -u root
And run the following queries to change the password:

UPDATE mysql.user SET authentication_string=PASSWORD('password') WHERE User='root';
FLUSH PRIVILEGES;
In MySQL 5.7, the password field in mysql.user table field was removed, now the field name is 'authentication_string'.

Quit the mysql safe mode and start mysql service by:

mysqladmin shutdown
sudo service mysql start

```