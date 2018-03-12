---
title: oauth2
date: 2018.2.28 17:36:45
reward: false
tags: 
    - oauth2
---

### oauth2 four roles

#### resource user

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;能够对受保护的资源进行授权的实体。当resource owner是人的时候，它是指终端用户

#### resource server

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;持有受保护的资源的服务器，使用access tokens能够接收和响应对受保护资源的请求

#### client

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在resource owner的行为和授权下，请求受保护资源的应用。“client”没有特指任何实现特性（比如，无论应用执行在服务器，桌面或者其他设备上）

#### authorization server

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当成功认证了resource owner并且获得授权后，分发access tokens给client的服务器。

#### 同步调用

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;同步调用是最简单也是最基本的一种调用方式，类A中的方法a()调用类B中的方法b()，等b()执行完成后再回到a()执行，该方式只适合执行完b()方法所需要的时间不长的情况，若b()执行所需要的时间过长时，a()方法处于等待状态，整个流程将会造成阻塞。

#### 异步调用

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;异步调用是为解决同步调用时造成的阻塞，类A中的方法a()通过启用一个线程调用类B中的方法b(),其a()方法的代码继续执行，无论b()方法执行多长时间都不会影响a()的执行，则解决了b()方法执行过程则造成的阻塞情况。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;异步调用中，a()方法不等待b()执行完，若a()方法中需要b()执行后返回的结果继续往下走，因此需要在a()内对b()方法执行的结果进行监听，a()获得结果后对其做出相应的操作。在java可以使用Futrue + Callable做到。

#### 回调

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;回调是指：类A的a()方法调用类B的b()方法，类B的b()方法执行完毕后主动调用类A的c()，方法其c()称为callback方法

### java callBack实现

实现两个整数相加简单的回调方法

#### 回调类接口

``` bash
oic==0.7.6
pyjwkest==1.0.1
cherrypy==3.2.4
pyaml==15.03.1

```