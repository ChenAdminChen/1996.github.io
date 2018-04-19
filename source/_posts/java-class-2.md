---
title: java class (二)
date: 2018.04.18 09:36:45
reward: false
tags: 
    - java
    - class
---

### 介绍class中数据类型

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;该文章引用于 https://blog.csdn.net/zhangjg_blog/article/details/21487287,若

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;class文件中的信息是一项一项排列的， 每项数据都有它的固定长度， 有的占一个字节， 有的占两个字节， 还有的占四个字节或8个字节， 数据项的不同长度分别用u1, u2, u4, u8表示， 分别表示一种数据项在class文件中占据一个字节， 两个字节， 4个字节和8个字节。 可以把u1, u2, u3, u4看做class文件数据项的“类型” 。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;class文件中存在以下数据项(该图表参考自《深入Java虚拟机》)：

类型 | 名称 | 数量 | 名称
---- | --- | ---
u4 | magic | 1 | 魔数
u2 | minor_version | 1 | 此版本号
u2 | major_version | 1 | 主版本号
u2 | constant_pool_count | 1
cp_info | constant_pool | constant_pool_count - 1
u2 | access_flags | 1
u2 | this_class | 1
u2 | super_class | 1
u2 | interfaces_count	 | 1
u2 | interfaces | interfaces_count
u2 | fields_count | 1
field_info | fields | fields_count
u2 | methods_count | 1
method_info | methods | methods_count
u2 | attribute_count | 1
attribute_info | attributes | attributes_count

### 介绍java class 中的魔数(magic)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在很多文件中都存在魔数,魔数是告诉调用者,该文件是属于那一类型,是否有用,在class文件中魔数放在第一位,其在class文件中的魔数占用了4个字节,网上说其class文件存在固定值 '0xCAFEBABE',将class文件以二进制流的形式打开文件，将会看到对应的魔数。

### 介绍java class 常量池中的数据项的类型

常量池中数据项类型 | 类型标志 | 类型描述 
---- | --- |
CONSTANT_Utf8 | 1 | UTF-8编码的Unicode字符串
CONSTANT_Integer | 3 | int类型字面值
CONSTANT_Float | 4 | float类型字面值
CONSTANT_Long | 5 | long类型字面值
CONSTANT_Double | 6 | double类型字面值
CONSTANT_Class | 7 | 对一个类或接口的符号引用
CONSTANT_String | 8 | String类型字面值
CONSTANT_Fieldref | 9 | 对一个字段的符号引用
CONSTANT_Methodref | 10 | 对一个类中声明的方法的符号引用
CONSTANT_InterfaceMethodref | 11 |对一个接口中声明的方法的符号引用
CONSTANT_NameAndType | 12 | 对一个字段或方法的部分符号引用

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;每个数据项叫做一个XXX_info项， 比如， 一个常量池中一个CONSTANT_Utf8类型的项， 就是一个CONSTANT_Utf8_info 。除此之外， 每个info项中都有一个标志值（tag）， 这个标志值表明了这个常量池中的info项的类型是什么， 从上面的表格中可以看出， 一个CONSTANT_Utf8_info中的tag值为1， 而一个CONSTANT_Fieldref_info中的tag值为9 。


### 常量池中存放的符号引用

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Java程序是动态链接的， 在动态链接的实现中， 常量池扮演者举足轻重的角色。 除了存放一些字面量之外， 常量池中还存放着以下几种符号引用：

（1） 类和接口的全限定名

（2） 字段的名称和描述符

（3） 方法的名称和描述符

#### 类和接口的全限定名：

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在常量池中， 一个类型的名字并不是我们在源文件中看到的那样， 也不是我们在源文件中使用的包名加类名的形式。 源文件中的全限定名和class文件中的全限定名不是相同的概念。 源文件中的全新定名是包名加类名， 包名的各个部分之间，包名和类名之间， 使用点号分割。 如Object类， 在源文件中的全限定名是java.lang.Object 。 而class文件中的全限定名是将点号替换成“/” 。 例如， Object类在class文件中的全限定名是 java/lang/Object 。 如果读者之前没有接触过class文件格式， 是class文件格式的初学者， 在这里不必知道全限定名在class文件中是如何使用的， 只需要知道， 源文件中一个类的名字， 在class文件中是用全限定名表述的。

#### 字段的名称和描述符

基本数据类型和void类型 | 类型的对应字符
---- | --- |
byte | B
char | C
double | D
float | F
int | I
long | J
short | S
boolean | Z
void | V

#### 类和接口，枚举描述

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;基本类型和void在描述符中都有一个大写字符和他们对应， 那么引用类型（类和接口，枚举）在描述符中是如何对应的呢？ 引用类型的对应字符串（注意， 引用类型在描述符中使用一个字符串做对应） ， 这个字符串的格式是：

``` bash

“L” + 类型的全限定名 + “;”

#55 = NameAndType        #74:#75        // getClass:()Ljava/lang/Class;
#74 = Utf8               getClass
#75 = Utf8               ()Ljava/lang/Class;

```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这三个部分之间没有空格， 是紧密排列的。 如Object在描述符中的对应字符串是： Ljava/lang/Object;  ； ArrayList在描述符中的对应字符串是： Ljava/lang/ArrayList;  ； 自定义类型com.examples.Person在描述符中的对应字符串是： Lcom/examples/Person; 。

#### 数组类型的描述

``` bash
若干个“[”  +  数组中元素类型的对应字符串

```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;int[]类型的对应字符串是： [I  。 int[][]类型的对应字符串是： [[I 。 Object[]类型的对应字符串是： [Ljava/lang/Object; 。 Object[][][]类型的对应字符串是： [[[Ljava/lang/Object; 。

#### 方法描述

``` bash

(参数1类型 参数2类型 参数3类型 ...)返回值类型

```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;下面举例说明（此表格来源于《深入Java虚拟机》）

方法描述符 | 方法声明
()I | int getSize()
()Ljava/lang/String; | String toString()
([Ljava/lang/String;)V | void main(String[] args)
()V | void wait()
(JI)V | void wait(long timeout, int nanos)
(ZILjava/lang/String;II)Z | boolean regionMatches(boolean ignoreCase, int toOffset, String other, int ooffset, int len)
([BII)I | int read(byte[] b, int off, int len )
()[[Ljava/lang/Object; | Object[][] getObjectArray()


#### 特殊方法的方法名

首先要明确一下， 这里的特殊方法是指的类的构造方法和类型初始化方法。 构造方法就不用多说了， 至于类型的初始化方法， 对应到源码中就是静态初始化块。 也就是说， 静态初始化块， 在class文件中是以一个方法表述的， 这个方法同样有方法描述符和方法名。 

类的构造方法的方法名使用字符串 <init> 表示， 而静态初始化方法的方法名使用字符串 <clinit> 表示。 除了这两种特殊的方法外， 其他普通方法的方法名， 和源文件中的方法名相同。

#### class文件中的访问标志信息

标志名 | 标志值 | 标志含义 | 针对的对像
---|---|---|
ACC_PUBLIC | 0x0001 | public类型 | 所有类型
ACC_FINAL | 0x0010 | final类型 | 类
ACC_SUPER | 0x0020 | 使用新的invokespecial语义 | 类和接口
ACC_INTERFACE | 0x0200 | 接口类型 | 接口
ACC_ABSTRACT | 0x0400 | 抽象类型 | 类和接口
ACC_SYNTHETIC | 0x1000 | 该类不由用户代码生成 | 所有类型
ACC_ANNOTATION | 0x2000 | 注解类型 | 注解
ACC_ENUM | 0x4000 | 枚举类型 | 枚举

#### class文件字段的访问标志信息（flag）


标志名 | 标志值 | 标志含义 | 设定者
---|---|---|
ACC_PUBLIC | 0x0001 | 字段被设为public | 类和接口
ACC_PRIVATE | 0x0002 | 字段被设为private | 类
ACC_PROTECTED | 0x0004 | 字段被设为protected | 类
ACC_STATIC | 0x0008 | 字段被设为static | 类和接口
ACC_FINAL | 0x0010 | 字段被设为final | 类和接口
ACC_VOLATILE | 0x0040 | 字段被设为volatile | 类
ACC_TRANSIENT | 0x0080 | 字段被设为transient | 类

#### 描述的是方法的访问标志信息

标志名 | 标志值 | 标志含义 | 设定者
---|---|---|
ACC_PUBLIC | 0x0001 | 方法设为public | 类和接口
ACC_PRIVATE | 0x0002 | 方法设为private | 类
ACC_PROTECTED | 0x0004 | 方法设为protected | 类
ACC_STATIC | 0x0008 | 方法设为static | 类
ACC_FINAL | 0x0010 | 方法设为final | 类
ACC_SYNCHRONIZED	0x0020 | 方法设为sychronized | 类
ACC_NATIVE	0x0100	方法设为native	类
ACC_ABSTRACT	0x0400	方法设为abstract	类和接口
ACC_STRICT	0x0800	方法设为strictFP	类和接口的<clinit>方法