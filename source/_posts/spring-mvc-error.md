---
title: spring mvc error
date: 2018.3.14 10:50:45
tags: 
    - spring mvc
    - error
---

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1).java.lang.ClassNotFoundException:org.springframework.web.servlet.DispatcherServlet当tomact用maven导包后，需要在生成war后到资源里去查看WEB-INF中的lib中有没有包

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2).java.lang.NoClassDefFoundError: javax/servlet/jsp/jstl/core/Config当出现错误时：证明你在使用jsp页面时，并没有导入jsp相关的包

``` bash
<!-- jsp标准标签库 -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>jstl</artifactId>
        <version>1.2</version>
    </dependency>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.16</version>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>1.6.1</version>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-nop</artifactId>
        <version>1.6.4</version>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
```
