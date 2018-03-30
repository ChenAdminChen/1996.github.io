---
title: maven
date: 2018.3.14 11:36:45
reward: false
tags: 
    - maven
---

### maven指令

``` bash

# 这个命令是在install时跳过test的类与javadoc的语句在run中设置
-Dmaven.test.skip -Dmaven.javadoc.skip install

#查看maven版本信息
mvn -v
    test：测试
    package：打包
    compile:编译
    clear:清除targer
    install:下载所需要的jar到本地仓库

```

### maven环境变量名M2_HOME与MAVEN_HOME的区别

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[M2_HOME的地址](http://blog.csdn.net/zhouhuakang/article/details/50611444 'M2_HOME')
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[MAVEN_HOME的地址](https://my.oschina.net/anyyang/blog/686893 'MAVEN_HOME')