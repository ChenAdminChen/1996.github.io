---
title: java equals
date: 2017/11/28 20:36:45
reward: false
tags: 
    - java
    - equals
---

###  == 与 equals基本比较

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(1)基本类型
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;基本数据类型，也称原始数据类型。byte,short,char,int,long,float,double,boolean。他们之间的比较，应用双等号（==）,比较的是他们的值

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(2)复合数据类型(类)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当他们用（==）进行比较的时候，比较的是他们在内存中的存放地址，所以，除非是同一个new出来的对象，他们的比较后的结果为true，否则比较后结果为false。 JAVA当中所有的类都是继承于Object这个基类的，在Object中的基类中定义了一个equals的方法，这个方法的初始行为是比较对象的内存地址，但在一些类库当中这个方法被覆盖掉了，如String,Integer,Date在这些类当中equals有其自身的实现，而不再是比较类在堆内存中的存放地址了。对于复合数据类型之间进行equals比较，在没有覆写equals方法的情况下，他们之间的比较还是基于他们在内存中的存放位置的地址值的，因为Object的equals方法也是用双等号（==）进行比较的，所以比较后的结果跟双等号（==）的结果相同。

### jdk中equals

``` bash
/**
     * Indicates whether some other object is "equal to" this one.
     * <p>
     * The {@code equals} method implements an equivalence relation
     * on non-null object references:
     * <ul>
     * <li>It is <i>reflexive</i>: for any non-null reference value
     *     {@code x}, {@code x.equals(x)} should return
     *     {@code true}.
     * <li>It is <i>symmetric</i>: for any non-null reference values
     *     {@code x} and {@code y}, {@code x.equals(y)}
     *     should return {@code true} if and only if
     *     {@code y.equals(x)} returns {@code true}.
     * <li>It is <i>transitive</i>: for any non-null reference values
     *     {@code x}, {@code y}, and {@code z}, if
     *     {@code x.equals(y)} returns {@code true} and
     *     {@code y.equals(z)} returns {@code true}, then
     *     {@code x.equals(z)} should return {@code true}.
     * <li>It is <i>consistent</i>: for any non-null reference values
     *     {@code x} and {@code y}, multiple invocations of
     *     {@code x.equals(y)} consistently return {@code true}
     *     or consistently return {@code false}, provided no
     *     information used in {@code equals} comparisons on the
     *     objects is modified.
     * <li>For any non-null reference value {@code x},
     *     {@code x.equals(null)} should return {@code false}.
     * </ul>
     * <p>
     * The {@code equals} method for class {@code Object} implements
     * the most discriminating possible equivalence relation on objects;
     * that is, for any non-null reference values {@code x} and
     * {@code y}, this method returns {@code true} if and only
     * if {@code x} and {@code y} refer to the same object
     * ({@code x == y} has the value {@code true}).
     * <p>
     * Note that it is generally necessary to override the {@code hashCode}
     * method whenever this method is overridden, so as to maintain the
     * general contract for the {@code hashCode} method, which states
     * that equal objects must have equal hash codes.
     *
     * @param   obj   the reference object with which to compare.
     * @return  {@code true} if this object is the same as the obj
     *          argument; {@code false} otherwise.
     * @see     #hashCode()
     * @see     java.util.HashMap
     */
    public boolean equals(Object obj) {
        return (this == obj);
    }

```

### class类中不重写equals方法

``` bash
public class TestEquals {
    public static void main(String[] args) {
        Value value1 = new Value();
        Value value2 = new Value();

        value1.i = value2.i = "sss";

        System.out.println("value1.equals(value2): " + value1.equals(value2));
        System.out.println("value1 == value2: " + (value1 == value2 ));

        System.out.println("value1.i:" + value1.i);
        System.out.println("value2.i:" + value2.i);

    }
}

class Value{
    String i;
}

#结果
value1.equals(value2): false
value1 == value2: false

```


### class类中重写equals

``` bash
public class TestEquals {
    public static void main(String[] args) {
        Value value1 = new Value();
        Value value2 = new Value();

        value1.i = value2.i = "sss";

        System.out.println("value1.equals(value2): " + value1.equals(value2));
        System.out.println("value1 == value2: " + (value1 == value2 ));

        System.out.println("value1.i:" + value1.i);
        System.out.println("value2.i:" + value2.i);

    }
}

class Value{
    String i;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        Value value = (Value) o;

        return i != null ? i.equals(value.i) : value.i == null;
    }

    @Override
    public int hashCode() {
        return i != null ? i.hashCode() : 0;
    }

#结果
value1.equals(value2): true
value1 == value2: false
```
