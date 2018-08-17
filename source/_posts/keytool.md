---
title: keytool
date: 2018.08.15 14:35:34
tags:
  - keytool
---

## 操作系统级别证书
   操作系统的级别证书，chrome所用的证书来自于本机操作系统证书

>window

    Internet管理内找到证书管理 
    
>linux

```
/etc/default/cacerts
```
    
## java环境变量内的证书
    
    因为java是基于jvm上的，由于其目标是跨平台应用，因此它不依赖于其操作系统，所以它保存了一份属于自己的证书
>window

    java管内找到证书管理 
    
>linux

```
/usr/lib/jvm/java-8-openjdk-amd64/jre/lib/security/cacerts

/etc/ssl/certs/java/cacerts #存放java所要的证书

```

### keytool的使用

>生成证书

```
#RSA 加密
#san: submitAilName 备用证书名,在导入系统级别的证书后，chrome使用时，若无该设备则会出错
sudo keytool -genkey -keystore chen.keystore -alias chen -keyalg RSA -storepass changeit -ext san=dns:cas.example.org
```

>导出证书

```
#-alias 与生成时的相同
sudo keytool -export -alias chen -file chen.cer -keystore chen.keystore
```

>导入证书到java环境

```
sudo keytool -import -alias chen -storepass changeit -file chen.cer -keystore "/usr/lib/jvm/java-8-openjdk-amd64/jre/lib/security/cacerts"
```

>删除java环境内的证书
```
sudo keytool -delete -alias chen -storepass changeit  -keystore "/usr/lib/jvm/java-8-openjdk-amd64/jre/lib/security/cacerts"
```

### 