---
title: java command
date: 2018.06.12 14:36:45
reward: false
tags: 
    - java
---

### java程序启动远程调试功能

```bash

#开启java的远程调试功能，同时启动时不进行调试功能
-Xdebug -Xrunjdwp:transport=dt_socket,server=y,address=8000,suspend=n

```

### 

```bash
#java max mem default
#查看java的默认内存
#-Xmx is in MaxHeapSize, -Xms is in InitialHeapSize
java -XX:+PrintFlagsFinal -version

```