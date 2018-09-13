---
title: java isInstance instanceof isAssignableFrom
date: 2018.1.15 20:36:45
reward: false
tags: 
    - java
    - isInstance
    - instanceof
    - isAssignableFrom
---

### isInstance

``` bash
B.class.isInstance(a)
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;动态等价，用于检查泛型，jdk中的CheckedMap里面用到这个检查Map里面的key、value类型是否和约定一样

#### instanceof

``` bash
class1 instanceof class
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;class1是class的实例，class是类的或者接口，父类或父接口，即B b = a成立

#### isAssignableFrom


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;两个class类型关系判断，判断B是不是A的子类或子接口
``` bash
a instanceof B
```

### test

``` bash
User: 用户基类
PublicUser extend User: PublicUser用户子类，继承User类

PublicUser prUser = new PublicUser();

#isintance
System.out.println(User.class.isintance prUser)       //true

System.out.println(prUser intanceof User)       //true

System.out.println(PublicUser.class.isAssignableFrom(User.class))       //true

System.out.println(User.class.isAssignableFrom(PublicUser.class))       //false
```