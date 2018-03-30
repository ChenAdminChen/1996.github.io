---
title: karaf database driver
date: 2018.1.29 16:36:45
reward: false
tags: 
    - karaf 
    - mysql
---

### 下载servicemix所需要的包
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database的数据源[学习地址](http://www.liquid-reality.de/display/liquid/2012/01/13/Apache+Karaf+Tutorial+Part+6+-+Database+Access "karaf database")
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;从官网下载最新的karaf容器，配置mysql数据源，基本操作如下：

``` bash

feature:repo-add mvn:org.ops4j.pax.jdbc/pax-jdbc-features/0.8.0/xml/features

feature:install transaction jndi pax-jdbc-h2 pax-jdbc-pool-dbcp2 pax-jdbc-config   #pax-jdbc-h2 是指你想连接那个数据库时，则下载相应的pax-jdbc-xx

service:list DataSourceFactory   # 用于查看该容器里配置的数据源

```

### features内配置的信息

``` bash
<features xmlns="http://karaf.apache.org/xmlns/features/v1.4.0" name="yfaf-features">
    <repository>mvn:org.apache.cxf.karaf/apache-cxf/3.1.7/xml/features</repository>
    <repository>mvn:org.apache.karaf.features/spring/4.0.8/xml/features</repository>

    <feature name="test-data" description="test : data module" version="${project.version}">
        <feature version="4.0.8" prerequisite="false" dependency="false">jndi</feature>
        <feature version="4.0.8" prerequisite="false" dependency="false">jdbc</feature>

        <feature version="1.3.1" prerequisite="false" dependency="false">transaction</feature>

        <feature version="0.9.0" prerequisite="false" dependency="false">pax-jdbc-mysql</feature>
        <feature version="0.9.0" prerequisite="false" dependency="false">pax-jdbc-pool-dbcp2</feature>

        # <feature version="3.2.17.RELEASE_1" prerequisite="false" dependency="false">spring-jdbc</feature>
        # <feature version="3.2.17.RELEASE_1" prerequisite="false" dependency="false">spring-tx</feature>

        # <bundle start-level="40">mvn:org.mybatis/mybatis/3.4.5</bundle>
        # <bundle start-level="40">mvn:org.mybatis/mybatis-spring/1.3.0</bundle>
        # <bundle start-level="40">mvn:org.mybatis.caches/mybatis-ehcache/1.1.0</bundle>

        # <bundle start-level="40">mvn:com.fasterxml.jackson.core/jackson-core/2.8.4</bundle>
        # <bundle start-level="40">mvn:com.fasterxml.jackson.core/jackson-annotations/2.8.4</bundle>
        # <bundle start-level="40">mvn:com.fasterxml.jackson.core/jackson-databind/2.8.4</bundle>
        # <bundle start-level="40">mvn:com.fasterxml.jackson.module/jackson-module-jaxb-annotations/2.8.4</bundle>

        # <bundle start-level="40">mvn:com.fasterxml.jackson.dataformat/jackson-dataformat-xml/2.8.4</bundle>

    </feature>

</features>

```

### 配置数据源信息

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;创建新的文件以.cfg为后缀名，如：org.ops4j.datasource-test.cfg,将其放入servicemix的etc文件夹下,文件内内容如下：

``` bash

osgi.jdbc.driver.name=mysql-pool-xa  #使用mysql数据库  mysql-pool-xa/h2-pool-xa
url=jdbc:mysql://localhost:3306/test?useSSL=false&useUnicode=true&characterEncoding=UTF-8&zeroDateTimeBehavior=round  #数据库连接地址
dataSourceName=test               #数据库名字
user=root
password=admin

```

### jndi

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;安装如上的包后，可以使用jndi对数据源进行操作

``` bash

jndi:alias
jndi:bind
jndi:contexts
jndi:create
jndi:delete
jndi:names
jndi:unbind

```

### jdbc

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;jdbc指令是对数据库本身的操作

``` bash
jdbc:ds-create
jdbc:ds-delete
jdbc:ds-factories
jdbc:ds-info
jdbc:ds-list  #查看数据idr
jdbc:execute
jdbc:query # jdbc:query sql ：对数据库表的操作
jdbc:tables

karaf@root> jdbc:query test select * from user
```