https://www.jianshu.com/p/fac884647720

#### 引用传递（Ref forwading）是一种通过组件向子组件自动传递 引用ref 的技术。

* 例如某些input组件，需要控制其focus，本来是可以使用ref来控制，但是因为该input已被包裹在组件中，这时就需要使用Ref forward来透过组件获得该input的引用。


#### Refs 使用场景

    处理焦点、文本选择或者媒体的控制
    触发必要的动画
    集成第三方 DOM 库


1.基本的使用：通过组件获得input的引用

```
import React, { Component, createRef, forwardRef } from 'react';

const FocusInput = forwardRef((props, ref) => (
    <input type="text" ref={ref} />
));

class ForwardRef extends Component {
    constructor(props) {
        super(props);
        this.ref = createRef();
    }

    componentDidMount() {
        const { current } = this.ref;
        current.focus();
    }

    render() {
        return (
            <div>
                <p>forward ref</p>
                <FocusInput ref={this.ref} />
            </div>
        );
    }
}
export default ForwardRef;

```

