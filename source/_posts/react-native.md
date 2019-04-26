---
title: react-native install
date: 2018.03.30 14:03:45
reward: false
tags:
    - react-native
    - install
---

## react-native介绍

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;使用React Native，你可以使用标准的平台组件，例如iOS的UITabBar或安卓的Drawer。 这使你的app获得平台一致的视觉效果和体验，并且获得最佳的性能和流畅性。 使用对应的React component，就可以轻松地把这些原生组件整合到你的React Native应用中

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在Javascript代码和原生平台之间的所有操作都是异步执行的，并且原生模块还可以根据需要创建新的线程。这意味着你可以在主线程解码图片，然后在后台将它保存到磁盘，或者在不阻塞UI的情况下计算文字大小和界面布局等等。所以React Native开发的app天然具备流畅和反应灵敏的优势。Javascript和原生代码之间的通讯是完全可序列化的，这使得我们可以借助Chrome开发者工具去调试应用，而不论应用运行在模拟器还是真机上。

## react-native安装

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;使用npm安装环境，但这前提条件是配置好了SDK
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在visual studio code中编辑代码时，可以下载Flow Language Support

``` bash

#下载服务器
npm install global react-native

#安装react-native-cli，用于创建react-native的项目
npm install -g yarn react-native-cli

#安装yarn，用于管理组件
npm install yarn

#创建项目
react-native init AwesomeProject

#进入项目
cd AwesomeProject

#运行安装
react-native run-android

#下载其他需要的包时，可使用yarn 指令

```
