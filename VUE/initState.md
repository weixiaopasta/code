
### initState
```
export function initState (vm: Component) {
  vm._watchers = []
  const opts = vm.$options
  if (opts.props) initProps(vm, opts.props)
  if (opts.methods) initMethods(vm, opts.methods)
  if (opts.data) {
    initData(vm) // 绑定this
  } else {
    observe(vm._data = {}, true /* asRootData */)
  }
  if (opts.computed) initComputed(vm, opts.computed)
  if (opts.watch && opts.watch !== nativeWatch) {
    initWatch(vm, opts.watch)
  }
}
```

判断传入的 options 对象中是否有 props methods data computed watch 等属性

如果有的话， 会分别去 进行实例化操作。

#### initProps

```
function initProps (vm: Component, propsOptions: Object) {
     const propsData = vm.$options.propsData || {}
     ···
     ···
     defineReactive(props, key, value)
}
```

#### defineReactive

这个核心的代码，就是 Vue 的 实现数据监听以及 单向数据流的 主要方式。

```
export function defineReactive(obj, key, val, customSetter, shallow) {
  const dep = new Dep();

  const property = Object.getOwnPropertyDescriptor(obj, key);
  if (property && property.configurable === false) {
    return;
  }

  // cater for pre-defined getter/setters
  const getter = property && property.get;
  const setter = property && property.set;
  if ((!getter || setter) && arguments.length === 2) {
    val = obj[key];
  }

  let childOb = !shallow && observe(val);
  Object.defineProperty(obj, key, {
    enumerable: true,
    configurable: true,
    get: function reactiveGetter() {
      const value = getter ? getter.call(obj) : val;
      if (Dep.target) {
        dep.depend();
        if (childOb) {
          childOb.dep.depend();
          if (Array.isArray(value)) {
            dependArray(value);
          }
        }
      }
      return value;
    },
    set: function reactiveSetter(newVal) {
      const value = getter ? getter.call(obj) : val;
      /* eslint-disable no-self-compare */
      if (newVal === value || newVal !== newVal && value !== value) {
        return;
      }
      /* eslint-enable no-self-compare */
      if (process.env.NODE_ENV !== 'production' && customSetter) {
        customSetter();
      }
      if (setter) {
        setter.call(obj, newVal);
      } else {
        val = newVal;
      }
      childOb = !shallow && observe(newVal);
      dep.notify();
    }
  });
}

```
#### initMethods(vm, opts.methods)
```
for (const key in methods) {
    ...
    vm[key] = methods[key] == null ? noop : bind(methods[key], vm);
}
```
#### initData()
```
 observe(data, true /* asRootData */)

    ---- defineReactive()
```
#### initComputed(vm, opts.computed)

```
if (!isSSR) {
    // create internal watcher for the computed property.
    watchers[key] = new Watcher(
    vm,
    getter || noop,
    noop,
    computedWatcherOptions
    )
}
```


#### initWtach
```
function initWatch (vm: Component, watch: Object) {
  for (const key in watch) {
    const handler = watch[key]
    if (Array.isArray(handler)) {
      for (let i = 0; i < handler.length; i++) { //https://cn.vuejs.org/v2/api/#vm-watch 你可以传入回调数组，它们会被逐一调用
        createWatcher(vm, key, handler[i]) 
      }
    } else {
      createWatcher(vm, key, handler)
    }
  }
}

function createWatcher(vm, expOrFn, handler, options) {
  if (isPlainObject(handler)) {
    options = handler;
    handler = handler.handler;
  }
  if (typeof handler === 'string') {
    handler = vm[handler];
  }
  return vm.$watch(expOrFn, handler, options);
}
```





看到了 这里是不是 突然有种 豁然开朗的感觉， 各种属性方法的实例化，让我们比较清楚的知道了在

props methods data computed watch 等属性或者方法中 的内部变化