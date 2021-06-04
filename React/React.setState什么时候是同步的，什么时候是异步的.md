#### setState何时同步何时异步？
由React控制的事件处理程序，以及生命周期函数调用setState不会同步更新state 。

React控制之外的事件中调用setState是同步更新的。比如原生js绑定的事件，setTimeout/setInterval等。




### 参数为函数的setState用法

```
handleClick() {
  this.setState({
    count: this.state.count + 1
  })
  this.setState({
    count: this.state.count + 1
  })
  this.setState({
    count: this.state.count + 1
  })
}
```

最终的结果只加了1

因为调用this.setState时，并没有立即更改this.state，所以this.setState只是在反复设置同一个值而已，上面的代码等同于这样
```
handleClick() {
  const count = this.state.count

  this.setState({
    count: count + 1
  })
  this.setState({
    count: count + 1
  })
  this.setState({
    count: count + 1
  })
}
```

count相当于一个快照，所以不管重复多少次，结果都是加1。

```
increment(state, props) {
  return {
    count: state.count + 1
  }
}

handleClick() {
  this.setState(this.increment)
  this.setState(this.increment)
  this.setState(this.increment)
}
```

结果: 13

对于多次调用函数式setState的情况，React会保证调用每次increment时，state都已经合并了之前的状态修改结果。

也就是说，第一次调用this.setState(increment)，传给increment的state参数的count是10，第二调用是11，第三次调用是12，最终handleClick执行完成后的结果就是this.state.count变成了13。

值得注意的是：在increment函数被调用时，this.state并没有被改变，依然要等到render函数被重新执行时（或者shouldComponentUpdate函数返回false之后）才被改变，因为render只执行一次。



在同一个事件处理程序中不要混用

```
increment(state, props) {
  return {
    count: state.count + 1
  }
}

handleClick() {
  this.setState(this.increment)
  this.setState({ count: this.state.count + 1 })
  this.setState(this.increment)
}
```

结果： 12

第一次执行setState，count为11，第二次执行，this.state仍然是没有更新的状态，所以this.state.count又打回了原形为10，加1以后变成11，最后再执行setState，所以最终count的结果是12。（render依然只执行一次）

setState的第二个回调参数会在更新state，重新触发render后执行。


https://www.jianshu.com/p/799b8a14ef96
