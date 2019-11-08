---
title: big-data-environment set-up
date: 2019.11.06 15:35:34
tags:
  - big-data
---

## big-data environment

+ zookeeper(apache-zookeeper-3.5.6-bin)
+ hadoop(hadoop-3.2.1)
+ hbase(hbase-2.2.2) 
+ 运行于单机环境中
+ 安装地址：/home/chen/work/service/

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
dataDir=/home/chen/work/service/apache-zookeeper-3.5.6-bin/zookeeperdir/zookeeper-data
dataLogDir=/home/chen/work/service/apache-zookeeper-3.5.6-bin/zookeeperdir/logs

# the port at which the clients will connect
clientPort=2181

# 集群的配置
server.1=127.0.0.1:2888:3888

```

### start

> ./bin/zkServer.sh start

### stop

> ./bin/zkServer.sh stop

### client connection

> ./bin/zkCli.sh


## hbase support hadoop version

[hbase support hadoop version](http://hbase.apache.org/book.html#hadoop)



## hadoop(3.2.1/3.0.0)

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

### ssh localhost



### export 

#### sudo nano /etc/profile
```shell script
export HADOOP_HOME=/home/chen/work/service/hadoop-3.2.1

```

>source /etc/profile

#### sudo nano ~/.barshrc

```shell script
export HADOOP_HOME=/home/chen/work/service/hadoop-3.2.1
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
        <value>file:///home/chen/work/service/hadoop-3.2.1/hdfs/namenode-test</value>
    </property>
    <property>
        <name>dfs.data.dir</name>
        <value>file:///home/chen/work/service/hadoop-3.2.1/hdfs/datanode-test</value>
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
            <value>/home/chen/work/service/hadoop-3.2.1/local-dir</value>
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
     <property>
        <name>yarn.resourcemanager.hostname</name>
        <value>127.0.0.1</value>
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

   <!-- <property>
        <name>yarn.nodemanager.env-whitelist</name>
        <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
    </property> -->
    
            <!-- 这里设置主节点 -->

</configuration>
```

### start

./sbin/start-all.sh

### web url

http://localhost:8088/cluster

## hbase(2.2.2)

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
<!--hdfs://localhost:9000 is hadoop storage data address,you must config address in hadoop core-site.xml    -->
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

### error fix

####  Could not initialize class org.apache.hadoop.hbase.io.asyncfs.FanOutOneBlockAsyncDFSOutputHelper

+ error
```shell script
2019-01-16 15:26:44,501 ERROR [RS_OPEN_META-regionserver/shizhi002:16020-0] handler.OpenRegionHandler: Failed open of region=hbase:meta,,1.1588230740
java.lang.NoClassDefFoundError: Could not initialize class org.apache.hadoop.hbase.io.asyncfs.FanOutOneBlockAsyncDFSOutputHelper
	at org.apache.hadoop.hbase.io.asyncfs.AsyncFSOutputHelper.createOutput(AsyncFSOutputHelper.java:51)
	at org.apache.hadoop.hbase.regionserver.wal.AsyncProtobufLogWriter.initOutput(AsyncProtobufLogWriter.java:167)
	at org.apache.hadoop.hbase.regionserver.wal.AbstractProtobufLogWriter.init(AbstractProtobufLogWriter.java:166)
	at org.apache.hadoop.hbase.wal.AsyncFSWALProvider.createAsyncWriter(AsyncFSWALProvider.java:113)
	at org.apache.hadoop.hbase.regionserver.wal.AsyncFSWAL.createWriterInstance(AsyncFSWAL.java:612)
	at org.apache.hadoop.hbase.regionserver.wal.AsyncFSWAL.createWriterInstance(AsyncFSWAL.java:124)
	at org.apache.hadoop.hbase.regionserver.wal.AbstractFSWAL.rollWriter(AbstractFSWAL.java:756)
	at org.apache.hadoop.hbase.regionserver.wal.AbstractFSWAL.rollWriter(AbstractFSWAL.java:486)
	at org.apache.hadoop.hbase.regionserver.wal.AsyncFSWAL.<init>(AsyncFSWAL.java:251)
	at org.apache.hadoop.hbase.wal.AsyncFSWALProvider.createWAL(AsyncFSWALProvider.java:73)
	at org.apache.hadoop.hbase.wal.AsyncFSWALProvider.createWAL(AsyncFSWALProvider.java:48)
	at org.apache.hadoop.hbase.wal.AbstractFSWALProvider.getWAL(AbstractFSWALProvider.java:152)
	at org.apache.hadoop.hbase.wal.AbstractFSWALProvider.getWAL(AbstractFSWALProvider.java:60)
	at org.apache.hadoop.hbase.wal.WALFactory.getWAL(WALFactory.java:284)
	at org.apache.hadoop.hbase.regionserver.HRegionServer.getWAL(HRegionServer.java:2104)
	at org.apache.hadoop.hbase.regionserver.handler.OpenRegionHandler.openRegion(OpenRegionHandler.java:284)
	at org.apache.hadoop.hbase.regionserver.handler.OpenRegionHandler.process(OpenRegionHandler.java:108)
	at org.apache.hadoop.hbase.executor.EventHandler.run(EventHandler.java:104)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
	at java.lang.Thread.run(Thread.java:748)

```

+ fix

WAL(Write-Ahead-Log)是HBase的RegionServer在处理数据插入和删除的过程中用来记录操作内容的一种日志  
hbase.wal.provider=multiwal，支持的值还有defaultProvider和filesystem
```xml
 <property>
        <name>hbase.wal.provider</name>
        <value>filesystem</value>
    </property>

```
WAL的持久化的级别有如下几种：

SKIP_WAL：不写wal日志,这种可以较大提高写入的性能，但是会存在数据丢失的危险，只有在大批量写入的时候才使用(出错了可以重新运行)，其他情况不建议使用。
ASYNC_WAL：异步写入
SYNC_WAL：同步写入wal日志文件，保证数据写入了DataNode节点。
FSYNC_WAL: 目前不支持了，表现是与SYNC_WAL是一致的
USE_DEFAULT: 如果没有指定持久化级别，则默认为USE_DEFAULT, 这个为使用HBase全局默认级别(SYNC_WAL)

## start order

> hadoop -> zookeeper -> hbase
> zookeeper -> hadoop -> hbase

## jps 

> jps 
```shell script
6608 DataNode
6817 SecondaryNameNode
7939 HRegionServer
2019 org.eclipse.equinox.launcher_1.5.600.v20191014-2022.jar
7783 HMaster
6440 NameNode
7576 QuorumPeerMain #使用外部的zookeeper时,启用QuorumPeerMain，否则使用QuorumPeer
25401 Main
7305 NodeManager
7050 ResourceManager
8334 Main
8814 Jps

``` 





## kafka



## spark


## hive

###




