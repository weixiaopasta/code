## 基本概念

### store
store就是保存数据的地方，你可以把它看成是一个容器。整个应用只能有一个store。
Redux 提供 createStore 这个函数，用来生成Store
```
import {createStore} from 'redux';

const store = createStore(fn);

const state = store.getState();
```

### state

state对象包含所有的数据。

当前时刻的state可以通过store.getState()拿到。


### Action


Action Creator在Redux中并没有一定要是个纯函数，只是不建议在里面直接运行有副作用的函数。请参考这篇在stackoverflow的Reduce作者的回答。



Action 返回的是一个对象，其中type是必须的，表示Action的名称。其他的可以自定义 - [可参考社区规范](https://github.com/redux-utilities/flux-standard-action)

```
const action = {
  type: 'ADD_TODO',
  payload: 'Learn Redux'
};

```

可以这样理解，Action 描述当前发生的事情。改变 State 的唯一办法，就是使用 Action。它会运送数据到 Store。



```
/* * action types */
export const ADD_TODO = 'ADD_TODO';
export const TOGGLE_TODO = 'TOGGLE_TODO';
export const SET_VISIBILITY_FILTER = 'SET_VISIBILITY_FILTER';

/* * action creators */
export function addTodo(text) {
  return { type: ADD_TODO, text }
};
export function toggleTodo(index) {
  return { type: TOGGLE_TODO, index }
};
export function setVisibilityFilter(filter) {
  return { type: SET_VISIBILITY_FILTER, filter }
};
// 通过dispatch来使用这些action对象
```

### store.dispatch 

发出action的唯一方法。

```
store.dispatch({
  type: 'ADD_TODO',
  payload: 'Learn Redux'
});
```

```
store.dispatch(addTodo('Learn something'))
```


### Reducer  是纯函数

Store 接收action后，必须给出一个新的state，这样View才会发生变化。 这种state的计算过程就叫Reducer。

reducer是一个函数，它接收action和当前的state作为参数，返回一个新的state。

```
const reducer = function (state, action) {
  // ...
  return new_state;
};
```

整个应用的初始状态，可以作为state的默认值。
```

const defaultState = 0;
const reducer = (state = defaultState, action) => {
  switch (action.type) {
    case 'ADD':
      return state + action.payload;
    default: 
      return state;
  }
};

const state = reducer(1, {
  type: 'ADD',
  payload: 2
});
```


上面代码中，reducer函数收到名为ADD的 Action 以后，就返回一个新的 State，作为加法的计算结果。其他运算的逻辑（比如减法），也可以根据 Action 的不同来实现。

实际应用中，Reducer 函数不用像上面这样手动调用，store.dispatch方法会触发 Reducer 的自动执行。为此，Store 需要知道 Reducer 函数，做法就是在生成 Store 的时候，将 Reducer 传入createStore方法。

```
import { createStore } from 'redux';
const store = createStore(reducer);
```

由于 Reducer 是纯函数，就可以保证同样的State，必定得到同样的 View。但也正因为这一点，Reducer 函数里面不能改变 State，必须返回一个全新的对象，

### store.subscribe

Store 允许使用store.subscribe方法设置监听函数，一旦 State 发生变化，就自动执行这个函数。
```
store.subscribe(listener);
```
显然，只要把 View 的更新函数（对于 React 项目，就是组件的render方法或setState方法）放入listen，就会实现 View 的自动渲染。


store.subscribe方法返回一个函数，调用这个函数就可以解除监听。
```

let unsubscribe = store.subscribe(() =>
  console.log(store.getState())
);

unsubscribe();
```

#### createStore方法的一个简单实现
```
const createStore = (reducer) => {
  let state;
  let listeners = [];

  const getState = () => state;

  const dispatch = (action) => {
    state = reducer(state, action);
    listeners.forEach(listener => listener());
  };

  const subscribe = (listener) => {
    listeners.push(listener);
    return () => {
      listeners = listeners.filter(l => l !== listener);
    }
  };

  dispatch({});

  return { getState, dispatch, subscribe };
};

```

### 工作流程

首先，用户发出 Action。

```
store.dispatch(action);
```
然后，Store 自动调用 Reducer，并且传入两个参数：当前 State 和收到的 Action。 Reducer 会返回新的 State 。

```
let nextState = todoApp(previousState, action);
State 一旦有变化，Store 就会调用监听函数。
```

// 设置监听函数

```
store.subscribe(listener);
```
listener可以通过store.getState()得到当前状态。如果使用的是 React，这时可以触发重新渲染 View。

```
function listerner() {
  let newState = store.getState();
  component.setState(newState);   
}
```