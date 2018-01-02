---
title: java-python-javaScript-iterator-iterable学习
tags: 
    - java
    - python
    - javaScript
    - filter
    - map
    - reduce
categories: filter
---

## java

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;自java更新到jdk8后，添加了lambda表达式及java.util.Stream,以下便是对此进行说明
Stream对数据进行处理，但对原来的数据不进行更改

### java创建stream的几种方法

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;From a Collection via the stream() and parallelStream() methods

``` bash

```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;From an array via Arrays.stream(Object[])

``` bash
int[] array = {1, 2, 3, 4, 5}

Stream stream = Arrays.stream(array)

```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;From static factory methods on the stream classes, such as Stream.of(Object[]), IntStream.range(int, int) or Stream.iterate(Object, UnaryOperator)

``` bash
int[] array = {1, 2, 3, 4, 5}

Stream stream = Stream.of(array)

Stream stream = IntStream.range(3,9)      //stream = {3, 4, 5, 6, 7, 8, 9}
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The lines of a file can be obtained from BufferedReader.lines()

``` bash
BufferedReader bufferedReader = new BufferedReader(new FileReader("E:\\tmdb_5000_movies2.json"));

Stream stream = bufferedReader.lines()

```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Streams of file paths can be obtained from methods in Files

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Streams of random numbers can be obtained from Random.ints()

``` bash
Random.ints().asLongStream()

```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Numerous other stream-bearing methods in the JDK, including BitSet.stream(), Pattern.splitAsStream(java.lang.CharSequence), and JarFile.stream()

### filter

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对stream的对象内的内容进行对其内部的所有数据进行循环，过滤不符合条件的数据，不会带来副作用

``` bash
int[] array = {1, 2, 3, 4, 5}

Stream stream = Stream.of(array)

stream.filter(e -> e / 2 == 0).forEach(System.out::println)  // --> {2, 4}

```

### map

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;获得stream对象中的数据进行操作，对其内部的所有数据进行循环操作，若用它进行过滤，将会带来副作用，不仅能返回值，同时返回可过滤后的结果Boolean值

``` bash
int[] array = {1, 2, 3, 4, 5}

Stream stream = Stream.of(array)

stream.filter(e -> e / 2 == 0)
.map(e -> e*2)
.forEach(System.out::println)  // --> {4, 8}

```

### reduce

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;该函数处于stream终端，用了reduce函数后不可再用stream的其他方法，若需要使用stream的方法时需要重新stream化.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在java8中reduce方法重载有三个，三个方法中的参数数量不同

``` bash
# supplier为reduce方法开始的参数，同时决定着返回值的类型
# 当stream为并行化时才会调用combiner
stream.parallel().reduce(Supplier<R> supplier, BiConsumer<R, ? super T> accumulator, BiConsumer<R, R> combiner)

stream.reduce(Supplier<R> supplier, BiConsumer<R, ? super T> accumulator, BiConsumer<R, R> combiner)

stream.reduce(BiConsumer<R, ? super T> accumulator)

```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

``` bash
stream.filter(e -> e / 2 == 0)
.map(e -> e*2)
.reduce()

```

## javaScript

### iterator

### iterable
