### 安装 

npm i vuex -s

### 使用

1. 在项目的根目录下新增一个store文件夹，在该文件夹内创建index.js

```
import Vue from 'vue'
import Vuex from 'vuex'

//挂载Vuex
Vue.use(Vuex)

//创建VueX对象
const store = new Vuex.Store({
    state:{
        //存放的键值对就是所要管理的状态
        name:'helloVueX'
    }
})

export default store
```
2. 将store挂载到当前项目的Vue实例当中去

```
import Vue from 'vue'
import App from './App'
import router from './router'
import store from './store'

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  store,  //store:store 和router一样，将我们创建的Vuex实例挂载到这个vue实例中
  render: h => h(App)
})

```

### VueX中的核心内容
在VueX对象中，其实不止有state,还有用来操作state中数据的方法集，以及当我们需要对state中的数据需要加工的方法集等等成员。

成员列表：
```
state 存放状态
mutations state成员操作
getters 加工state成员给外界
actions 异步操作
modules 模块化状态管理
```
###  VueX的工作流程
<img src="https://upload-images.jianshu.io/upload_images/16550832-20d0ad3c60a99111.png?imageMogr2/auto-orient/strip|imageView2/2/w/701/format/webp">

1. Vue组件如果调用某个VueX的方法过程中需要向后端请求时或者说出现异步操作时，需要dispatch VueX中actions的方法，以保证数据的同步。可以说，action的存在就是为了让mutations中的方法能在异步操作中起作用。

2. 如果没有异步操作，那么我们就可以直接在组件内提交状态中的Mutations中自己编写的方法来达成对state成员的操作。注意，1.3.3节中有提到，不建议在组件中直接对state中的成员进行操作，这是因为直接修改(例如：this.$store.state.name = 'hello')的话不能被VueDevtools所监控到。

最后被修改后的state成员会被渲染到组件的原位置当中去。


#### Mutations
mutations是操作state数据的方法的集合，比如对该数据的修改、增加、删除等等
* 接受参数 state payload
    * state:是当前VueX对象中的state
    * payload: 该方法在被调用时传递参数
```
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.store({
    state:{
        name:'helloVueX'
    },
    mutations:{
        //es6语法，等同edit:funcion(){...}
        edit(state, payload){
            state.name = 'jack'
        }
    }
})

export default store
```
* 使用方法
    this.$store.commit('edit')

#### Mutations使用及其传值的方式

 1. 单值提交
```
this.$store.commit('edit',15)
```
  2. 多值提交

  ```
  this.$store.commit('edit',{age:15,sex:'男'})

  ```
  3. 另一种提交数据的方式

```
this.$store.commit({
    type:'edit',
    payload:{
        age:15,
        sex:'男'
    }
})
```

### getters

对state成员加工后传递给外界
接收两个参数 
    1. state
    2、getters -- 用于将getters下的其他getter拿到
```
getters:{
    nameInfo(state){
        return "姓名:"+state.name
    },
    fullInfo(state,getters){
        return getters.nameInfo+'年龄:'+state.age
    }  
}
```
组件中调用
```
this.$store.getters.nameInfo
```

### Actions

由于直接在mutation方法中进行异步操作，将会引起数据失效。所以提供了Actions来专门进行异步操作，最终提交mutation方法。

Actions中的方法有两个默认参数

    context 上下文(相当于箭头函数中的this)对象
        * context 是个对象，里边有state，commit, getters, dispatch,rootState(module), rootGetters(module)
    payload 挂载参数

```
actions:{
    aEdit(context,payload){
        setTimeout(()=>{
            context.commit('edit',payload)
        },2000)
    }
}
```

在组件中调用:
```
this.$store.dispatch('aEdit',{age:15})
```

改进:

由于是异步操作，所以我们可以为我们的异步操作封装为一个Promise对象
```
    aEdit(context,payload){
        return new Promise((resolve,reject)=>{
            setTimeout(()=>{
                context.commit('edit',payload)
                resolve()
            },2000)
        })
    }
```

### modules

https://www.jianshu.com/p/a0c11ae01991

当项目庞大，状态非常多时，可以采用模块化管理模式。Vuex 允许我们将 store 分割成模块（module）。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割。

```
modules:{
    a:{
        state:{},
        getters:{},
        ....
    }
}
```
组件内调用模块a的状态：
```
this.$store.state.a
```

而提交或者dispatch某个方法和以前一样,会自动执行所有模块内的对应type的方法：
   
    在模块中，state 是被限制到模块的命名空间下，需要命名空间才能访问。 但actions 和mutations, 其实还有 getters 却没有被限制，在默认情况下，它们是注册到全局命名空间下的，所谓的注册到全局命名空间下，其实就是我们访问它们的方式和原来没有module 的时候是一样的


```
this.$store.commit('editKey')
this.$store.dispatch('aEditKey')
```

#### 模块的细节
 * 模块中mutations和getters中的方法接受的第一个参数是自身局部模块内部的state

* getters中方法的第三个参数是根节点状态

    * getters 参数 state(局部), getters（局部）, rootState,rootGetters
         ```
         rootState和 rootGetter参数顺序不要写反，一定是state在前，getter在后面，这是vuex的默认参数传递顺序， 可以打印出来看一下。
         ````
```
modules:{
    a:{
        state:{key:5},
        getters:{
            getKeyCount(state,getter,rootState){
                return  rootState.key + state.key
            }
        },
        ....
    }
}
```
* actions中方法获取局部模块状态是context.state,根节点状态是context.rootState
```
modules:{
    a:{
        state:{key:5},
        actions:{
            aEidtKey(context){
                if(context.state.key === context.rootState.key){
                    context.commit('editKey')
                }
            }
        },
        ....
    }
}
```


#### 模块的命名空间

```
// namespaced 属性，限定命名空间
export default {
  namespaced:true,
  state,
  mutations,
  actions,
  getters
}
```

当所有的actions, mutations, getters 都被限定到模块的命名空间下，我们dispatch actions, commit mutations 都需要用到命名空间。

    如 dispacth("changeName"), 就要变成 dispatch("login/changeName"); getters.localJobTitle 就要变成 getters["login/localJobTitle"]


```
<template>
 <div id="app">
  <img src="./assets/logo.png">
  <h1 @click ="alertName">{{useName}}</h1>
 
  <!-- 增加h2 展示 localJobTitle -->
  <h2>{{localJobTitle}}</h2>
  <!-- 添加按钮 -->
  <div>
   <button @click="changeName"> change to json</button>
  </div>
 </div>
</template>
 
<script>
import {mapActions, mapState,mapGetters} from "vuex";
export default {
 // computed属性，从store 中获取状态state，不要忘记login命名空间。
 computed: {
  ...mapState("login",{
   useName: state => state.useName
  }),
 
   localJobTitle() {
    return this.$store.getters["login/localJobTitle"]
   }
 },
 methods: {
  changeName() {
   this.$store.dispatch("login/changeName", "Jason")
  },
  alertName() {
   this.$store.dispatch("login/alertName")
  }
 }
}
</script>
```


有了命名空间之后，mapState, mapGetters, mapActions 函数也都有了一个参数，用于限定命名空间，每二个参数对象或数组中的属性，都映射到了当前命名空间中。

```
<script>
import {mapActions, mapState,mapGetters} from "vuex";
export default {
 computed: {
  // 对象中的state 和数组中的localJobTitle 都是和login中的参数一一对应。
  ...mapState("login",{
   useName: state => state.useName
  }),
  ...mapGetters("login", ["localJobTitle"])
 },
 methods: {
  changeName() {
   this.$store.dispatch("login/changeName", "Jason")
  },
  ...mapActions('login', ['alertName'])
 }
}
</script>
```


### Mutation_type

<img src="https://pic2.zhimg.com/v2-9f3637ad40371d0f9e2f2e17dc22afd2_r.jpg?source=1940ef5c">





### 关于为什么一定要在mutations中执行同步函数, actions 执行异步函数?

* 无论是actions 或者是 mutations 都可以写同步和异步函数，只是一种约定而已，说到底只是一个函数，你想在里面做什么都可以，严格模式下会有异常抛出。


* 尤大大  尤雨溪 的标准回答：


    文翻译可能有些偏差（不是我翻的）。区分 actions 和 mutations 并不是为了解决竞态问题，而是为了能用 devtools 追踪状态变化。事实上在 vuex 里面 actions 只是一个架构性的概念，并不是必须的，说到底只是一个函数，你在里面想干嘛都可以，只要最后触发 mutation 就行。异步竞态怎么处理那是用户自己的事情。vuex 真正限制你的只有 mutation 必须是同步的这一点（在 redux 里面就好像 reducer 必须同步返回下一个状态一样）。同步的意义在于这样每一个 mutation 执行完成后都可以对应到一个新的状态（和 reducer 一样），这样 devtools 就可以打个 snapshot 存下来，然后就可以随便 time-travel 了。如果你开着 devtool 调用一个异步的 action，你可以清楚地看到它所调用的 mutation 是何时被记录下来的，并且可以立刻查看它们对应的状态。其实我有个点子一直没时间做，那就是把记录下来的 mutations 做成类似 rx-marble 那样的时间线图，对于理解应用的异步状态变化很有帮助。

