---
title: cas server set up
date: 2018.02.28 12:36:45
reward: false
tags: 
    - cas
    - openid
    - 单点登录
---

### cas的学习

#### 下载基于5.2x版本的cas服务器

&nbsp;&nbsp;&nbsp;&nbsp;[下载地址](https://github.com/apereo/cas-overlay-template "github地址")

#### cas.properties文件配置

&nbsp;&nbsp;&nbsp;&nbsp;在项目的cas-overlay-template\etc\cas\config文件下需要配置cas.properties内相关的配置内容。<br/>
&nbsp;&nbsp;&nbsp;&nbsp;以下是相关配置文件

``` bash
#server config
cas.server.name: https://localhost
cas.server.prefix: https://localhost/cas

#logger，可在log4j2.xml中设置logger lever
logging.config: file:/etc/cas/config/log4j2.xml

#adminPagesSecurity
cas.adminPagesSecurity.ip=127\.0\.0\.1
cas.adminPagesSecurity.adminRoles[0]=ROLE_ADMIN
cas.adminPagesSecurity.actuatorEndpointsEnabled=true

#openid connect Authentication
cas.authn.oidc.issuer=https://localhost/cas/oidc
cas.authn.oidc.jwksFile=file:/etc/cas/keystore.jwks
cas.authn.oidc.dynamicClientRegistrationMode=OPEN
cas.authn.oidc.scopes=openid,profile,email,address,phone,offline_access
cas.authn.oidc.claims=sub,name,preferred_username,family_name,given_name,middle_name,given_name,profile,picture,nickname,website,zoneinfo,locale,updated_at,birthdate,email,email_verified,phone_number,phone_number_verified,address
cas.authn.oidc.claimsMap.given_name=email
cas.authn.oidc.claimsMap.phone_number=phone

#accept.authn.users=casuser::Mellon,ROLE_ADMIN
#用户名：casuser
#密码：Mellon
#取消有静态登录的警告
cas.authn.accept.users=

#service
cas.serviceRegistry.watcherEnabled=true
cas.serviceRegistry.config.location=classpath:/services
cas.serviceRegistry.initFromJson=true

# mysql database
cas.serviceRegistry.jpa.ddlAuto=create-drop
cas.serviceRegistry.jpa.user=root
cas.serviceRegistry.jpa.password=xxx
cas.serviceRegistry.jpa.driverClass=com.mysql.jdbc.Driver
cas.serviceRegistry.jpa.url=jdbc:mysql://127.0.0.1:3306/cas?useUnicode=true&amp;characterEncoding=UTF-8&amp;useTimezone=true&amp;serverTimezone=GMT%2B8:00&amp;zeroDateTimeBehavior=round
cas.serviceRegistry.jpa.dialect=org.hibernate.dialect.HSQLDialect
cas.serviceRegistry.jpa.autocommit=true

# login databases configure
cas.authn.jdbc.query[0].user=root
cas.authn.jdbc.query[0].password=xxx
cas.authn.jdbc.query[0].driverClass=com.mysql.jdbc.Driver
cas.authn.jdbc.query[0].url=jdbc:mysql://127.0.0.1:3306/yfaf?useUnicode=true&amp;characterEncoding=UTF-8&amp;useTimezone=true&amp;serverTimezone=GMT%2B8:00&amp;zeroDateTimeBehavior=round
cas.authn.jdbc.query[0].dialect=org.hibernate.dialect.MySQL5Dialect

#login certification

cas.authn.jdbc.query[0].sql=SELECT LOWER(password) as password, deleted, account, name, nickname, date, phone, email, sex as gender, im_user_id as im, safety_id as safe_aid, com_id as company FROM user WHERE account=?
cas.authn.jdbc.query[0].fieldPassword=password
cas.authn.jdbc.query[0].passwordEncoder.type=DEFAULT
cas.authn.jdbc.query[0].passwordEncoder.encodingAlgorithm=MD5
cas.authn.jdbc.query[0].passwordEncoder.characterEncoding=UTF-8

#ticket configure
cas.ticket.registry.inMemory.cache=false
cas.ticket.registry.jpa.ddlAuto=create-drop
cas.ticket.registry.jpa.user=root
cas.ticket.registry.jpa.password=as1996
cas.ticket.registry.jpa.driverClass=com.mysql.jdbc.Driver
cas.ticket.registry.jpa.url=jdbc:mysql://chen-PC:3306/cas?useUnicode=true&amp;characterEncoding=UTF-8&amp;useTimezone=true&amp;serverTimezone=GMT%2B8:00&amp;zeroDateTimeBehavior=round

#用作https://localhost/cas/status/attrresolution页面查询
cas.authn.attributeRepository.jdbc[0].user=root
cas.authn.attributeRepository.jdbc[0].password=as1996
cas.authn.attributeRepository.jdbc[0].driverClass=com.mysql.jdbc.Driver
cas.authn.attributeRepository.jdbc[0].url=jdbc:mysql://chen-PC:3306/yfaf?useUnicode=true&amp;characterEncoding=UTF-8&amp;useTimezone=true&amp;serverTimezone=GMT%2B8:00&amp;zeroDateTimeBehavior=round

cas.authn.attributeRepository.jdbc[0].singleRow=true
cas.authn.attributeRepository.jdbc[0].requireAllAttributes=true
cas.authn.attributeRepository.jdbc[0].sql=SELECT account, name, nickname, date, phone, email, sex as gender, im_user_id as im, safety_id as safe_aid, com_id as company FROM user WHERE account={0}
cas.authn.attributeRepository.jdbc[0].username=account

#用作oidc查询映射
cas.authn.attributeRepository.defaultAttributesToRelease=name,account
#,nickname,date,phone,email,gender,im,safe_aid,company

#service注册所在目录
cas.serviceRegistry.config.location: classpath:/services

#将cas页面中查看 sso session/ register service page 
cas.monitor.endpoints.enabled=true   
cas.monitor.endpoints.sensitive=false

cas.monitor.endpoints.dashboard.enabled=true
cas.monitor.endpoints.dashboard.sensitive=false

cas.monitor.endpoints.status.enabled=true   
cas.monitor.endpoints.status.sensitive=false

#开启 jwt token
cas.authn.token.crypto.encryptionEnabled=true
cas.authn.token.crypto.signingEnabled=true

#jwt key,若key，未设置，则在启动时会自动生成下面的key
cas.authn.token.crypto.encryption.key=xPZkj_A8RXlQfvD8EwmT7mU2TseRrcWHjM8q3R1AGik
cas.authn.token.crypto.signing.key=PKH_BNwqiUDkMey108ix1fTWktUGXkALNBqoyE1NBT_RABe1WPY42FAqJOwCltLAa6duzvANst-Kxx7lYctrrg

cas.tgc.crypto.encryption.key=kJbeEjRlDRQCixHWwn2WpjiXAwXzan8h_o-jCv89k90
cas.tgc.crypto.signing.key=pR0GCaP_10pxitFBh2I2KNEGrDF9vb6HodyyV2u5u-3DkQiXd6GcSACeDfWxVsfS345ao99cIvF8ox4y_kjdSw

cas.webflow.crypto.signing.key=TnK9AvAkwAbvFLIIZgtoT1K3YiGBWoebDIWC_8bozIfT2XKFGxQxochwa-d4Q_2WRwYwDv4Kw_d930MwywZHeg
cas.webflow.crypto.encryption.key=9fuRE_lJHtFmCVJUi8K3xQ

```

#### service.json

```
{
  "@class" : "org.apereo.cas.services.RegexRegisteredService",
  "serviceId" : "^https?://.*", //所注册的服务地址
  "name" : "CAS Spring Secured App",
  "description": "This is a Spring App that usses the CAS Server for it's authentication",
  "id" : 19991,
  "evaluationOrder" : 1,
  //代理服务器地址
  "proxyPolicy" : {
    "@class" : "org.apereo.cas.services.RegexMatchingRegisteredServiceProxyPolicy",
    "pattern" : "^https?://.*"
  },
  //设置 生成 jwt时所要的key，该key来自于生成的cer中，则必需使用与cas服务器中相同的证书
  "publicKey" : {
      "@class" : "org.apereo.cas.services.RegisteredServicePublicKeyImpl",
      "location" : "file:/etc/cas/chen.cer",
      "algorithm" : "RSA" //证书生成时所使用的加密方式
  }，
  //使用jwt时才使用
  "properties" : {
   "@class" : "java.util.HashMap",
    "jwtAsServiceTicket" : {
      "@class" : "org.apereo.cas.services.DefaultRegisteredServiceProperty",
      "values" : [ "java.util.HashSet", [ "true" ] ]
     },
    "jwtAsServiceTicketSigningKey" : {
       "@class" : "org.apereo.cas.services.DefaultRegisteredServiceProperty",
       "values" : [ "java.util.HashSet", [ "ONOqVlFFievk5lo2Yz84S8J41DTrAJDbt4MnB2ZpniKAlzHitlL12xJ1VYxB8GpB" ] ]  //key一定为64字节，在jwtAsServiceTicketEncryptionKey加密才使用该加密
      },
     "jwtAsServiceTicketEncryptionKey" : {
         "@class" : "org.apereo.cas.services.DefaultRegisteredServiceProperty",
         "values" : [ "java.util.HashSet", [ "afkeykeykeykyekyefdafsadfsdafdafdafsadfsafa" ] ]  //key一定为256位 ，先使用该key 加密
      },
      "jwtSecretsAreBase64Encoded": {
          "@class": "org.apereo.cas.services.DefaultRegisteredServiceProperty",
          "values": [ "java.util.HashSet", [ "true" ] ]
        }
    },
    //开启代理服务
  "attributeReleasePolicy" : {
    "@class" : "org.apereo.cas.services.ReturnAllowedAttributeReleasePolicy",
    "authorizedToReleaseCredentialPassword" : true
  }
  
}

```

#### 配置pom.xml

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在cas-overlay-template项目下存在一个pom.xml文件，需要添加依赖包后，才会开启相应的功能，否则只存在一部分基础功能

``` bash
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd ">
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.apereo.cas</groupId>
    <artifactId>cas-overlay</artifactId>
    <packaging>war</packaging>
    <version>1.0</version>

    <build>
        <plugins>
            <plugin>
                <groupId>com.rimerosolutions.maven.plugins</groupId>
                <artifactId>wrapper-maven-plugin</artifactId>
                <version>0.0.4</version>
                <configuration>
                    <verifyDownload>true</verifyDownload>
                    <checksumAlgorithm>MD5</checksumAlgorithm>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>${springboot.version}</version>
                <configuration>
                    <mainClass>${mainClassName}</mainClass>
                    <addResources>true</addResources>
                    <executable>${isExecutable}</executable>
                    <layout>WAR</layout>
                </configuration>
                <executions>
                    <execution>
                        <goals>
                            <goal>repackage</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-war-plugin</artifactId>
                <version>2.6</version>
                <configuration>
                    <warName>cas</warName>
                    <failOnMissingWebXml>false</failOnMissingWebXml>
                    <recompressZippedFiles>false</recompressZippedFiles>
                    <archive>
                        <compress>false</compress>
                        <manifestFile>${manifestFileToUse}</manifestFile>
                    </archive>
                    <overlays>
                        <overlay>
                            <groupId>org.apereo.cas</groupId>
                            <artifactId>cas-server-webapp${app.server}</artifactId>
                        </overlay>
                    </overlays>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.3</version>
            </plugin>
        </plugins>
        <finalName>cas</finalName>
    </build>

    <dependencies>
        <dependency>
            <groupId>org.apereo.cas</groupId>
            <artifactId>cas-server-webapp${app.server}</artifactId>
            <version>${cas.version}</version>
            <type>war</type>
            <scope>runtime</scope>
        </dependency>

        <dependency>
            <groupId>org.apereo.cas</groupId>
            <artifactId>cas-server-support-rest</artifactId>
            <version>${cas.version}</version>
        </dependency>

        <dependency>
            <groupId>org.apereo.cas</groupId>
            <artifactId>cas-server-support-rest-tokens</artifactId>
            <version>${cas.version}</version>
        </dependency>
        <dependency>
            <groupId>org.apereo.cas</groupId>
            <artifactId>cas-server-support-token-tickets</artifactId>
            <version>${cas.version}</version>
        </dependency>

        <dependency>
            <groupId>org.apereo.cas</groupId>
            <artifactId>cas-server-support-jdbc</artifactId>
            <version>${cas.version}</version>
        </dependency>

        <dependency>
           <groupId>org.apereo.cas</groupId>
           <artifactId>cas-server-support-jdbc-drivers</artifactId>
           <version>${cas.version}</version>
        </dependency>

        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-entitymanager</artifactId>
            <version>5.2.11.Final</version>
        </dependency>

       <!-- <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.30</version>
        </dependency> -->

        <dependency>
            <groupId>org.apereo.cas</groupId>
            <artifactId>cas-server-support-jdbc-authentication</artifactId>
            <version>${cas.version}</version>
        </dependency>

        #此处也需要添加,无法打包时可删除
        <!--<dependency>
            <groupId>org.jasig.cas</groupId>
            <artifactId>cas-server-support-jpa-ticket-registry</artifactId>
            <version>${cas.version}</version>
        </dependency> -->

        <dependency>
            <groupId>org.apereo.cas</groupId>
            <artifactId>cas-server-support-oidc</artifactId>
            <version>${cas.version}</version>
        </dependency>

        <dependency>
            <groupId>org.apereo.cas</groupId>
            <artifactId>cas-server-core-authentication</artifactId>
            <version>${cas.version}</version>
        </dependency>

        <dependency>
            <groupId>org.apereo.cas</groupId>
            <artifactId>cas-server-support-json-service-registry</artifactId>
            <version>${cas.version}</version>
        </dependency>

    </dependencies>

    <properties>
        <cas.version>5.2.1</cas.version>
        <springboot.version>1.5.8.RELEASE</springboot.version>
        <app.server>-tomcat</app.server> 

        <mainClassName>org.springframework.boot.loader.WarLauncher</mainClassName>
        <isExecutable>false</isExecutable>
        <manifestFileToUse>${project.build.directory}/war/work/org.apereo.cas/cas-server-webapp${app.server}/META-INF/MANIFEST.MF</manifestFileToUse>

        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <repositories>
        <repository>
            <id>sonatype-releases</id>
            <url>http://oss.sonatype.org/content/repositories/releases/</url>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
            <releases>
                <enabled>true</enabled>
            </releases>
        </repository>
        <repository>
            <id>sonatype-snapshots</id>
            <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
            <releases>
                <enabled>false</enabled>
            </releases>
        </repository>
        <repository>
            <id>shibboleth-releases</id>
            <url>https://build.shibboleth.net/nexus/content/repositories/releases</url>
        </repository>
    </repositories>

    <profiles>
        <profile>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
            <id>exec</id>
            <properties>
                <mainClassName>org.apereo.cas.web.CasWebApplication</mainClassName>
                <isExecutable>true</isExecutable>
                <manifestFileToUse></manifestFileToUse>
            </properties>
            <build>
                <plugins>
                    <plugin>
                        <groupId>com.soebes.maven.plugins</groupId>
                        <artifactId>echo-maven-plugin</artifactId>
                        <version>0.3.0</version>
                        <executions>
                            <execution>
                                <phase>prepare-package</phase>
                                <goals>
                                    <goal>echo</goal>
                                </goals>
                            </execution>
                        </executions>
                        <configuration>
                            <echos>
                            <echo>Executable profile to make the generated CAS web application executable.</echo></echos>
                        </configuration>
                    </plugin>
                </plugins>
            </build>
        </profile>

        <profile>
            <activation>
                <activeByDefault>false</activeByDefault>
            </activation>
            <id>pgp</id>
            <build>
                <plugins>
                    <plugin>
                        <groupId>com.github.s4u.plugins</groupId>
                        <artifactId>pgpverify-maven-plugin</artifactId>
                        <version>1.1.0</version>
                        <executions>
                            <execution>
                                <goals>
                                    <goal>check</goal>
                                </goals>
                            </execution>
                        </executions>
                        <configuration>
                            <pgpKeyServer>hkp://pool.sks-keyservers.net</pgpKeyServer>
                            <pgpKeysCachePath>${settings.localRepository}/pgpkeys-cache</pgpKeysCachePath>
                            <scope>test</scope>
                            <verifyPomFiles>true</verifyPomFiles>
                            <failNoSignature>false</failNoSignature>
                        </configuration>
                    </plugin>
                </plugins>
            </build>
        </profile>
    </profiles>
</project>

```
### SSL认证

#### 生成私钥
&nbsp;&nbsp;&nbsp;&nbsp;利用jdk自身的keytool生成相应的数字一个证书,生成证书到D:\localhost.keystore

``` bash
#使用JDK的keytool命令，生成证书（包含证书/公钥/私钥）到D:\localhost.keystore：

#-ext san（subjectAltName）: 备用域名,在新的规范内代替cn，可映射到多个域名，也可指向ip,在chrome中需要设置
    
#cn 域名指向

D:>keytool -genkey -keystore "D:\localhost.keystore" -alias localhost -keyalg RSA -storepass changeit -ext san=dns:cas.example.org,ip:127.0.01

输入密钥库口令:

再次输入新口令:

您的名字与姓氏是什么?
  [Unknown]:  localhost
您的组织单位名称是什么?
  [Unknown]:  sishuok.com
您的组织名称是什么?
  [Unknown]:  sishuok.com
您所在的城市或区域名称是什么?
  [Unknown]:  beijing
您所在的省/市/自治区名称是什么?
  [Unknown]:  beijing
该单位的双字母国家/地区代码是什么?
  [Unknown]:  cn
CN=localhost, OU=sishuok.com, O=sishuok.com, L=beijing, ST=beijing, C=cn是否正确
?
  [否]:  y

输入 <localhost> 的密钥口令
        (如果和密钥库口令相同, 按回车):
再次输入新口令:

#通过如上步骤，生成证书到D:\ localhost.keystore；

```

#### 生成公钥

&nbsp;&nbsp;&nbsp;&nbsp;首先使用localhost.keystore导出数字证书（公钥）到D:\localhost.cer

```bash

#-alias 与生成时的名字要相同

D:>keytool -export -alias localhost -file D:\localhost.cer -keystore D:\localhost.keystore

```

#### 将其配置到jdk内

&nbsp;&nbsp;&nbsp;&nbsp;因为CAS client需要使用该证书进行验证，需要将证书导入到JDK中

```bash
#需要进入jdk安装路径的security文件内
D:> cd D:\jdk1.7.0_21\jre\lib\security
keytool -import -alias localhost -file D:\localhost.cer -noprompt -trustcacerts -storetype jks -keystore cacerts -storepass changeit

#若导入出现错误时,可将security目录下的cacerts删掉,再次导入
```

## 打包运行

&nbsp;&nbsp;&nbsp;&nbsp;打包成功后会,项目内会出现target文件内存在cas.war的包

``` bash
#将etc目录下的所有文件都复制到项目所在的根目下面
build.cmd copy

#自动生成证书
build.cmd gencert

#cmd进入项目文件下按github上面的指令进行打包
D:>build.cmd package -U       # -U是清除后打包

#打包后，再运行
build.cmd run

#直接运行war,需要进入到cas.war所在的目录下
java -jar cas.war

#可查看build.cmd/build.sh下自己写入启动方法
```

## 下面的步骤不需要使用

#### 将cas.war放入tomact容器 

&nbsp;&nbsp;&nbsp;&nbsp;将cas.war放入D:\apache-tomcat-8.0.30\webapps内

#### 与tomact同级创建ect文件

&nbsp;&nbsp;&nbsp;&nbsp;将项目中的ect文件全部复制到与tomact同级的磁盘存储,在该文件下只改变cas.properties内的内容时,不需要将项目重新打包,只需要将tomact重启就可生效配置文件

### 配置tomact的,认证SSL ,其中这一步可不执行,跳转到
&nbsp;&nbsp;&nbsp;&nbsp;配置D:\apache-tomcat-8.0.30\conf下的server.xml文件,将下面的内容添加进出,认证SSL,其keystoreFile所指向的地方:为私钥存放的地方

```bash
 <Connector port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol"
               maxThreads="150" SSLEnabled="true" scheme="https" secure="true"
               clientAuth="false" sslProtocol="TLS"
               keystoreFile="D:\localhost.keystore" keystorePass="123456"
           />
```

&nbsp;&nbsp;&nbsp;&nbsp;将其内容配置好后,启动tomcat,访问地址:https://127.0.0.1:8443/cas/login