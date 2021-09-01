https://www.jianshu.com/p/92def9e95af5

####  forwardRef
forwardRef的做法本身没有什么问题, 但是我们是将子组件的DOM直接暴露给了父组件:

直接暴露给父组件带来的问题是某些情况的不可控
父组件可以拿到DOM后进行任意的操作
我们只是希望父组件可以操作的focus，其他并不希望它随意操作其他方法

#### useImperativeHandle

#### 作用
减少暴露给父组件获取的DOM元素属性, 只暴露给父组件需要用到的DOM方法

通过useImperativeHandle可以只暴露特定的操作

1.通过useImperativeHandle的Hook, 将父组件传入的ref和useImperativeHandle第二个参数返回的对象绑定到了一起
2.所以在父组件中, 调用inputRef.current时, 实际上是返回的对象
#### 参数
* useImperativeHandle最主要接收两个参数
    1. ref;
    2. 回调函数：要求有一个返回值，返回值一般为对象;


```
import React, { useRef, forwardRef, useImperativeHandle } from 'react'
const HYInput = forwardRef((props, ref) => {
  // inputRef在整个生命周期中应该是不变的
  const inputRef = useRef();
  useImperativeHandle(ref, () => ({
    focus: () => {
      inputRef.current.focus();
    }
  }), [inputRef.current]);
  return <input ref={inputRef} type="text" />
})
export default function UseImperativeHandleHookDemo() {
  const inputRef = useRef();
  return (
    <div>
      <HYInput ref={inputRef} />
      <button onClick={e => inputRef.current.focus()}>聚焦</button>
    </div>
  )
}
```
一般来说，只要用到forwardRef，那么最好搭配该hook一起使用。