---
title: java callBack function讲解
date: 2018.1.1 16:36:45
reward: false
tags: 
    - module call
    - java
    - callBack
---

### 模块调用

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;一个软件系统都会存在模块与模块之间的调用，其之间的调用分为：同步调用，异步调用，回调

#### 同步调用

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;同步调用是最简单也是最基本的一种调用方式，类A中的方法a()调用类B中的方法b()，等b()执行完成后再回到a()执行，该方式只适合执行完b()方法所需要的时间不长的情况，若b()执行所需要的时间过长时，a()方法处于等待状态，整个流程将会造成阻塞。

#### 异步调用

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;异步调用是为解决同步调用时造成的阻塞，类A中的方法a()通过启用一个线程调用类B中的方法b(),其a()方法的代码继续执行，无论b()方法执行多长时间都不会影响a()的执行，则解决了b()方法执行过程则造成的阻塞情况。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;异步调用中，a()方法不等待b()执行完，若a()方法中需要b()执行后返回的结果继续往下走，因此需要在a()内对b()方法执行的结果进行监听，a()获得结果后对其做出相应的操作。在java可以使用Futrue + Callable做到。

#### 回调

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;回调是指：类A的a()方法调用类B的b()方法，类B的b()方法执行完毕后主动调用类A的c()，方法其c()称为callback方法

### java callBack实现

实现两个整数相加简单的回调方法

#### 回调类接口

``` bash
//回调接口类
public interface CallBackTest {
    int add(int a, int b);
}

---------------------------------------------------------------------------------------
//A类中方法
public class CallBackTestA implements CallBackTest {

    private CallBackTestB callBackTestB;

    public CallBackTestA(CallBackTestB callBackTesB){
        this.callBackTestB = callBackTesB;
    }

    public void question(int a, int b){
        System.out.println("callBackTestA中的question函数 start");

        callBackTestB.addB(CallBackTestA.this ,a, b);

        System.out.println("callBackTestA中的question函数 end");

    }

    //回调方法的实现
    @Override
    public int add(int a, int b) {
        System.out.println("callBackTestA中的回调函数");
        return a + b;
    }

}

//内部类的实现
public class CallBackTestD  {

    private CallBackTestB callBackTestB;

    public CallBackTestA(CallBackTestB callBackTesB){
        this.callBackTestB = callBackTesB;
    }

    public void question(int a, int b){
        System.out.println("callBackTestA中的question函数 start");

        callBackTestB.addB(new CallBackTestC() ,a, b);

        System.out.println("callBackTestA中的question函数 end");

    }

    class CallBackTestC implements CallBackTest{
        //回调方法的实现
        @Override
        public int add(int a, int b) {
            System.out.println("callBackTestA中的回调函数");
            return a + b;
        }
    }
}

---------------------------------------------------------------------------------------
//B类中c的方法
public class CallBackTestB {

    public void addB(CallBackTest callBackTest, int a, int b){
        System.out.println("调用CallBackTestB中的addB start");

        callBackTest.add(a, b);

        System.out.println("调用CallBackTestB中的addB end");
    }
}

//测试
 @Test
    public void callBackTest(){
        CallBackTestB callBackTestB = new CallBackTestB();

        CallBackTestA callBackTestA = new CallBackTestA(callBackTestB);

        callBackTestA.question(1, 2);
    }

```