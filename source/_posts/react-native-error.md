---
title: react-native error
date: 2018.04.26 09:19:34
tags: 
    - react-native error
categories: react-native
---

### react-native-charts-wrapper

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;该组件用于曲线，其[github地址](https://github.com/wuxudong/react-native-charts-wrapper 'github')，其react-native用0.54.0版本时，出现如下错误信息，[解决方法](https://github.com/wuxudong/react-native-charts-wrapper/issues/229 '解决的方法')

``` bash
#error
(Android) RN 0.54 Exception "local reference table overflow (max=512)" #229

#solve 
#在MPAndroidChartPackage.java/ createViewManagers()中添加如下内容：
ReadableNativeArray.setUseNativeAccessor(true);
ReadableNativeMap.setUseNativeAccessor(true);

```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;从react-native的源码可以看出ReadableNativeArray、ReadableNativeMap中的setUseNativeAccessor方法为static function，因此可在react-native-charts-wrapper源码加载LineChart数据之前的任何一个地方加载该方法，其问题就解决。

``` bash

https://github.com/facebook/react-native/blob/master/ReactAndroid/src/main/java/com/facebook/react/bridge/ReadableNativeMap.java

https://github.com/facebook/react-native/blob/master/ReactAndroid/src/main/java/com/facebook/react/bridge/ReadableNativeArray.java

```