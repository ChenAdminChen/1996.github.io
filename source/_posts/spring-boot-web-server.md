---
title: spring boot web server
date: 2018/02/25 16:53:56
reward: false
tags: 
    - spring-boot
    - web
    
---

### 

``` bash
<iq type='set' from='00000032@im.com/swm-mac' to='pubsub.im.com' id='create1'>
    <pubsub xmlns='http://jabber.org/protocol/pubsub'>
        <create node='story tell'/> # story tell 为节点名称
    </pubsub>
</iq>
```

### 订阅节点

``` bash
<iq type='set' from='00000010@im.com' to='pubsub.im.com' id='sub1'>
    <pubsub xmlns='http://jabber.org/protocol/pubsub'>
        <subscribe node='story tell' jid='00000010@im.com'/>  #订阅story tell节点
    </pubsub>
</iq>
```

### 所有者查看订阅人员

``` bash
<iq type='get' from='12345677@im.com/elsinore' to='pubsub.im.com' id='subman1'>
    <pubsub xmlns='http://jabber.org/protocol/pubsub#owner'>
        <subscriptions node='story tell'/>
    </pubsub>
</iq>
```

### 查询某个pubsub的订阅者

``` bash
<iq type='get' from='00000011@im.com' to='pubsub.im.com' id='subscriptions2'>
  <pubsub xmlns='http://jabber.org/protocol/pubsub'>
    <subscriptions node='story tell'/>
  </pubsub>
</iq>
```

### 向节点内发送消息

``` bash
<iq type ="set" from ="00000010@im.com" to="pubsub.im.com" id="publish1">
    <pubsub xmlns ="http://jabber.org/protocol/pubsub">
        <publish node ="story tell">
            <item id="bnd81g37d61f49fgn581">
                <presence xmlns='http://yfaf.com/xmpp-ext/persence' jid="00000010@im.com">20</presence>
            </item>
        </publish>
    </pubsub>
</iq>
```

### 删除订阅的节点

``` bash
<iq type ='set'from ='12345678_yf.com@im.com/elsinore' to ='pubsub.im.com' id ='delete1 '>
    <pubsub xmlns ='http://jabber.org/protocol/pubsub#owner'>
        <delete node ='story tell'/ >
    </ pubsub >
</iq >
```
