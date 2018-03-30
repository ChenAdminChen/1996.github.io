---
title: oauth2
date: 2018.2.28 17:36:45
reward: false
tags: 
    - oauth2
    - 未写完
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
