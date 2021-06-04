https://www.jianshu.com/p/07546bee4a7c
* useImperativeHandle最主要接收两个参数
    1. ref：；
    2. 回调函数：要求有一个返回值，返回值一般为对象；


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