---
title: flutter
date: 2018.03.27 14:36:45
reward: false
tags:
    - flutter
    - image
    - install
---

## flutter介绍

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Flutter 是一个跨平台（Android 和 iOS）的移动开发框架，使用的是 Dart 语言。和 React Native 不同的是，Flutter 框架并不是一个严格意义上的原生应用开发框架。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Flutter 的目标是用来创建高性能、高稳定性、高帧率、低延迟的 Android 和 iOS 应用。并且开发出来的应用在不同的平台用起来跟原生应用具有一样的体验。不同的平台的原生体验应该得到保留，让该应用看起来同整个系统更加协调。不同平台的滚动操作、字体、图标 等特殊的特性 应该和该平台上的其他应用保持一致，让用户感觉就像操作原生应用一样。比如，返回图标 Android 和 iOS 是不一样的；滚动内容滚动到底的反馈也是不一样的。

## 安装flutter

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;使用Git下载服务，并且配置好环境变量,便可使用flutter，但这前提条件是配置好了SDK

``` bash

#下载服务器
git clone -b beta https://github.com/flutter/flutter.git

#配置好环境变量
path = D:\flutter\bin;

#确定是否有硬件连接电脑
flutter devices

#进入某个项目下 运行安装
flutter run

```

## flutter mirror config

sudo nano /etc/profile
```
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
```
source /etc/profile

## flutter change channel

```
# show channel options 
flutter channel
```

## flutter change version

```
# show version options
flutter version

# upgrade version 
flutter version 1.5.4

# upgrade version
flutter upgrade

```

## flutter image

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;flutter中图片的加载分为从网络上获得，或者本地加载图片

### flutter Image.assets

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Images.assets用于加载本地图片，项目结构如下图，

基本步骤如下：

``` bash
# 在pubspec.yaml中添加图片地址

flutter:
  uses-material-design: true

  # 图片地址
  assets:
   - assets/images/search.png

# 引入图片：
  new Image.assets('assets/images/search.png', width:200.0, height:200.0)
```