https://zhuanlan.zhihu.com/p/166087818

### Vuex
* Vuex 本质上是一个对象
* 有两个属性，一个是install方法，一个是Store这个类
* install方法的作用是将store这个实例挂在到所有的组件上，注意是同一个store实例。
* Store这个类拥有Commit,dispatch这些方法，Store类里将用户传入的state包装成data，作为new Vue的参数,从而实现了state值的响应式。

### 剖析Vuex本质

先抛出个问题，Vue项目中是怎么引入Vuex。

    1.安装Vuex，再通过import Vuex from 'vuex'引入
    2.先 var store = new Vuex.Store({...}),
    再把store作为参数的一个属性值，new Vue({store})，
    3.通过Vue.use(Vuex) 使得每个组件都可以拥有store实例

从这个引入过程我们可以发现什么？

我们是通过new Vuex.store({})获得一个store实例，也就是说，我们引入的Vuex中有Store这个类作为Vuex对象的一个属性。因为通过import引入的，实质上就是一个导出一个对象的引用。
所以我们可以初步假设
    
```
    Class Store{
    
    }

    let Vuex = {
        Store
    }
```

我们还使用了Vue.use(),而Vue.use的一个原则就是执行对象的install这个方法
所以，我们可以再一步 假设Vuex有有install这个方法。

```
Class Store{
  
}
let install = function(){
  
}

let Vuex = {
  Store,
  install
}
```
到这里，你能大概地将Vuex写出来吗？

很简单，就是将上面的Vuex对象导出，如下就是myVuex.js

```
//myVuex.js
class Store{

}
let install = function(){

}

let Vuex = {
    Store,
    install
}

export default Vuex
```

install方法我们可以这样完善

```
let install = function(Vue){
    Vue.mixin({
        beforeCreate(){
            if (this.$options && this.$options.store){ // 如果是根组件
                this.$store = this.$options.store
            }else { //如果是子组件
                this.$store = this.$parent && this.$parent.$store
            }
        }
    })
}
```

解释下代码：

1.参数Vue，我们在第四小节分析Vue.use的时候，再执行install的时候，将Vue作为参数传进去。
2.mixin的作用是将mixin的内容混合到Vue的初始参数options中。相信使用vue的同学应该使用过mixin了。
3.为什么是beforeCreate而不是created呢？因为如果是在created操作的话，$options已经初始化好了。
3.如果判断当前组件是根组件的话，就将我们传入的store挂在到根组件实例上，属性名为$store。
4.如果判断当前组件是子组件的话，就将我们根组件的$store也复制给子组件。注意是引用的复制，因此每个组件都拥有了同一个$store挂载在它身上。



### 这里有个问题，为什么判断当前组件是子组件，就可以直接从父组件拿到$store呢？这让我想起了曾经一个面试官问我的问题：父组件和子组件的执行顺序？

A：父beforeCreate-> 父created -> 父beforeMounte -> 子beforeCreate ->子create ->子beforeMount ->子 mounted -> 父mounted
可以得到，在执行子组件的beforeCreate的时候，父组件已经执行完beforeCreate了，那理所当然父组件已经有$store了。

### 实现Vuex的state

```
  <p>{{this.$store.state.num}}</p>
```


我们都知道，可以通过这个 语句获得 state的值 但是我们在Store类里还没实现，显然，现在就这样取得话肯定报错。

前面讲过，我们是这样使用Store的
```
export default new Vuex.Store({
  state: {
    num:0
  },
  mutations: {
  },
  actions: {
  },
  modules: {
  }
})
```
也就是说，我们把这个对象
```
{
  state: {
    num:0
  },
  mutations: {
  },
  actions: {
  },
  modules: {
  }
}
```
当作参数了。

那我们可以直接在Class Store里，获取这个对象
```
class Store{
    constructor(options){
        this.state = options.state || {}
        
    }
}
```

当然没有这么简单，我们忽略了一点，state里的值也是响应式的哦，我们这样可没有实现响应式。

曾经面试官问我Vuex和全局变量比有什么区别。这一点就是注意区别吧
那要怎么实现响应式呢？ 我们知道，我们new Vue（）的时候，传入的data是响应式的，那我们是不是可以new 一个Vue，然后把state当作data传入呢？ 没有错，就是这样。

```
class Store{

    constructor(options) {
        this.vm = new Vue({
            data:{
                state:options.state
            }
        })
    }

}
```
现在是实现响应式了，但是我们怎么获得state呢？好像只能通过this.$store.vm.state了？但是跟我们平时用的时候不一样，所以，是需要转化下的。

我们可以给Store类添加一个state属性。这个属性自动触发get接口。


```
class Store{

    constructor(options) {
        this.vm = new Vue({
            data:{
                state:options.state
            }
        })

    }
    //新增代码
    get state(){
        return this.vm.state
    }


}
```

这是ES6，的语法，有点类似于Object.defineProperty的get接口



#### 实现getter
```

//myVuex.js
class Store{

    constructor(options) {
        this.vm = new Vue({
            data:{
                state:options.state
            }
        })
        // 新增代码
        let getters = options.getter || {}
        this.getters = {}
        Object.keys(getters).forEach(getterName=>{
            Object.defineProperty(this.getters,getterName,{
                get:()=>{
                    return getters[getterName](this.state)
                }
            })
        })

    }
    get state(){
        return this.vm.state
    }
}

```

### 实现mutations

```
//myVuex.js
class Store{
    constructor(options) {
        this.vm = new Vue({
            data:{
                state:options.state
            }
        })

        let getters = options.getter || {}
        this.getters = {}
        Object.keys(getters).forEach(getterName=>{
            Object.defineProperty(this.getters,getterName,{
                get:()=>{
                    return getters[getterName](this.state)
                }
            })
        })
        
        let mutations = options.mutations || {}
        this.mutations = {}
        Object.keys(mutations).forEach(mutationName=>{
            this.mutations[mutationName] =  (arg)=> {
                mutations[mutationName](this.state,arg)
            }
        })

    }
    //新增代码
    commit(method,arg){
        this.mutations[method](arg)
    }
    get state(){
        return this.vm.state
    }
}
```