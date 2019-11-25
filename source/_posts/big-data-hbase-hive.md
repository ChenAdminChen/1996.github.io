---
title: big-data-hbase-hive
date: 2019.11.25 15:35:34
tags:
  - big-data
---

## hbase-hive

[learn address](https://cwiki.apache.org/confluence/display/Hive/HBaseintegration)

### hbase table information

> describe 'f'
```sql
Table f is ENABLED                                                                                                                                                                            
f                                                                                                                                                                                             
COLUMN FAMILIES DESCRIPTION                                                                                                                                                                   
{NAME => 'f', VERSIONS => '1', EVICT_BLOCKS_ON_CLOSE => 'false', NEW_VERSION_BEHAVIOR => 'false', KEEP_DELETED_CELLS => 'FALSE', CACHE_DATA_ON_WRITE => 'false', DATA_BLOCK_ENCODING => 'NONE'
, TTL => 'FOREVER', MIN_VERSIONS => '0', REPLICATION_SCOPE => '0', BLOOMFILTER => 'ROW', CACHE_INDEX_ON_WRITE => 'false', IN_MEMORY => 'false', CACHE_BLOOMS_ON_WRITE => 'false', PREFETCH_BLO
CKS_ON_OPEN => 'false', COMPRESSION => 'NONE', BLOCKCACHE => 'true', BLOCKSIZE => '65536'}                                                                                                    

```

> scan 'f'
```sql
ROW                                              COLUMN+CELL                                                                                                                                  
 row1                                            column=f:name, timestamp=1574480231593, value=chen                                                                                           
 row2                                            column=f:age, timestamp=1574660750499, value=11                                                                                              
 row3                                            column=f:name, timestamp=1574666237806, value=kevin                                                                                          
```

> put 
```
put 'f','row6','f:age',34
put 'f','row6','f:name','qi'
```

> scan 'f'

```
ROW                                              COLUMN+CELL                                                                                                                                  
 row1                                            column=f:name, timestamp=1574480231593, value=chen                                                                                           
 row2                                            column=f:age, timestamp=1574660750499, value=11                                                                                              
 row3                                            column=f:name, timestamp=1574666237806, value=kevin                                                                                          
 row4                                            column=f:age, timestamp=1574668813422, value=12                                                                                              
 row4                                            column=f:name, timestamp=1574668813422, value=test                                                                                           
 row5                                            column=f:age, timestamp=1574669091951, value=13                                                                                              
 row5                                            column=f:name, timestamp=1574669091951, value=li                                                                                             
 row6                                            column=f:age, timestamp=1574672582983, value=34                                                                                              
 row6                                            column=f:name, timestamp=1574672608454, value=qi                                                                                             
```

### hive 

hive table information from hbase

> create 'hive_f'
```sql

CREATE EXTERNAL TABLE hive_f(id string, value string,age int) 
STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
WITH SERDEPROPERTIES ("hbase.columns.mapping" = ":key,f:name,f:age")
TBLPROPERTIES("hbase.table.name" = "f", "hbase.mapred.output.outputtable" = "f");
```

> select * from hive_f
```sql
row1	chen	NULL
row2	NULL	11
row3	kevin	NULL
```

> insert into hive_f(id,value,age) values('row4','test',12)

+ error

```
Application Timeout (Remaining Time):	Unlimited
Diagnostics:	
Application application_1574645197988_0001 failed 2 times due to AM Container for appattempt_1574645197988_0001_000002 exited with exitCode: 1
Failing this attempt.Diagnostics: [2019-11-25 15:33:38.304]Exception from container-launch.
Container id: container_1574645197988_0001_02_000001
Exit code: 1
[2019-11-25 15:33:38.306]Container exited with a non-zero exit code 1. Error file: prelaunch.err.
Last 4096 bytes of prelaunch.err :
Last 4096 bytes of stderr :
错误: 找不到或无法加载主类 org.apache.hadoop.mapreduce.v2.app.MRAppMaster
[2019-11-25 15:33:38.307]Container exited with a non-zero exit code 1. Error file: prelaunch.err.
Last 4096 bytes of prelaunch.err :
Last 4096 bytes of stderr :
错误: 找不到或无法加载主类 org.apache.hadoop.mapreduce.v2.app.MRAppMaster
For more detailed output, check the application tracking page: http://localhost:8088/cluster/app/application_1574645197988_0001 Then click on links to logs of each attempt.
. Failing the application.
```

+ fix (configuration hadoop) 
   
~~+ get hadoop classpath~~
~~./bin/hadoop classpath~~
  
```
/home/chen/work/service/hadoop-3.0.0/etc/hadoop:/home/chen/work/service/hadoop-3.0.0/share/hadoop/
common/lib/*:/home/chen/work/service/hadoop-3.0.0/share/hadoop/common/*:/home/chen/work/service/hadoop-3.0.0
/share/hadoop/hdfs:/home/chen/work/service/hadoop-3.0.0/share/hadoop/hdfs/lib/*:/home/chen/work/service
/hadoop-3.0.0/share/hadoop/hdfs/*:/home/chen/work/service/hadoop-3.0.0/share/hadoop/mapreduce/*:
/home/chen/work/service/hadoop-3.0.0/share/hadoop/yarn:/home/chen/work/service/hadoop-3.0.0/
share/hadoop/yarn/lib/*:/home/chen/work/service/hadoop-3.0.0/share/hadoop/yarn/*
```

  + configuration hadoop mapred-site.xml
  
```xml
<property>
        <name>mapreduce.application.classpath</name>
        <value>$HADOOP_HOME/share/hadoop/mapreduce/*</value>
</property>
```

>hbase scan 'f'

```
ROW                                              COLUMN+CELL                                                                                                                                  
 row1                                            column=f:name, timestamp=1574480231593, value=chen                                                                                           
 row2                                            column=f:age, timestamp=1574660750499, value=11                                                                                              
 row3                                            column=f:name, timestamp=1574666237806, value=kevin                                                                                          
 row4                                            column=f:age, timestamp=1574668813422, value=12                                                                                              
 row4                                            column=f:name, timestamp=1574668813422, value=test                                                                                           
 row5                                            column=f:age, timestamp=1574669091951, value=13                                                                                              
 row5                                            column=f:name, timestamp=1574669091951, value=li                                                                                             
5 row(s)
```

