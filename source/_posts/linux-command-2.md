---
title: linux
date: 2019.11.22 19:35:34
tags:
  - linux command
---

### Limits on Number of Files and Processes (ulimit)

1. user can open file that size < ??

```shell script
ulimit -u

# result : 1024
```

2. user can create process(nproc)
```shell script
# select all user create process
ps h -Led -o user | sort | uniq -c | sort -n

# select one user create process
ps -o nlwp,pid,lwp,args -u chen | sort -n
```

3. update process number

if command print error, that can change user can process number
```shell script
Cannot create GC thread. Out of system resources  
java.lang.OutOfMemoryError: unable to create new native thread
```

+ 需要先看linux操作系统内核版本，通过uname -a查看内核版本，因为2.6版本的内核默认是在/etc/security/limits.d/90-nproc.conf里的配置会覆盖/etc/security/limits.conf的配置

> uname -a

Linux hadoop219 2.6.32-431.el6.x86_64 #1 SMP Fri Nov 22 03:15:09 UTC 2013 x86_64 x86_64 x86_64 GNU/Linux
 
+ 修改配置文件

vi /etc/security/limits.d/90-nproc.conf 

注释掉 * soft nproc 1024

+ 修改limits.conf文件

vi /etc/security/limits.conf
添加 

> soft  nproc  655350

+ 在控制台执行(修改这个不需要重启)

> ulimit -u 655350

5.检查下是否生效，在控制台切到该用户下

> ulimit -u

### ssh

[add ssh key learn address](https://code.visualstudio.com/docs/remote/troubleshooting#_configuring-key-based-authentication)

```shell script
#db
ssh-keygen -t rsa -b 4096

# generat ssh key
ssh-copy-id root@192.168.4.214

```
### DNS

### NTP

NTP(network time protocol)

