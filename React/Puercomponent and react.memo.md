1. 一个给class 组件用 
2. 一个给function 组件使用
1、PuerComponent
当组件更新时，如果组件的 props 和 state 都没发生改变， render 方法就不会触发，省去 Virtual DOM 的生成和比对过程，达到提升性能的目的

总结下就是React.PureComponent是基于浅比较，所以只要属性值是引用类型，但是修改后的值变了，但是地址不变，也不会重新渲染。在深层数据结构发生变化时可以调用 forceUpdate() 来确保组件被正确地更新。也可以用 immutable 对象加速嵌套数据的比较。


PureComponent与shouldComponentUpdate 共存
经过上面的简单介绍，其实可以知道如果 PureComponent 里有 shouldComponentUpdate 函数的话，直接使用 shouldComponentUpdate 的结果作为是否更新的依据，没有 shouldComponentUpdate 函数的话，才会去判断是不是 PureComponent ，是的话再去做 shallowEqual 浅比较。


与老版本共存
```
import React { PureComponent, Component } from 'react';

class App extends (PureComponent || Component) {
  //...
}
```

### React.memo
https://zhuanlan.zhihu.com/p/105940433

React.memo() 可以支持指定一个参数，可以相当于 shouldComponentUpdate 的作用，因此 React.memo() 相对于 PureComponent 来说，用法更加方便。

2、React.memo 在性能优化上的实践
理解下面三个组件代码即可理解 React.memo 的使用：

index.js：父组件
Child.js：子组件
ChildMemo.js：使用 React.memo 包装过的子组件
1、index.js
父组件进行的逻辑很简单，就是引入两个子组件，并且将三个 state 通过 props 的方式传递给子组件

父组件本身进行的逻辑会进行三个 state 的变化

理论上，父组件每次变化一个 state 都通过 props 传递给了子组件，那子组件就会重新执行渲染。（无论子组件有没有真正用到这个 props）
```
import React, { useState, } from 'react';
import Child from './Child';
import ChildMemo from './Child-memo';

export default (props = {}) => {
    const [step, setStep] = useState(0);
    const [count, setCount] = useState(0);
    const [number, setNumber] = useState(0);

    const handleSetStep = () => {
        setStep(step + 1);
    }

    const handleSetCount = () => {
        setCount(count + 1);
    }

    const handleCalNumber = () => {
        setNumber(count + step);
    }


    return (
        <div>
            <button onClick={handleSetStep}>step is : {step} </button>
            <button onClick={handleSetCount}>count is : {count} </button>
            <button onClick={handleCalNumber}>numberis : {number} </button>
            <hr />
            <Child step={step} count={count} number={number} /> <hr />
            <ChildMemo step={step} count={count} number={number} />
        </div>
    );
}
```
2、Child.js
这个子组件本身没有任何逻辑，也没有任何包装，就是渲染了父组件传递过来的 props.number。

需要注意的是，子组件中并没有使用到 props.step 和 props.count，但是一旦 props.step 发生了变化就会触发重新渲染
```
import React from 'react';

export default (props = {}) => {
    console.log(`--- re-render ---`);
    return (
        <div>
            {/* <p>step is : {props.step}</p> */}
            {/* <p>count is : {props.count}</p> */}
            <p>number is : {props.number}</p>
        </div>
    );
};
```
3、ChildMemo.js
这个子组件使用了 React.memo 进行了包装，并且通过 isEqual 方法判断只有当两次 props 的 number 的时候才会重新触发渲染，否则 console.log 也不会执行。
```
import React, { memo, } from 'react';

const isEqual = (prevProps, nextProps) => {
    if (prevProps.number !== nextProps.number) {
        return false;
    }
    return true;
}

export default memo((props = {}) => {
    console.log(`--- memo re-render ---`);
    return (
        <div>
            {/* <p>step is : {props.step}</p> */}
            {/* <p>count is : {props.count}</p> */}
            <p>number is : {props.number}</p>
        </div>
    );
}, isEqual);
```

<img src ="https://pic2.zhimg.com/v2-f90f1e681746cba77bad4061d0335a1d_b.webp">


通过上图可以发现，在点击 step 和 count 的时候，props.step 和 props.count 都发生了变化，因此 Child.js 这个子组件每次都在重新执行渲染（----re-render----），即使没有用到这两个 props。

而这种情况下，ChildMemo.js 则不会重新进行 re-render。

只有当 props.number 发生变化的时候，ChildMemo.js 和 Child.js 表现是一致的。

从上面可以看出，React.memo() 的第二个方法在某种特定需求下，是必须存在的。 因为在实验的场景中，我们能够看得出来，即使我使用 React.memo 包装了 Child.js，也会一直触发重新渲染，因为 props 浅比较肯定是发生了变化。