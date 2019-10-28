---
title: spring-boot-eureka
date: 2019/10/16 15:13:45
reward: false
tags: 
    - spring-boot
    - eureka
---

## spring-boot-eureka

### application.yml

```yaml
server:
  port: 8541
  
  #ssl
  ssl:
    enabled: true
    key-store: ff.jks
    key-alias: alias
    key-store-password: 123
    key-password: 123
    
eureka:
#  environment: product

  instance:
    #non-secure-port-enabled: false
#    secure-port-enabled: true
#    secure-port: 8541
    preferIpAddress: true #注册时使用IP地址而不是机器名
    #hostname: localhost
#    ip-address: 192.168.1.1
#    instance-id: 192.168.1.1

  client:
    registerWithEureka: false
#    fetchRegistry: true
    serviceUrl:
      defaultZone: http://localhost:${server.port}/eureka/
    
zuul:
  retryable: true
  routes:

    user:
      sensitiveHeaders:  #服务之间的调用将头部信息转发
      serviceId: user
      
    perm:
      sensitiveHeaders:
      serviceId: permission

    portal:
      path: /portal/**
      url: http://localhost:8181

```

### spring-boot-admin

### `spring-boot-sleuth` and `spring-boot-zipkin` and ` spring-boot-actuator`

&nbsp;&nbsp;Zipkin is a distributed tracing system. It helps gather timing data needed to troubleshoot latency problems in service architectures. Features include both the collection and lookup of this data.
    
[sleuth学习地址](https://howtodoinjava.com/spring-cloud/spring-cloud-zipkin-sleuth-tutorial/)  
[zipkin学习地址](https://zipkin.io/pages/quickstart.html 'zipkin')

#### 添加依赖
```gradle
    //actuator 监控和管理Spring Boot应用，比如健康检查、审计、统计和HTTP追踪 
     implementation("org.springframework.boot:spring-boot-starter-actuator")
    
    //Spring-Cloud-Sleuth是Spring Cloud的组成部分之一，为SpringCloud应用实现了一种分布式追踪解决方案，其兼容了Zipkin, HTrace和log-based追踪
    implementation("org.springframework.cloud:spring-cloud-starter-sleuth")
    implementation("org.springframework.cloud:spring-cloud-starter-zipkin")
    
    //基础的依赖
     implementation('org.springframework.boot:spring-boot-starter-web')
    
```

#### application.yml配置

```yaml

spring:       
# 提供application.name 方便查看日志信息
  application:
    name: permission
  zipkin:
    base-url: http://localhost:9441
#采样率  
  sleuth:  
    sampler.precentage: 1

# actuator 配置，配合sleuth使用
management:
  endpoints:
    web:
      exposure:
        include: '*'
  endpoint:
    health:
      show-details: always
```

#### 配置采样率

日志采集的越多，追踪越全面，但是消耗也越大；反之日志追踪不够多也就失去了意义。到底追踪多少日志才是平衡点？Spring Cloud Sleuth把这个问题交给了使用者，
通过配置spring.sleuth.sampler.percentage=0.1这个参数来决定了日志记录发送给采集器的概率，0-1交给使用者自己配置。开发阶段和运行初期，一般配置成1全量收集日志，到了上线后可以慢慢降低这一概率。
开发者还可以直接在代码中创建AlwaysSampler实例来指定100%输出日志，效果跟spring.sleuth.sampler.percentage=1是一样的。

```kotlin
@Bean
    fun alwaysSampler(): Sampler {
        return Sampler.ALWAYS_SAMPLE
    }
```
