---
title: java class (一)
date: 2018.04.17 17:36:45
reward: false
tags: 
    - java
    - class
---

### java 类

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;现在创建一个普通的实体类，并且该类带有main方法，如下代码

``` bash

public class Person {
    String name;
    int age;
    String describe;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getDescribe() {
        return describe;
    }

    public void setDescribe(String describe) {
        this.describe = describe;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        Person person = (Person) o;

        if (age != person.age) return false;
        if (name != null ? !name.equals(person.name) : person.name != null) return false;
        return describe != null ? describe.equals(person.describe) : person.describe == null;
    }

    @Override
    public int hashCode() {
        int result = name != null ? name.hashCode() : 0;
        result = 31 * result + age;
        result = 31 * result + (describe != null ? describe.hashCode() : 0);
        return result;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", describe='" + describe + '\'' +
                '}';
    }

	 public static void main(String[] args) {
        System.out.println("测试");
    }

}

```

### javac编译

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;进入到当前目录，对Person.java进行编译，其 'javac className.java'

``` bash

javac Person.java  #其会在当前目标下生成Person.class

```

### java 执行

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;进入到当前目录，执行Person.class,其 'java className'

``` bash

java Person

```

### 查看编译过后生成的class文件中的常量池

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;进入到当前目录，执行 'javap -verbose className.class',可以看到编译过后的常量池

``` bash
javap -verbose Person.class

#结果如下
Classfile /E:/Users/java/Person.class
  Last modified 2018-4-18; size 1726 bytes
  MD5 checksum 51074296dee03987f028c680cf1b10f7
  Compiled from "Person.java"
public class Person
  minor version: 0
  major version: 52
  flags: ACC_PUBLIC, ACC_SUPER
Constant pool:
   #1 = Methodref          #21.#51        // java/lang/Object."<init>":()V
   #2 = Fieldref           #6.#52         // Person.name:Ljava/lang/String;
   #3 = Fieldref           #6.#53         // Person.age:I
   #4 = Fieldref           #6.#54         // Person.describe:Ljava/lang/String;
   #5 = Methodref          #21.#55        // java/lang/Object.getClass:()Ljava/lang/Class;
   #6 = Class              #56            // Person
   #7 = Methodref          #57.#58        // java/lang/String.equals:(Ljava/lang/Object;)Z
   #8 = Methodref          #57.#59        // java/lang/String.hashCode:()I
   #9 = Class              #60            // java/lang/StringBuilder
  #10 = Methodref          #9.#51         // java/lang/StringBuilder."<init>":()V
  #11 = String             #61            // Person{name='
  #12 = Methodref          #9.#62         // java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
  #13 = Methodref          #9.#63         // java/lang/StringBuilder.append:(C)Ljava/lang/StringBuilder;
  #14 = String             #64            // , age=
  #15 = Methodref          #9.#65         // java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
  #16 = String             #66            // , describe='
  #17 = Methodref          #9.#67         // java/lang/StringBuilder.toString:()Ljava/lang/String;
  #18 = Fieldref           #68.#69        // java/lang/System.out:Ljava/io/PrintStream;
  #19 = String             #70            // 娴嬭瘯
  #20 = Methodref          #71.#72        // java/io/PrintStream.println:(Ljava/lang/String;)V
  #21 = Class              #73            // java/lang/Object
  #22 = Utf8               name
  #23 = Utf8               Ljava/lang/String;
  #24 = Utf8               age
  #25 = Utf8               I
  #26 = Utf8               describe
  #27 = Utf8               <init>
  #28 = Utf8               ()V
  #29 = Utf8               Code
  #30 = Utf8               LineNumberTable
  #31 = Utf8               getName
  #32 = Utf8               ()Ljava/lang/String;
  #33 = Utf8               setName
  #34 = Utf8               (Ljava/lang/String;)V
  #35 = Utf8               getAge
  #36 = Utf8               ()I
  #37 = Utf8               setAge
  #38 = Utf8               (I)V
  #39 = Utf8               getDescribe
  #40 = Utf8               setDescribe
  #41 = Utf8               equals
  #42 = Utf8               (Ljava/lang/Object;)Z
  #43 = Utf8               StackMapTable
  #44 = Class              #56            // Person
  #45 = Utf8               hashCode
  #46 = Utf8               toString
  #47 = Utf8               main
  #48 = Utf8               ([Ljava/lang/String;)V
  #49 = Utf8               SourceFile
  #50 = Utf8               Person.java
  #51 = NameAndType        #27:#28        // "<init>":()V
  #52 = NameAndType        #22:#23        // name:Ljava/lang/String;
  #53 = NameAndType        #24:#25        // age:I
  #54 = NameAndType        #26:#23        // describe:Ljava/lang/String;
  #55 = NameAndType        #74:#75        // getClass:()Ljava/lang/Class;
  #56 = Utf8               Person
  #57 = Class              #76            // java/lang/String
  #58 = NameAndType        #41:#42        // equals:(Ljava/lang/Object;)Z
  #59 = NameAndType        #45:#36        // hashCode:()I
  #60 = Utf8               java/lang/StringBuilder
  #61 = Utf8               Person{name='
  #62 = NameAndType        #77:#78        // append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
  #63 = NameAndType        #77:#79        // append:(C)Ljava/lang/StringBuilder;
  #64 = Utf8               , age=
  #65 = NameAndType        #77:#80        // append:(I)Ljava/lang/StringBuilder;
  #66 = Utf8               , describe='
  #67 = NameAndType        #46:#32        // toString:()Ljava/lang/String;
  #68 = Class              #81            // java/lang/System
  #69 = NameAndType        #82:#83        // out:Ljava/io/PrintStream;
  #70 = Utf8               娴嬭瘯
  #71 = Class              #84            // java/io/PrintStream
  #72 = NameAndType        #85:#34        // println:(Ljava/lang/String;)V
  #73 = Utf8               java/lang/Object
  #74 = Utf8               getClass
  #75 = Utf8               ()Ljava/lang/Class;
  #76 = Utf8               java/lang/String
  #77 = Utf8               append
  #78 = Utf8               (Ljava/lang/String;)Ljava/lang/StringBuilder;
  #79 = Utf8               (C)Ljava/lang/StringBuilder;
  #80 = Utf8               (I)Ljava/lang/StringBuilder;
  #81 = Utf8               java/lang/System
  #82 = Utf8               out
  #83 = Utf8               Ljava/io/PrintStream;
  #84 = Utf8               java/io/PrintStream
  #85 = Utf8               println
{
  java.lang.String name;
    descriptor: Ljava/lang/String;
    flags:

  int age;
    descriptor: I
    flags:

  java.lang.String describe;
    descriptor: Ljava/lang/String;
    flags:

  public Person();
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: return
      LineNumberTable:
        line 1: 0

  public java.lang.String getName();
    descriptor: ()Ljava/lang/String;
    flags: ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: getfield      #2                  // Field name:Ljava/lang/String;
         4: areturn
      LineNumberTable:
        line 7: 0

  public void setName(java.lang.String);
    descriptor: (Ljava/lang/String;)V
    flags: ACC_PUBLIC
    Code:
      stack=2, locals=2, args_size=2
         0: aload_0
         1: aload_1
         2: putfield      #2                  // Field name:Ljava/lang/String;
         5: return
      LineNumberTable:
        line 11: 0
        line 12: 5

  public int getAge();
    descriptor: ()I
    flags: ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: getfield      #3                  // Field age:I
         4: ireturn
      LineNumberTable:
        line 15: 0

  public void setAge(int);
    descriptor: (I)V
    flags: ACC_PUBLIC
    Code:
      stack=2, locals=2, args_size=2
         0: aload_0
         1: iload_1
         2: putfield      #3                  // Field age:I
         5: return
      LineNumberTable:
        line 19: 0
        line 20: 5

  public java.lang.String getDescribe();
    descriptor: ()Ljava/lang/String;
    flags: ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: getfield      #4                  // Field describe:Ljava/lang/String;
         4: areturn
      LineNumberTable:
        line 23: 0

  public void setDescribe(java.lang.String);
    descriptor: (Ljava/lang/String;)V
    flags: ACC_PUBLIC
    Code:
      stack=2, locals=2, args_size=2
         0: aload_0
         1: aload_1
         2: putfield      #4                  // Field describe:Ljava/lang/String;
         5: return
      LineNumberTable:
        line 27: 0
        line 28: 5

  public boolean equals(java.lang.Object);
    descriptor: (Ljava/lang/Object;)Z
    flags: ACC_PUBLIC
    Code:
      stack=2, locals=3, args_size=2
         0: aload_0
         1: aload_1
         2: if_acmpne     7
         5: iconst_1
         6: ireturn
         7: aload_1
         8: ifnull        22
        11: aload_0
        12: invokevirtual #5                  // Method java/lang/Object.getClass:()Ljava/lang/Class;
        15: aload_1
        16: invokevirtual #5                  // Method java/lang/Object.getClass:()Ljava/lang/Class;
        19: if_acmpeq     24
        22: iconst_0
        23: ireturn
        24: aload_1
        25: checkcast     #6                  // class Person
        28: astore_2
        29: aload_0
        30: getfield      #3                  // Field age:I
        33: aload_2
        34: getfield      #3                  // Field age:I
        37: if_icmpeq     42
        40: iconst_0
        41: ireturn
        42: aload_0
        43: getfield      #2                  // Field name:Ljava/lang/String;
        46: ifnull        66
        49: aload_0
        50: getfield      #2                  // Field name:Ljava/lang/String;
        53: aload_2
        54: getfield      #2                  // Field name:Ljava/lang/String;
        57: invokevirtual #7                  // Method java/lang/String.equals:(Ljava/lang/Object;)Z
        60: ifne          75
        63: goto          73
        66: aload_2
        67: getfield      #2                  // Field name:Ljava/lang/String;
        70: ifnull        75
        73: iconst_0
        74: ireturn
        75: aload_0
        76: getfield      #4                  // Field describe:Ljava/lang/String;
        79: ifnull        96
        82: aload_0
        83: getfield      #4                  // Field describe:Ljava/lang/String;
        86: aload_2
        87: getfield      #4                  // Field describe:Ljava/lang/String;
        90: invokevirtual #7                  // Method java/lang/String.equals:(Ljava/lang/Object;)Z
        93: goto          108
        96: aload_2
        97: getfield      #4                  // Field describe:Ljava/lang/String;
       100: ifnonnull     107
       103: iconst_1
       104: goto          108
       107: iconst_0
       108: ireturn
      LineNumberTable:
        line 32: 0
        line 33: 7
        line 35: 24
        line 37: 29
        line 38: 42
        line 39: 75
      StackMapTable: number_of_entries = 10
        frame_type = 7 /* same */
        frame_type = 14 /* same */
        frame_type = 1 /* same */
        frame_type = 252 /* append */
          offset_delta = 17
          locals = [ class Person ]
        frame_type = 23 /* same */
        frame_type = 6 /* same */
        frame_type = 1 /* same */
        frame_type = 20 /* same */
        frame_type = 10 /* same */
        frame_type = 64 /* same_locals_1_stack_item */
          stack = [ int ]

  public int hashCode();
    descriptor: ()I
    flags: ACC_PUBLIC
    Code:
      stack=2, locals=2, args_size=1
         0: aload_0
         1: getfield      #2                  // Field name:Ljava/lang/String;
         4: ifnull        17
         7: aload_0
         8: getfield      #2                  // Field name:Ljava/lang/String;
        11: invokevirtual #8                  // Method java/lang/String.hashCode:()I
        14: goto          18
        17: iconst_0
        18: istore_1
        19: bipush        31
        21: iload_1
        22: imul
        23: aload_0
        24: getfield      #3                  // Field age:I
        27: iadd
        28: istore_1
        29: bipush        31
        31: iload_1
        32: imul
        33: aload_0
        34: getfield      #4                  // Field describe:Ljava/lang/String;
        37: ifnull        50
        40: aload_0
        41: getfield      #4                  // Field describe:Ljava/lang/String;
        44: invokevirtual #8                  // Method java/lang/String.hashCode:()I
        47: goto          51
        50: iconst_0
        51: iadd
        52: istore_1
        53: iload_1
        54: ireturn
      LineNumberTable:
        line 44: 0
        line 45: 19
        line 46: 29
        line 47: 53
      StackMapTable: number_of_entries = 4
        frame_type = 17 /* same */
        frame_type = 64 /* same_locals_1_stack_item */
          stack = [ int ]
        frame_type = 255 /* full_frame */
          offset_delta = 31
          locals = [ class Person, int ]
          stack = [ int ]
        frame_type = 255 /* full_frame */
          offset_delta = 0
          locals = [ class Person, int ]
          stack = [ int, int ]

  public java.lang.String toString();
    descriptor: ()Ljava/lang/String;
    flags: ACC_PUBLIC
    Code:
      stack=2, locals=1, args_size=1
         0: new           #9                  // class java/lang/StringBuilder
         3: dup
         4: invokespecial #10                 // Method java/lang/StringBuilder."<init>":()V
         7: ldc           #11                 // String Person{name='
         9: invokevirtual #12                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
        12: aload_0
        13: getfield      #2                  // Field name:Ljava/lang/String;
        16: invokevirtual #12                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
        19: bipush        39
        21: invokevirtual #13                 // Method java/lang/StringBuilder.append:(C)Ljava/lang/StringBuilder;
        24: ldc           #14                 // String , age=
        26: invokevirtual #12                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
        29: aload_0
        30: getfield      #3                  // Field age:I
        33: invokevirtual #15                 // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
        36: ldc           #16                 // String , describe='
        38: invokevirtual #12                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
        41: aload_0
        42: getfield      #4                  // Field describe:Ljava/lang/String;
        45: invokevirtual #12                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
        48: bipush        39
        50: invokevirtual #13                 // Method java/lang/StringBuilder.append:(C)Ljava/lang/StringBuilder;
        53: bipush        125
        55: invokevirtual #13                 // Method java/lang/StringBuilder.append:(C)Ljava/lang/StringBuilder;
        58: invokevirtual #17                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
        61: areturn
      LineNumberTable:
        line 52: 0

  public static void main(java.lang.String[]);
    descriptor: ([Ljava/lang/String;)V
    flags: ACC_PUBLIC, ACC_STATIC
    Code:
      stack=2, locals=1, args_size=1
         0: getstatic     #18                 // Field java/lang/System.out:Ljava/io/PrintStream;
         3: ldc           #19                 // String 娴嬭瘯
         5: invokevirtual #20                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
         8: return
      LineNumberTable:
        line 60: 0
        line 61: 8
}
SourceFile: "Person.java"

```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;查看编译过后class文件,从上面打印能够显示的看到的存在常量池中的变量存在<font color = 'red' size = 7 >85项</font>,其实它存在<font color = 'red' size = 7 >86项</font>,不要忘记了第0个常量,因为第0个常量被用来表示class中的数据项不引用任何常量池中的常量.