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
    
<!--web request hdfs,but not must  -->
    <property>
        <name>dfs.http.address</name>
        <value>0.0.0.0:50070</value>
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


test环境redis信息：
redis-cli -h 121.40.83.65 auth Yftestredis123

redis-cli -h 121.40.83.65 -p 6379 -a Yftestredis123

## kafka

## spark


## hive

## configuration 

hive-env.sh

```shell script
HADOOP_HOME=/home/chen/work/service/hadoop-3.0.0
HBASE_HOME=/home/chen/work/service/hbase-2.2.2

JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64

HIVE_HOME=/home/chen/work/service/apache-hive-3.1.2-bin

export HIVE_CONF_DIR=$HIVE_HOME/conf
export HIVE_AUX_JARS_PATH=$HBASE_HOME/lib
export CLASSPATH=$CLASSPATH:$JAVA_HOME/lib:$HADOOP_HOME/lib:$HIVE_HOME/lib

```

hive-site.xml
```xml
<configuration>
  <property>
    <name>javax.jdo.option.ConnectionURL</name>
    <value>jdbc:mysql://localhost:3306/hivedbH?createDatabaseIfNotExist=true</value>
    <description>JDBC connect string for a JDBC metastore</description>
    <!-- 如果 mysql 和 hive 在同一个服务器节点，那么请更改 hadoop02 为 localhost -->
  </property>
  <property>
    <name>javax.jdo.option.ConnectionDriverName</name>
    <value>com.mysql.jdbc.Driver</value>
    <description>Driver class name for a JDBC metastore</description>
  </property>
  <property>
    <name>javax.jdo.option.ConnectionUserName</name>
    <value>root</value>
    <description>username to use against metastore database</description>
  </property>
  <property>
    <name>javax.jdo.option.ConnectionPassword</name>
    <value>as1996</value>
    <description>password to use against metastore database</description>
  </property>
  <property>
    <name>datanucleus.autoCreateSchema</name>
    <value>true</value>
  </property>
  <property>
    <name>datanucleus.autoCreateTables</name>
    <value>true</value>
  </property>
  <property>
    <name>datanucleus.autoCreateColumns</name>
    <value>true</value>
  </property>

  <!-- 设置 hive仓库的HDFS上的位置 -->
  <property>
    <name>hive.metastore.warehouse.dir</name>
    <value>/home/chen/work/service/apache-hive-3.1.2-bin/hive/data</value>
    <description>location of default database for the warehouse</description>
  </property>

  <property>
    <name>hive.downloaded.resources.dir</name>
    <value>/home/chen/work/service/apache-hive-3.1.2-bin/hive/resources</value>
    <description>Temporary local directory for added resources in the remote file system.</description>
  </property>

  <!-- 修改日志位置 -->
  <property>
    <name>hive.exec.local.scratchdir</name>
    <value>/home/chen/work/service/apache-hive-3.1.2-bin/hive/HiveJobsLog</value>
    <description>Local scratch space for Hive jobs</description>
  </property>


  <property>
    <name>hive.downloaded.resources.dir</name>
    <value>/home/chen/work/service/apache-hive-3.1.2-bin/hive/ResourcesLog</value>
    <description>Temporary local directory for added resources in the remote file system.</description>
  </property>
  <property>
    <name>hive.querylog.location</name>
    <value>/home/chen/work/service/apache-hive-3.1.2-bin/hive/HiveRunLog</value>
    <description>Location of Hive run time structured log file</description>
  </property>
  <property>
    <name>hive.server2.logging.operation.log.location</name>
    <value>/home/chen/work/service/apache-hive-3.1.2-bin/hive/OpertitionLog</value>
    <description>Top level directory where operation tmp are stored if logging functionality is enabled</description>
  </property>

  <!-- 配置HWI接口 -->
  <property>
    <name>hive.hwi.war.file</name>
    <value>/home/chen/work/service/apache-hive-3.1.2-bin/lib/hive-hwi-2.1.1.jar</value>
    <description>This sets the path to the HWI war file, relative to ${HIVE_HOME}. </description>
  </property>
  <property>
    <name>hive.hwi.listen.host</name>
    <value>localhost</value>
    <description>This is the host address the Hive Web Interface will listen on</description>
  </property>
  <property>
    <name>hive.hwi.listen.port</name>
    <value>9999</value>
    <description>This is the port the Hive Web Interface will listen on</description>
  </property>
  <property>
    <name>hive.server2.thrift.bind.host</name>
    <value>localhost</value>
  </property>
  <property>
    <name>hive.server2.thrift.port</name>
    <value>10000</value>
  </property>
  <property>
    <name>hive.server2.thrift.http.port</name>
    <value>10001</value>
  </property>
  <property>
    <name>hive.server2.thrift.http.path</name>
    <value>cliservice</value>
  </property>

  <!-- HiveServer2的WEB UI -->
  <property>
    <name>hive.server2.webui.host</name>
    <value>localhost</value>
  </property>
  <property>
    <name>hive.server2.webui.port</name>
    <value>10002</value>
  </property>
  <property>
    <name>hive.scratch.dir.permission</name>
    <value>755</value>
  </property>
  <property>
    <name>hive.server2.enable.doAs</name>
    <value>false</value>
  </property>
  <property>
    <name>hive.auto.convert.join</name>
    <value>false</value>
  </property>
  <property>
    <name>spark.dynamicAllocation.enabled</name>
    <value>true</value>
    <description>动态分配资源</description>
  </property>

  <property>
    <name>hive.execution.engine</name>
    <value>spark</value>
    <description>这个值可以使mr/tez/spark中的一个作为hive的执行引擎，mr是默认值，而tez仅hadoop 2支持</description>
  </property>

  <property>
    <name>hive.enable.spark.execution.engine</name>
    <value>true</value>
  </property>

  <property>
    <name>spark.home</name>
    <value>/home/chen/work/service/spark-2.3.0-bin-hadoop2.7</value>
  </property>
  <property>
    <name>spark.master</name>
    <value>spark://127.0.0.1:7077</value>
    <!-- <value>yarn-client</value> -->
  </property>

  <property>
    <name>spark.serializer</name>
    <value>org.apache.spark.serializer.KryoSerializer</value>
  </property>
  <property>
    <name>spark.executor.memeory</name>
    <value>1g</value>
  </property>
  <property>
    <name>spark.driver.memeory</name>
    <value>1g</value>
  </property>
  <!-- <property>
  <name>spark.executor.extraJavaOptions</name>
  <value>-XX:+PrintGCDetails -Dkey=value -Dnumbers="one two three"</value>
</property> -->
  <!-- 使用Hive on spark时,若不设置下列该配置会出现内存溢出异常 -->
  <property>
    <name>spark.driver.extraJavaOptions</name>
    <value>-XX:PermSize=128M -XX:MaxPermSize=512M</value>
  </property>

  <property>
    <name>spark.submit.deployMode</name>
    <value>client</value>
  </property>
  <property>
    <name>spark.eventLog.enabled</name>
    <value>true</value>
  </property>
  <property>
    <name>spark.eventLog.dir</name>
    <value>hdfs://localhost:9000/spark-log</value>
  </property>


  <property>
    <name>hive.exec.reducers.bytes.per.reducer</name>
    <value>1024</value>
  </property>
  <property>
    <name>mapreduce.job.reduces</name>
    <value>10</value>
  </property>
  <property>
    <name> hive.exec.reducers.bytes.per.reducer</name>
    <value>10</value>
  </property>

  <property>
    <name>spark.yarn.jars</name>
    <value>hdfs://localhost:8020/spark-jars/*</value>
  </property>

</configuration>
```

>  拷贝JDBC包
```shell script
cp mysql-connector-java-5.1.19-bin.jar /hive-2.1.1/lib/

```
  
> 删除Hadoop之前的jline包，拷贝Hive中jline的扩展包
```shell script
cp /hive-2.1.1/lib/jline-2.12.jar /hadoop-3.0.0/share/hadoop/yarn/lib/
```
  
>拷贝tools.jar包
```shell script
cp /java8/lib/tools.jar /hive-2.1.1/lib/
```

> init matedata database
```shell script

./bin/schematool -dbType mysql -initSchema

```

## start hive
```shell script
./bin/hive
```

### start serverhive2
```shell script
./bin/hiveserver2

./bin/hiveserver2 --hiveconf hive.root.logger=INFO,console 
```

### jdbc connect hive
```shell script
./bin/beeline -u jdbc:hive2://localhost:10000/default
```

### error

+ error 1

```
java.lang.NoClassDefFoundError: org/apache/tez/dag/api/TezConfiguration
	at org.apache.hadoop.hive.ql.exec.tez.TezSessionPoolSession$AbstractTriggerValidator.startTriggerValidator(TezSessionPoolSession.java:74) ~[hive-exec-3.1.2.jar:3.1.2]
	at org.apache.hadoop.hive.ql.exec.tez.TezSessionPoolManager.initTriggers(TezSessionPoolManager.java:207) ~[hive-exec-3.1.2.jar:3.1.2]
	at org.apache.hadoop.hive.ql.exec.tez.TezSessionPoolManager.startPool(TezSessionPoolManager.java:114) ~[hive-exec-3.1.2.jar:3.1.2]
	at org.apache.hive.service.server.HiveServer2.initAndStartTezSessionPoolManager(HiveServer2.java:839) ~[hive-service-3.1.2.jar:3.1.2]
	at org.apache.hive.service.server.HiveServer2.startOrReconnectTezSessions(HiveServer2.java:822) ~[hive-service-3.1.2.jar:3.1.2]
	at org.apache.hive.service.server.HiveServer2.start(HiveServer2.java:745) ~[hive-service-3.1.2.jar:3.1.2]
	at org.apache.hive.service.server.HiveServer2.startHiveServer2(HiveServer2.java:1037) [hive-service-3.1.2.jar:3.1.2]
	at org.apache.hive.service.server.HiveServer2.access$1600(HiveServer2.java:140) [hive-service-3.1.2.jar:3.1.2]
	at org.apache.hive.service.server.HiveServer2$StartOptionExecutor.execute(HiveServer2.java:1305) [hive-service-3.1.2.jar:3.1.2]
	at org.apache.hive.service.server.HiveServer2.main(HiveServer2.java:1149) [hive-service-3.1.2.jar:3.1.2]
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[?:1.8.0_222]
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62) ~[?:1.8.0_222]
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) ~[?:1.8.0_222]
	at java.lang.reflect.Method.invoke(Method.java:498) ~[?:1.8.0_222]
	at org.apache.hadoop.util.RunJar.run(RunJar.java:239) [hadoop-common-2.8.5.jar:?]
	at org.apache.hadoop.util.RunJar.main(RunJar.java:153) [hadoop-common-2.8.5.jar:?]
Caused by: java.lang.ClassNotFoundException: org.apache.tez.dag.api.TezConfiguration
	at java.net.URLClassLoader.findClass(URLClassLoader.java:382) ~[?:1.8.0_222]
	at java.lang.ClassLoader.loadClass(ClassLoader.java:424) ~[?:1.8.0_222]
	at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:349) ~[?:1.8.0_222]
	at java.lang.ClassLoader.loadClass(ClassLoader.java:357) ~[?:1.8.0_222]
	... 16 more

```

> fix error
在hive-site.xml添加如下配置
```xml
<property>
    <name>hive.server2.active.passive.ha.enable</name>
    <value>true</value>
</property>
```

## spark


### 检查是否安装成功

```shell script
./bin/spark-submit --master spark://localhost:7077 --class org.apache.spark.examples.SparkPi ../examples/jars/spark-examples_2.11-2.1.1.jar 100
./bin/spark-submit --master spark://localhost:7077 --class org.apache.spark.examples.SparkPi /home/chen/work/service/spark-2.3.0-bin-hadoop2.7/examples/jars/spark-examples_2.11-2.1.1.jar 100

```





