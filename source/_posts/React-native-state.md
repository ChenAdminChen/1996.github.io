---
title: React Native State
date: 2018.03.14 13:36:45
reward: false
tags:
    - React Native
    - State
---

### React Native state定义

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;State必须能代表一个组件UI呈现的完整状态集，即组件的任何UI改变，都可以从State的变化中反映出来；同时，State还必须是代表一个组件UI呈现的最小状态集，即State中的所有状态都是用于反映组件UI的变化，没有任何多余的状态，也不需要通过其他状态计算而来的中间状态。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;组件中用到的一个变量是不是应该作为组件State，可以通过下面的4条依据进行判断：
<ul>
    <li这个变量是否是通过Props从父组件中获取？如果是，那么它不是一个状态。</li>
    <li>这个变量是否在组件的整个生命周期中都保持不变？如果是，那么它不是一个状态。</li>
    <li>这个变量是否可以通过其他状态（State）或者属性(Props)计算得到？如果是，那么它不是一个状态。</li>
    <li>这个变量是否在组件的render方法中使用？如果不是，那么它不是一个状态。这种情况下，这个变量更适合定义为组件的一个普通属性，例如组件中用到的定时器，就应该直接定义为this.timer，this.state.timer。</li>
    <li>请务必牢记，并不是组件中用到的所有变量都是组件的状态！当存在多个组件共同依赖一个状态时，一般的做法是状态上移，将这个状态放到这几个组件的公共父组件中</li>
</ul>

### React Native 声明及修改

``` bash
#声明
class react extends React.component{
    consurctor(props){
        super(props);
        this.state = {
            title: "测试",
            content: "测试中的内容"
        };
    }
}

#修改时，可以部分修改
this.setState({
    title: "改变title",
});

```

### State 的更新是一个浅合并（Shallow Merge）的过程

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当调用setState修改组件状态时，只需要传入发生改变的State，而不是组件完整的State，因为组件State的更新是一个浅合并（Shallow Merge）的过程

### State的更新是异步的
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;调用setState，组件的state并不会立即改变，setState只是把要修改的状态放入一个队列中，React会优化真正的执行时机，并且React会出于性能原因，可能会将多次setState的状态修改合并成一次状态修改。所以不要依赖当前的State，计算下个State。当真正执行状态修改时，依赖的this.state并不能保证是最新的State，因为React会把多次State的修改合并成一次，这时，this.state将还是这几次State修改前的State。另外需要注意的事，同样不能依赖当前的Props计算下个状态，因为Props一般也是从父组件的State中获取，依然无法确定在组件状态更新时的值。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;举个例子，对于一个电商类应用，在我们的购物车中，当我们点击一次购买数量按钮，购买的数量就会加1，如果我们连续点击了两次按钮，就会连续调用两次this.setState({number: this.state.number + 1})，在React合并多次修改为一次的情况下，相当于等价执行了如下代码：

``` bash
Object.assign(
  {number: this.state.number + 1},
  {number: this.state.number + 1}
)
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;后面的操作覆盖掉了前面的操作，最终购买的数量只增加了1个。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;可以使用另一个接收一个函数作为参数的setState，这个函数有两个参数，第一个是当前最新状态（本次组件状态修改后的状态）的前一个状态preState（本次组件状态修改前的状态），第二个参数是当前最新的属性props。如下所示：

``` bash
// 正确
this.setState((preState, props) => {
  counter: preState.number + 1;
})

```