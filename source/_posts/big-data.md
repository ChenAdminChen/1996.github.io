---
title: big-data-environment set-up
date: 2019.11.06 15:35:34
tags:
  - big-data
---

## zookeeper

[zookeeper 学习地址](http://zookeeper.apache.org/doc/current/zookeeperStarted.html)

### configuration

#### conf/zoo.cfg

```
tickTime=2000
# The number of ticks that the initial 
# synchronization phase can take
initLimit=10

# The number of ticks that can pass between 
# sending a request and getting an acknowledgement
syncLimit=5

# the directory where the snapshot is stored.
# do not use /tmp for storage, /tmp here is just 
# example sakes.
# dataDir=/tmp/zookeeper
dataDir=/home/chen/work/service/zookeeper-3.4.13/zookeeperdir/zookeeper-data
dataLogDir=/home/chen/work/service/zookeeper-3.4.13/zookeeperdir/logs

# the port at which the clients will connect
clientPort=2181

# 集群的配置
erver.1=127.0.0.1:2888:3888

```

### start

> ./bin/zkServer.sh start

### stop

> ./bin/zkServer.sh stop

### client connection

> ./bin/zkCli.sh


## hbase support hadoop version

[hbase support hadoop version](http://hbase.apache.org/book.html#hadoop)



## hadoop

[hadoop history version](https://hadoop.apache.org/old/)

~~hadoop 3.1.x and hadoop 3.2.x not use in hbase~~

~~This command that can check hadoop~~
~~> bin/hdfs getconf -backupNodes~~

~~hadoop can not use~~
~~```~~
~~HDFS_GETCONF_USER~~
~~0.0.0.0~~
~~```~~

~~hadoop can use~~
~~```~~
~~0.0.0.0~~
~~```~~

### export 

#### sudo nano /etc/profile
```shell script
export HADOOP_HOME=/home/chen/work/service/hadoop-3.1.2

```

>source /etc/profile

#### sudo nano ~/.barshrc

```shell script
export HADOOP_HOME=/home/chen/work/service/hadoop-3.1.2
```
> source ~/.barshrc

### etc/hadoop configuration

#### hadoop-env.sh

```shell script
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64

#export HDFS_NAMENODE_USER="root"

#export HDFS_DATANODE_USER="root"

#export HDFS_SECONDARYNAMENODE_USER="root"

#export YARN_RESOURCEMANAGER_USER="root"

#export YARN_NODEMANAGER_USER="root"

```

#### hdfs-site.xml

```xml
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.name.dir</name>
        <value>file:///home/chen/work/service/hadoop-3.1.2/hdfs/namenode-test</value>
    </property>
    <property>
        <name>dfs.data.dir</name>
        <value>file:///home/chen/work/service/hadoop-3.1.2/hdfs/datanode-test</value>
    </property>
</configuration>
```

./bin/hadoop namenode -format

#### core-site.xml

```xml
<configuration>
 <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```

#### mapred-site.xml

```xml

<configuration>
    <property>
        <name>mapred.job.tracker</name>
        <value>http://localhost:49001</value>
   </property>
 
    <property>
            <name>mapred.local.dir</name>
            <value>/home/chen/work/service/hadoop-3.1.2/local-dir</value>
    </property>

    <property>
            <name>mapreduce.framework.name</name>
            <value>yarn</value>
    </property>

     <!-- <property>
        <name>mapreduce.application.classpath</name>
        <value>$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*</value>
    </property> -->

</configuration>
```

#### yarn-site.xml

```xml
<configuration>

<!-- Site specific YARN configuration properties -->
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
        <!-- 这里设置主节点 -->
    <!-- <property>
        <name>yarn.resourcemanager.hostname</name>

        <value>127.0.0.1</value>
   </property>
   <property>

        <description>The address of the applications manager interface in the RM.</description>
        <name>yarn.resourcemanager.address</name>
        <value>${yarn.resourcemanager.hostname}:8032</value>
    </property> -->

     <!-- <property>
        <name>yarn.nodemanager.env-whitelist</name>
        <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
    </property> -->
    
            <!-- 这里设置主节点 -->
<!--
    <property>
            <name>yarn.resourcemanager.hostname</name>
            <value>node01</value>
    </property>
    <property>
            <description>The address of the applications manager interface in the RM.</description>
            <name>yarn.resourcemanager.address</name>
            <value>${yarn.resourcemanager.hostname}:8032</value>
    </property>
    <property>
            <description>The address of the scheduler interface.</description>
            <name>yarn.resourcemanager.scheduler.address</name>
            <value>${yarn.resourcemanager.hostname}:8030</value>
    </property>
    
    <property>
            <description>The https adddress of the RM web application.</description>
            <name>yarn.resourcemanager.webapp.https.address</name>
            <value>${yarn.resourcemanager.hostname}:8090</value>
    </property>
    <property>
            <name>yarn.resourcemanager.resource-tracker.address</name>
            <value>${yarn.resourcemanager.hostname}:8031</value>
    </property>
    <property>
            <description>The address of the RM admin interface.</description>
            <name>yarn.resourcemanager.admin.address</name>
            <value>${yarn.resourcemanager.hostname}:8033</value>
    </property>
    <property>
            <name>yarn.nodemanager.aux-services</name>
            <name>yarn.resourcemanager.webapp.address</name>
            <value>${yarn.resourcemanager.hostname}:8088</value>
    </property>
-->
</configuration>
```

### start

./sbin/start-all.sh

### web url

http://localhost:8088/cluster

## hbase

### configuration

#### hbase-env.sh

```shell script

<!-- # HBASE_MANAGES_ZK=false 表示启动的是独立的zookeeper,而配置成true则是hbase自带的zookeeper   -->
export HBASE_MANAGES_ZK=false

export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64

```

#### hbase-site.xml

```xml
<configuration>
  <property>
    <name>hbase.rootdir</name>
    <value>hdfs://localhost:9000/hbase</value>
  </property>

<!-- open web page -->
  <property>
    <name>hbase.master.info.port</name>
    <value>60010</value>
  </property>

  <!-- distributed = true , hbase use external zookeeper -->
  <property>
     <name>hbase.cluster.distributed</name>
     <value>true</value>
   </property>

   <!-- 分布式zookeeper configuration  <value>127.0.0.1,192.168.1.1,192.168.1.2</value>  -->
 <property>
     <name>hbase.zookeeper.quorum</name>
     <value>127.0.0.1</value>
  </property>

  <property>
    <name>hbase.zookeeper.property.dataDir</name>
    <value>/home/chen/work/service/hbase-2.2.2/hbase-zookeeper</value>
  </property>
  <property>
    <name>hbase.zookeeper.property.clientPort</name>
    <value>2181</value>
  </property>

  <property>
    <name>hbase.unsafe.stream.capability.enforce</name>
    <value>false</value>
    <description>
      Controls whether HBase will check for stream capabilities (hflush/hsync).

      Disable this if you intend to run on LocalFileSystem, denoted by a rootdir
      with the 'file://' scheme, but be mindful of the NOTE below.

      WARNING: Setting this to false blinds you to potential data loss and
      inconsistent system state in the event of process and/or node failures. If
      HBase is complaining of an inability to use hsync or hflush it's most
      likely not a false positive.
    </description>
  </property>
</configuration>
```

### start

> ./bin/start-hbase.sh

### stop 

> ./bin/stop-hbase.sh

### client connection

> ./bin/hbase shell

## start order

> hadoop -> zookeeper -> hbase

## kafka



## spark


## hive

###




