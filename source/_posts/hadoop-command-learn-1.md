---
title: hadoop
date: 2019.10.10 14:16:45
reward: false
tags:
    - hadoop
    - install
    - map
    - mapReduce
    - yarn
---

## hadoop environment setup

### ssh authorization

. set use key login linux command
```
ssh-keygen -t rsa 
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys 
chmod 0600 ~/.ssh/authorized_keys 
```

. check key login is true  
    use `ssh localhost` can login linux that is not input password  

### config JAVA_HOME

you must config `JAVA_HOME` environment

> sudo nano ~/.bashrc
```
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```

> source ~/.bashrc

>echo $JAVA_HOME
```
/usr/lib/jvm/java-8-openjdk-amd64
```

### hadoop start

```
YangTianT4900c-00:~/work/service/hadoop-3.2.1$ ./sbin/start-yarn.sh
YangTianT4900c-00:~/work/service/hadoop-3.2.1$ ./sbin/stop-yarn.sh
```

## hadoop command 

-put
```
YangTianT4900c-00:~/work/service/hadoop-3.2.1$ ./bin/hadoop fs -put LICENSE.txt input
```

-ls
```
YangTianT4900c-00:~/work/service/hadoop-3.2.1$ ./bin/hadoop fs -ls hdfsData
```

- wordcount

. `wordcount` can count word numbers
```
YangTianT4900c-00:~/work/service/hadoop-3.2.1$ ./bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.1.jar wordcount hdfsData/LICENSE.txt output
```
. show result
```
YangTianT4900c-00:~/work/service/hadoop-3.2.1$ cat output/part-r-00000
```

## yarn

## map

## mapReduce


