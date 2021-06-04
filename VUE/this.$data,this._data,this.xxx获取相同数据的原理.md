https://blog.csdn.net/weixin_41845146/article/details/85257924

```

function initData (vm: Component) {
  let data = vm.$options.data // vm.$options 是访问自定属性，此处就是vue实例中的 this.$data
  data = vm._data = typeof data === 'function' // 先执行三元表达式，然后赋值给 vm._data 和 data，这样子这两个值都是同时变化的，就是 this.$data.xxx 和 this._data 同时变化
    ? getData(data, vm)
    : data || {}
    ···
```
#### 重点来了：return data.call(vm, vm)
    data 函数执行的时候 用 call 方法，让 vm 继承了 data 的属性和方法，也就是 this 继承了 this.$option.data 的属性和方法， 所以我们可以使用 this.xxx

```
export function getData (data: Function, vm: Component): any {
  // #7573 disable dep collection when invoking data getters
  pushTarget()
  try {
    return data.call(vm, vm)
  } catch (e) {
    handleError(e, vm, `data()`)
    return {}
  } finally {
    popTarget()
  }
}
```


#### data 为什么是个函数而不是个对象

看以上代码，initData() 方法：先有个 三元表达式 typeof data === ‘function' ? getData(data, vm) : data || {} ，如果data是个函数则执行 getData() 方法，如果不是则返回原值，如果原值不存在则返回一个空对象，但是为什么 data 得是个函数？？？

如果 data 是个对象，那么整个vue实例将共享一份数据，也就是各个组件实例间可以随意修改其他组件的任意值，这就很坑爹了。

但是 data 定义成一个函数，将会 return 出一个唯一的对象，不会和其他组件共享一个对象。

我们注册组件的时候实际上是建立了一个组件构造器的引用，只有使用组件的时候才会真正创建一个组件实例。




### 扩展问题 import 组件 执行次数

import引入文件时，我们一个页面A引用了3次同一个组件B，但是B组件里又引用了1次组件C，那它们各import了几次呢？

答案：import 都只执行一次，但是不止如此，但凡 export default {} 外的方法都只执行一次，但是 export default {} 对象里的方法都执行了3次。
