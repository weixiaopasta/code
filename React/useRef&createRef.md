正如官网说的, 它像一个变量, 类似于 this , 它就像一个盒子, 你可以存放任何东西. createRef 每次渲染都会返回一个新的引用，而 useRef 每次都会返回相同的引用。
https://zhuanlan.zhihu.com/p/105276393


```
import logo from './logo.svg';
import './App.css';
import React, { useState, useEffect, useCallback, createRef, useRef} from 'react'

function App() {
    const [count, setCount] = useState(0);
    const refFromUseRef = useRef();
    const refFromCreateRef = createRef();
    const handleAlertClick = ()=>{
      setTimeout(()=>{
        console.log('refFromUseRef count: ', refFromUseRef.current)  // 实时 
        console.log('refFromCreateRef count: ', refFromCreateRef.current)
      }, 2000)
    }
    useEffect(()=>{
      refFromUseRef.current = count; 
      refFromCreateRef.current = count;
    })
    
  
    return (
      <div> {count} 
      <button onClick={()=>setCount(count+1) }> click me </button> 
      <button onClick={handleAlertClick }> Alert </button> </div>
    );
}
export default App;

```


# 获取前一次的值

```
function App() {
    const [count, setCount] = useState(0);
    const refFromUseRef = useRef();
    useEffect(()=>{
      refFromUseRef.current = count
    },[count])
    
  
    return (
      <div> 
        <p>refFromUseRef.current : {refFromUseRef.current}</p>
        <p>count : {count}</p>
        <button onClick={()=>setCount(count+1) }> click me </button> 
      </div>
    );
}
export default App;

```