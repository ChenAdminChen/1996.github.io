---
title: android
date: 2018.11.27 16:36:45
reward: false
tags:
    - linux
    - android
---

## linux-android

linux下安装android-studio,开启模拟器时，显示/dev/kvm不存在

>进入BIOS系统设置支持kvm，开启虚拟机

```
Virtualization technolog = enabled
```

>重启后，/dev/kvm存在

1.将用户添加到kvm组

1.1 kvm组不存在
```
egrep -c '(vmx|svm)' /proc/cpuinfo

sudo apt-get install cpu-checker

sudo kvm-ok  #检查kvm是否存在

sudo apt-get install qemu-kvm  #创建kvm组

```

1.2 kvm组存在
```
id #查看当前用户存在的组信息

usermod -a -G kvm user_name  #将user_name添加到kvm 组内

```

2.重启系统配置生效





