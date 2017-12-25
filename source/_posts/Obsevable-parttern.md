---
title: Obsevable parttern
date: 2017/12/25 20:36:45
reward: false
tags: 
    - java
    - javaScript
    - python
    - observable
---

## 观察者模式

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;观察者模式是对象的行为模式，又叫发布-订阅(Publish/Subscribe)模式、模型-视图(Model/View)模式、源-监听器(Source/Listener)模式或从属者(Dependents)模式。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;观察者模式定义了一种一对多的依赖关系，让多个观察者对象同时监听某一个主题对象。这个主题对象在状态上发生变化时，会通知所有观察者对象，使它们能够自动更新自己。

## 观察者模式组成

<ul>
<li>1.抽象主题角色：把所有对观察者对象的引用保存在一个集合中，每个抽象主题角色都可以有任意数量的观察者。抽象主题提供一个接口，可以增加和删除观察者角色。一般用一个抽象类和接口来实现。</li>
<li>2.抽象观察者角色：为所有具体的观察者定义一个接口，在得到主题的通知时更新自己。</li>
<li>3.具体主题角色：在具体主题内部状态改变时，给所有登记过的观察者发出通知。具体主题角色通常用一个子类实现。</li>
<li>4.具体观察者角色：该角色实现抽象观察者角色所要求的更新接口，以便使本身的状态与主题的状态相协调。通常用一个子类实现。如果需要，具体观察者角色可以保存一个指向具体主题角色的引用。</li>
</ul>
![observable的图片](https://raw.githubusercontent.com/ChenAdminChen/1996_imgs/master/blog_img/java/observer.png)

## 观察者模式优点

　　第一、观察者模式在被观察者和观察者之间建立一个抽象的耦合。被观察者角色所知道的只是一个具体观察者列表，每一个具体观察者都符合一个抽象观察者的接口。被观察者并不认识任何一个具体观察者，它只知道它们都有一个共同的接口。 
　　由于被观察者和观察者没有紧密地耦合在一起，因此它们可以属于不同的抽象化层次。如果被观察者和观察者都被扔到一起，那么这个对象必然跨越抽象化和具体化层次。

　　第二、观察者模式支持广播通讯。被观察者会向所有的登记过的观察者发出通知
　　观察者模式有下面的缺点： 
　　第一、如果一个被观察者对象有很多的直接和间接的观察者的话，将所有的观察者都通知到会花费很多时间。 
　　第二、如果在被观察者之间有循环依赖的话，被观察者会触发它们之间进行循环调用，导致系统崩溃。在使用观察者模式是要特别注意这一点。 
　　第三、如果对观察者的通知是通过另外的线程进行异步投递的话，系统必须保证投递是以自恰的方式进行的。 
　　第四、虽然观察者模式可以随时使观察者知道所观察的对象发生了变化，但是观察者模式没有相应的机制使观察者知道所观察的对象是怎么发生变化的。 

### java库的observable与observer

&nbsp;&nbsp;&nbsp;&nbsp;学习地址：https://docs.oracle.com/javase/8/docs/api/java/util/Observable.html
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在java中观察者模式存在被观察者与观察者的接口类，若需要实现观察者模式时，可以继承java.util.Observable及实现java.util.Observer两个接口就可以完成了，以下为实现两个接口的实现

``` bash
// observer
public class ConcretObserver implements Observer{
    @Override
    public void update(Observable o, Object arg) {
        System.out.println(o+ "\t\t"+ arg);
    }
}

//observable
public class ConcretObservable extends Observable {
    private String data = "";

    public String retrieveData()
    {
        return data;
    }

    public void changeData(String data)
    {
        if ( !this.data.equals( data) )
        {
            this.data = data;
            setChanged();     //记录上一次广播的数据与这一次的是否一样，若一样则不广播
        }

        notifyObservers(data);
    }
}

//test
public class TestObservable {
     public static void main(String args[]) {

         ConcretObservable observable = new ConcretObservable();

         Observer observer = new ConcretObserver();
         Observer observer1 = new ConcretObserver();
         Observer observer2 = new ConcretObserver();
         Observer observer3 = new ConcretObserver();

         observable.addObserver(observer);
         observable.addObserver(observer1);
         observable.addObserver(observer2);
         observable.addObserver(observer3);

         observable.changeData("测试");
         observable.changeData("测试");
         observable.changeData("测试1");
      }

}
```

### 自定义实现观察者模式
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;若将观察者模式提升到一个协议层，可以大概认为只要做到以下两个原则，便可认为它是一个观察者模式
<ul>
    <li>一个观察者类中存在一个动作方法</li>
    <li>被观察者类中应存在一个存观察者的set集合，添加观察者的方法、删除观察者的方法及循环调用观察者的动作的方法</li>
</ul>

#### java实现观察者模式
``` bash
//observble
public interface Watched {
    void addWatch(Watcher watcher);

    void removeWatch(Watcher watcher);

    void notifyWatchers(String str);

}

/**
 * 具体的被观察者
 * Created by chen on 2017/12/19.
 */
public class ConcretWatched implements Watched {
    private List<Watcher> watchers = new ArrayList<>();

    @Override
    public void addWatch(Watcher watcher) {
        if(!watchers.contains(watcher)){
            watchers.add(watcher);
        }
    }

    @Override
    public void removeWatch(Watcher watcher) {
        watchers.remove(watcher);
    }

    /**
     * 自动调用实际上是主题进行调用的
     * @param str
     */
    @Override
    public void notifyWatchers(String str) {
        for (Watcher watcher : watchers)
        {
            watcher.update(str);
        }
    }
}

//observer
public interface Watcher {
    void update(String str);
}

/**
 * 具体的观察者
 * Created by chen on 2017/12/19.
 */
public class ConcretWatcher implements Watcher {
    @Override
    public void update(String str) {
        System.out.println(str);
    }
}


//test
public class TestWatch {
     public static void main(String args[]) {
         Watched  wathced = new ConcretWatched();

         Watcher watcher = new ConcretWatcher();
         Watcher watcher1 = new ConcretWatcher();
         Watcher watcher2 = new ConcretWatcher();
         Watcher watcher3 = new ConcretWatcher();

         wathced.addWatch(watcher);
         wathced.addWatch(watcher1);
         wathced.addWatch(watcher2);
         wathced.addWatch(watcher3);

         wathced.notifyWatchers("测试");

     }
}
```

### javaScript实现观察者模式

``` bash

#订阅者
class Subscriber{
    constructor(name){
        this.name = name;
    }

    update(message){
        console.log (this.name + “收到通知消息”+ message);
    }
}

#生产者
class publisher{
    constructor(){
        this.observers = new Set();
    }

    register(who){
        this.observers.add(who);
    }

    unregister(who){
        this.observers.delete(who);
    }

    dispatch(message){
        for (let observer of this.observers){
            observer.update(message);
        }
    }
}

```

### python实现观察者模式
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;学习地址：http://www.giantflyingsaucer.com/blog/?p=5117
``` bash

class Subscriber:
     def __init__(self, name):
         self.name = name
     def update(self, message):
         print('{} got message "{}"'.format(self.name, message))

class Publisher:
     def __init__(self):
         self.subscribers = set()
     def register(self,who):
         self.subscribers.add(who)
     def unregister(self, who):
         self.subscribers.discard(who)
     def dispathc(self, message):
         for subscriber in self.subscribers:
             subscriber.update(message)

pub = Publisher()
bob = Subscriber('Bob')
alice = Subscriber('Alice')
john = Subscriber('John')
pub.register(bob)
pub.register(alice)
pub.register(john)
pub.dispatch("It's lunchtime!")
pub.unregister(john)
pub.dispatch("Time for dinner")

```