---
title: linux mysql java time
date: 2017/11/20 19:36:45
reward: false
tags: 
    - linux
    - mysql
    - java
---


## linux

### linux查看当前系统时间

``` bash
root@localhost# date
```
### 引用TZ包 
``` bash
root@localhost# export TZ     #(TZ：time zone)
```

### linux tzselect的学习

#### teselect的介绍

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;teselect命令用于选择时区，但是tzselect只是帮助我们把选择的时区显示出来，并不会实际生效，也只是说我们按照它的提示去选择，结果只会告诉我们如何去设置环境变量TZ。如果要永久更改主时区，按照tzselect命令的信息,在.profile或者/etc/profile中设置正确的TZ环境变量。

``` bash
[root@new55 ~]# tzselect 
Please identify a location so that time zone rules can be set correctly.
Please select a continent or ocean.
 1) Africa
 2) Americas
 3) Antarctica
 4) Arctic Ocean
 5) Asia
 6) Atlantic Ocean
 7) Australia
 8) Europe
 9) Indian Ocean
10) Pacific Ocean
11) none - I want to specify the time zone using the Posix TZ format.
#? 5 
Please select a country.
 1) Afghanistan           18) Israel                35) Palestine
 2) Armenia               19) Japan                 36) Philippines
 3) Azerbaijan            20) Jordan                37) Qatar
 4) Bahrain               21) Kazakhstan            38) Russia
 5) Bangladesh            22) Korea (North)         39) Saudi Arabia
 6) Bhutan                23) Korea (South)         40) Singapore
 7) Brunei                24) Kuwait                41) Sri Lanka
 8) Cambodia              25) Kyrgyzstan            42) Syria
 9) China                 26) Laos                  43) Taiwan
10) Cyprus                27) Lebanon               44) Tajikistan
11) East Timor            28) Macau                 45) Thailand
12) Georgia               29) Malaysia              46) Turkmenistan
13) Hong Kong             30) Mongolia              47) United Arab Emirates
14) India                 31) Myanmar (Burma)       48) Uzbekistan
15) Indonesia             32) Nepal                 49) Vietnam
16) Iran                  33) Oman                  50) Yemen
17) Iraq                  34) Pakistan
#? 9 
Please select one of the following time zone regions.
1) east China - Beijing, Guangdong, Shanghai, etc.
2) Heilongjiang (except Mohe), Jilin
3) central China - Sichuan, Yunnan, Guangxi, Shaanxi, Guizhou, etc.
4) most of Tibet & Xinjiang
5) west Tibet & Xinjiang
#? 1

The following information has been given:

        China
        east China - Beijing, Guangdong, Shanghai, etc.

Therefore TZ='Asia/Shanghai' will be used.
Local time is now:      Mon Dec  6 09:40:35 CST 2010.
Universal Time is now:  Mon Dec  6 01:40:35 UTC 2010.
Is the above information OK?
1) Yes
2) No
#? 1

You can make this change permanent for yourself by appending the line
        TZ='Asia/Shanghai'; export TZ 
to the file '.profile' in your home directory; then log out and log in again.

Here is that TZ value again, this time on standard output so that you
can use the /usr/bin/tzselect command in shell scripts:
Asia/Shanghai

```

然后按照上面的选择后的结果

``` bash
TZ='Asia/Shanghai'; export TZ   #在系统文件中将配置并生效
```

### cd etc/sysconfig/clock

### 使用zdump读取/etc/localtime

``` bash
root@localhost# zdump -v /etc/localtime
```

### zoneinfo介绍

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;zoneinfo另一种查看时间信息的方法，其代码如下：

``` bash
root@localhost# cd /usr/share/zoneinfo/
root@localhost/usr/share/zoneinfo# ls #查看该文件中所有的信息
root@localhost/usr/share/zoneinfo# cat PRC # 查看当前时间设置的信息
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;如下配置的linux无法读取时间  export TZ='GMT+8'

## mysql

``` bash
#查看当前时区
show variables like 'time_zone';

#查看mysql的时间配置信息
show variables like '%time%'

#重启服务器
service mysql restart

```

## java时间

``` bash
export TZ='GMT+8'

#java 代码获得当前系统的时间
import java.util.Date;
import java.text.SimpleDateFormat;

public class NowString {
    public static void main(String[] args) { 
        SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");//设置日期格式
        System.out.println(df.format(new Date()));// new Date()为获取当前系统时间
    }
}

```