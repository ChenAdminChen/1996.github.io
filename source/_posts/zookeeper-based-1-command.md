---
title: zookeeper
date: 2018.09.13 14:36:45
reward: false
tags:
    - zookeeper
---

## zookeeper server start

```
#start server
chen@chen:~/work/service/zookeeper-3.4.13/bin$ ./kafka-server-start.sh ../config/server.properties

#start client
chen@chen:~/work/service/zookeeper-3.4.13/bin$ ./zkCli.sh -server 127.0.0.1:2181
```

## zookeeper command

```
ls /

#顺序节点
create -s test

#临时节点
create -e test

#持久节点
create test 

rmr test

```