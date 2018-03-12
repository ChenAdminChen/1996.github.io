---
title: react-native-install-app
date: 2018.3.12 15:36:45
reward: false
tags:
    - react-native
    - android studio
---

### recat-native create project

#### node.js install

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;下载node.js安装包,[下载地址](https://nodejs.org/zh-cn/ "下载地址")

#### yarn install

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;使用node下载yarn

``` bash
npm install yarn
```

#### install SDK
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[SDK的下载地址](https://developer.android.com/studio/index.html "android studio")
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;配置adb服务,[下载地址](http://adbshell.com/downloads "adb 下载地址")

``` bash
#查看当前电脑连接的手机设备
adb devices
```

#### react-native install

``` bash
#安装react-native-cli包,获得react-native指令
yarn global add react-native-cli

#创建项目
react-native init AwesomeProject

cd AwesomeProject

#将安装app到客户端
react-native run-android
```
