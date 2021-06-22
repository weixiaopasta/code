### redux-thunk - 中间件，处理异步action
需要处理异步action，出现了redux-thunk 

### Action
#### 和普通action的区别
redux-thunk处理的异步action是一个函数

action.js
```
// 其他同步的action省略，和上面代码一致

function fetchUser(id) {
  return (dispatch, getState, { api, whatever }) => {
    // you can use api and something else here
  }
}
/*
这里的action creator返回的不再是一个 { type: xxx, ... } 的对象，而是一个函数。使用和其他普通action creator并无差异。
dispatch(fetchUser(id))：传递的是一个函数，这个函数在中间件redux-thunk中会被识别，然后执行。
redux-thunk大概长这样：
(store) => (next) => (action) => {
    if (typeof action === 'function') {
        return action(store.dispatch, store.getState, ...someotherParams);
    }
    return next(action);
 } 
*/
```

### reducers.js

处理数据的形式同上面，也是很多字符串和switch...case...


### store.js
中间件的使用需要处理一下

```
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import rootReducer from './reducers';
// Note: this API requires redux@>=3.1.0
const store = createStore(
  rootReducer,
  applyMiddleware(thunk) // 此处需要额外应用中间件
);
export default store;
```


### redux-saga

使用generator进行异步流程控制

这个中间件就比较厉害了！专门定了一层saga抽象来处理异步流程，功能比较强大！

sagas.js

```
import { call, put, takeEvery, takeLatest } from 'redux-saga/effects'
import Api from '...'

function* fetchUser(action) {
   try {
      const user = yield call(Api.fetchUser, action.payload.userId);
      yield put({type: "USER_FETCH_SUCCEEDED", user: user});
   } catch (e) {
      yield put({type: "USER_FETCH_FAILED", message: e.message});
   }}

function* mySaga() {
  yield takeLatest("USER_FETCH_REQUESTED", fetchUser);
}
export default mySaga;
// 通过dispatch({type: xxx, payload: xxx})来触发对应的异步处理逻辑
// 比如 dispatch({type: 'USER_FETCH_REQUESTED', payload: '123'})会触发 fetchUser请求
// 然后通过提供的helper方法 put去分发 action 更新数据：put({type: "USER_FETCH_SUCCEEDED", user: user})

```

actions.js

action的形式同 原始的redux代码 的actions.js，这里就不再重复了。

reducers.js

reducer的处理逻辑的形式和 原始的redux代码 的reducers.js一样，也不重复写了。

store.js

中间件的使用都要先配置

```
import { createStore, applyMiddleware } from 'redux'
import createSagaMiddleware from 'redux-saga' // 引入saga 
import reducer from './reducers'
import mySaga from './sagas' // 引入saga异步处理js

const sagaMiddleware = createSagaMiddleware();// 此处创建saga
const store = createStore(
  reducer,
  applyMiddleware(sagaMiddleware) // 此处使用saga
); 
sagaMiddleware.run(mySaga); // 此处执行sage
export default store;
```

虽然异步处理能力变强了，但是action type的使用依旧没有变少，reducer里面也没什么变化，另外还增加了一层saga抽象和若干api...貌似让事情变复杂了

### Dva

dva 首先是一个基于 redux 和 redux-saga 的数据流方案，然后为了简化开发体验，dva 还额外内置了 react-router 和 fetch，所以也可以理解为一个轻量级的应用框架。

重要的模块(dva 通过 model 的概念把一个领域的模型管理起来，包含同步更新 state 的 reducers，处理异步逻辑的 effects，订阅数据源的 subscriptions )


https://blog.csdn.net/qq_39897978/article/details/106689915

###  Model对象相关

#### Model对象属性
1.namespace
当前Modal的名称，整个应用的state，由多少个小的Modal的State以nameSpace为key合成
2.state
该Modal当前的状态。数据都存在这里，直接决定了视图层的输出。
3.reducers
Action的处理器，处理同步动作，算出最新的State
4.effects
Action处理器，处理异步动作


#### call put

dva执行函数的方法

call 执行异步函数
put 发出一个Action， 类似于dispatch

#### reducer

reducer是一个Action处理器，用来处理同步操作，可以看做是state的计算器。它的作用是根据Action，从上一个State算出当前的State。


#### effects

Action处理器，处理异步动作，基于Redux-saga实现。

```
function *addAfter1Second(action, {put, call}){
  yield call(delay,1000);
  yield put({type: 'add'});
}
```

