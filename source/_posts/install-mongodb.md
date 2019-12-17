---
title: mongodb 
date: 2019.12.16 17:35:34
tags:
  - mongodb
---

## install mongodb

[install address](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-red-hat/)

### configuration

> /etc/mongodb.conf
```shell script
# mongod.conf

# for documentation of all options, see:
#   http://docs.mongodb.org/manual/reference/configuration-options/

# where to write logging data.
systemLog:
  destination: file
  logAppend: true
  path: /var/log/mongodb/mongod.log

# Where and how to store data.
storage:
  dbPath: /var/lib/mongo
  journal:
    enabled: true
#  engine:
#  wiredTiger:

# how the process runs
processManagement:
  fork: true  # fork and run in background
  pidFilePath: /var/run/mongodb/mongod.pid  # location of pidfile
  timeZoneInfo: /usr/share/zoneinfo

# network interfaces
net:
  port: 27017
  #bindIp: 127.0.0.1
  bindIp: 0.0.0.0  # Enter 0.0.0.0,:: to bind to all IPv4 and IPv6 addresses or, alternatively, use the net.bindIpAll setting.

#security:
#   authorization: 'enabled'
#operationProfiling:

#replication:

#sharding:

## Enterprise-Only Options

#auditLog:

#snmp:

```

> mongodb install address

```shell script
>whereis mongo
```

> start mongodb

```shell script
sudo service mongod start

systemctl start mongod.service

```

> stop mongodb

```shell script
sudo service mongod stop

systemctl stop mongod.service

```

> mongodb listener

```shell script
netstat -anp |grep 27017
```

## mongodb client
[mongodb client download](https://robomongo.org/)
