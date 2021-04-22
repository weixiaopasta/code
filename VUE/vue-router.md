####  vue的单页面应用是基于路由和组件的，路由用于设定访问路径，并将路径和组件映射起来。传统的页面应用，是用一些超链接来实现页面切换和跳转的。在vue-router单页面应用中，则是路径之间的切换，实际上就是组件的切换。

#### 记住参数或查询的改变并不会触发进入/离开的导航守卫。你可以通过观察 $route 对象来应对这些变化，或使用 beforeRouteUpdate 的组件内守卫。
####  安装vue-router模块

 ```
    npm install vue-router -save

 ```
#### vue-router有三个要素
    1. 路由map - 指路由与组件的映射关系
    2. 路由视图 - 指路由映射对应组件的渲染位置 <router-view></router-view>
    3. 路由导航 - 指可以使地址栏发生变化的导航链接
```
路由导航
方法1：使用<router-link> 创建 a 标签来定义导航链接
<router-link :to="{path:'apple'}"> to apple</router-link>
<router-link :to="{path:'banana'}">to apple</router-link>

方法2：使用 router.push 方法
router.push({ path: 'apple' })
```
``` 
import Vue from 'vue'   //引入Vue
import Router from 'vue-router'  //引入vue-router
import Hello from '@/components/Hello'  //引入根目录下的Hello.vue组件
 
Vue.use(Router)  // Vue全局使用Router
 
export default new Router({
  //以下是路由map
  routes: [              //配置路由，这里是个数组
    {                    //每一个链接都是一个对象
      path: '/',         //链接路径
      name: 'Hello',     //路由名称，
      component: Hello   //对应的组件模板
    }，{
      path:'/hi',
      component:Hi,
      children:[        //子路由,嵌套路由 （此处偷个懒，免得单独再列一点）
        {path:'/',component:Hi},
        {path:'hi1',component:Hi1},
        {path:'hi2',component:Hi2},
      ]
    }
  ]
})
```
### 动态路径参数
* 把某种模式匹配到的所有路由，全都映射到同个组件 
```
const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { path: '/user/:id', component: User }
  ]
})
```

一个『路径参数』使用冒号 : 标记。当匹配到一个路由时，参数值会被设置到this.$route.params，可以在每个组件内使用。
你可以在一个路由中设置多段『路径参数』，对应的值都会设置到 $route.params 中。例如：

<img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xMDg2ODQ0OS1iMTExOTM3ZGQzNGYyOWQ1LnBuZw?x-oss-process=image/format,png" alt="图片alt" title="图片title">

### 注意点

1、关于带参数的路由总结如下：

无论是直接路由“path" 还是命名路由“name”，带查询参数query，地址栏会变成“/url?查询参数名：查询参数值“;
直接路由“path" 带路由参数params params 不生效;
命名路由“name" 带路由参数params 地址栏保持是“/url/路由参数值”;

2、设置路由map里的path值：

 带路由参数params时，路由map里的path应该写成:  path:'/apple/:color' ;
 带查询参数query时，路由map里的path应该写成: path:'/apple' ；

3、获取参数方法：

在组件中：  {{$route.params.color}}
在js里： this.$route.params.color

### 响应路由参数的变化
 
* 当使用路由参数时，例如从 /user/foo 导航到 /user/bar，**原来的组件实例会被复用**。因为两个路由都渲染同个组件，比起销毁再创建，复用则显得更加高效。**不过，这也意味着组件的生命周期钩子不会再被调用**
    * 复用组件时，想对路由参数的变化作出响应的话，你可以简单地 watch (监测变化) $route 对象：  
   ```
   const User = {
        template: '...',
        watch: {
            $route(to, from) {
            // 对路由变化作出响应...
            }
        }
    } 

   ```  
    * 或者使用 2.2 中引入的 beforeRouteUpdate 导航守卫：
    ```
    const User = {
        template: '...',
        beforeRouteUpdate(to, from, next) {
            // react to route changes...
            // don't forget to call next()
        }
    }
    ```
### 捕获所有路由或 404 Not found 路由
* 常规参数只会匹配被 / 分隔的 URL 片段中的字符。如果想匹配任意路径，我们可以使用通配符 (*)：
```
{
  // 会匹配所有路径
  path: '*'
}
{
  // 会匹配以 `/user-` 开头的任意路径
  path: '/user-*'
}
```
* 常规参数只会匹配被 / 分隔的 URL 片段中的字符。如果想匹配任意路径，我们可以使用通配符 (*)：

    * 当使用一个通配符时，$route.params 内会自动添加一个名为 pathMatch 参数。它包含了 URL 通过通配符被匹配的部分：
    ```
    // 给出一个路由 { path: '/user-*' }
    this.$router.push('/user-admin')
    this.$route.params.pathMatch // 'admin'
    // 给出一个路由 { path: '*' }
    this.$router.push('/non-existing')
    this.$route.params.pathMatch // '/non-existing'
    ```

### router-link制作导航
```
// 字符串
<router-link to="apple"> to apple</router-link>
// 对象
<router-link :to="{path:'apple'}"> to apple</router-link>
// 命名路由
<router-link :to="{name: 'applename'}"> to apple</router-link>
//直接路由带查询参数query，地址栏变成 /apple?color=red
<router-link :to="{path: 'apple', query: {color: 'red' }}"> to apple</router-link>
// 命名路由带查询参数query，地址栏变成/apple?color=red
<router-link :to="{name: 'applename', query: {color: 'red' }}"> to apple</router-link>
//直接路由带路由参数params，params 不生效，如果提供了 path，params 会被忽略
<router-link :to="{path: 'apple', params: { color: 'red' }}"> to apple</router-link>
// 命名路由带路由参数params，地址栏是/apple/red
<router-link :to="{name: 'applename', params: { color: 'red' }}"> to apple</router-link>
```

#### 1. router-link 是一个组件，它默认会被渲染成一个带有链接的a标签，通过to属性指定链接地址。

    注意：被选中的router-link将自动添加一个class属性值.router-link-active。
```
<router-link to="/">[text]</router-link>
```
####  2.router-view 用于渲染匹配到的组件。

    ①.可以给router-view组件设置transition过渡

```
<transition name="fade">
  <router-view ></router-view>
</transition>
```

    ②.还可以配合<keep-alive>使用，keep-alive可以缓存数据，这样不至于重新渲染路由组件的时候，之前那个路由组件的数据被清除了。比如对当前的路由组件a进行了一些DOM操作之后，点击进入另一个路由组件b，再回到路由组件a的时候之前的DOM操作还保存在，如果不加keep-alive再回到路由组件a时，之前的DOM操作就没有了，得重新进行。如果你的应用里有一个购物车组件，就需要用到keep-alive。
```
    <transition name="slide" mode="out-in">
        <keep-alive>
            <router-view></router-view>
        </keep-alive>
    </transition>
```
#### mode - 过渡模式 - 三种
    * default – 进入和离开过渡同时发生

    * in-out – 新元素的过渡先进入。然后，当前元素过渡出去。

    * out-in - 当前元素先过渡出去。然后，新元素过渡进来。






### $router 访问路由实例

#### router.push(location, onComplete?, onAbort?)
* 这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的 URL。

* 该方法的参数可以是一个字符串路径，或者一个描述地址的对象。例如
```
// 字符串
router.push('home')

// 对象
router.push({ path: 'home' })

// 命名的路由
router.push({ name: 'user', params: { userId: '123' }})

// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})

```

#### 注意：如果提供了 path，params 会被忽略，上述例子中的 query 并不属于这种情况。取而代之的是下面例子的做法，你需要提供路由的 name 或手写完整的带有参数的 path：
```
const userId = '123'
router.push({ name: 'user', params: { userId }}) // -> /user/123
router.push({ path: `/user/${userId}` }) // -> /user/123
// 这里的 params 不生效
router.push({ path: '/user', params: { userId }}) // -> /user
```

#### 注意： 如果目的地和当前路由相同，只有参数发生了改变 (比如从一个用户资料到另一个 /users/1 -> /users/2)，你需要使用 beforeRouteUpdate 来响应这个变化 (比如抓取用户信息)。

### router.go(n)
* 这个方法的参数是一个整数，意思是在 history 记录中向前或者后退多少步，类似 window.history.go(n)。

```
// 在浏览器记录中前进一步，等同于 history.forward()
router.go(1)

// 后退一步记录，等同于 history.back()
router.go(-1)

// 前进 3 步记录
router.go(3)

// 如果 history 记录不够用，那就默默地失败呗
router.go(-100)
router.go(100)
```

### 实现不同路由不同页面标题
```
// 定义路由的时候如下定义，name也可为中文
const routes = [
  { path: '/goods', component: goods, name: 'goods' },
  { path: '/orders', component: orders, name: 'orders' },
  { path: '/seller', component: seller, name: 'seller' }
];
// 创建路由实例
const router = new VueRouter({
  routes: routes
})
// 关键在这里，设置afterEach钩子函数
router.afterEach((to, from, next) => {
  document.title = to.name;
})
```


### 命名视图

* 有时候想同时 (同级) 展示多个视图，而不是嵌套展示，例如创建一个布局，有 sidebar (侧导航) 和 main (主内容) 两个视图，这个时候命名视图就派上用场了。你可以在界面中拥有多个单独命名的视图，而不是只有一个单独的出口。如果 router-view 没有设置名字，那么默认为 default。

```
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>

```
一个视图使用一个组件渲染，因此对于同个路由，多个视图就需要多个组件。确保正确使用 components 配置 (带上 s)：
```
const router = new VueRouter({
  routes: [
    {
      path: '/',
      components: {
        default: Foo,
        a: Bar,
        b: Baz
      }
    }
  ]
})
```

### vue-router别名alias
```
const router = new VueRouter({
  routes: [
    { path: '/a', component: A, alias: '/b' }
  ]
})
```
* /a 的别名是 /b，意味着，当用户访问 /b 时，URL 会保持为 /b，但是路由匹配则为 /a，就像用户访问 /a 一样。
* “别名”的功能让你可以自由地将 UI 结构映射到任意的 URL，而不是受限于配置的嵌套路由结构。
```
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: '/b' }
  ]
})

```
* “重定向”的意思是，当用户访问 /a时，URL 将会被替换成 /b，然后匹配路由为 /b，那么“别名”又是什么呢？


### 路由组件传参
    * 布尔模式
    * 对象模式
    * 函数模式
* 使用 props 将组件和路由解耦：
    * 取代与 $route 的耦合
    ```
    const User = {
        template: '<div>User {{ $route.params.id }}</div>'
    }
    const router = new VueRouter({
        routes: [{ path: '/user/:id', component: User }]
    })
    ```
    * 通过 props 解耦
        * 注意区分component和components的区别
    ```
    const User = {
        props: ['id'],
        template: '<div>User {{ id }}</div>'
    }
    const router = new VueRouter({
        routes: [
            { path: '/user/:id', component: User, props: true }, //布尔模式

            // 对于包含命名视图的路由，你必须分别为每个命名视图添加 `props` 选项：
            {
            path: '/user/:id',
            components: { default: User, sidebar: Sidebar },
            props: { default: true, sidebar: false } // 对象模式
            }
        ]
    })

    const router = new VueRouter({
        routes: [
            {
            path: '/search',
            component: SearchUser,
            props: route => ({ query: route.query.q })
            } // 函数模式
        ]
        })
    ```


    ### 完整的导航解析流程

    https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#%E5%AE%8C%E6%95%B4%E7%9A%84%E5%AF%BC%E8%88%AA%E8%A7%A3%E6%9E%90%E6%B5%81%E7%A8%8B

        导航被触发。
        在失活的组件里调用 beforeRouteLeave 守卫。
        调用全局的 beforeEach 守卫。
        在重用的组件里调用 beforeRouteUpdate 守卫 (2.2+)。
        在路由配置里调用 beforeEnter。
        解析异步路由组件。
        在被激活的组件里调用 beforeRouteEnter。
        调用全局的 beforeResolve 守卫 (2.5+)。
        导航被确认。
        调用全局的 afterEach 钩子。
        触发 DOM 更新。
        调用 beforeRouteEnter 守卫中传给 next 的回调函数，创建好的组件实例会作为回调函数的参数传入。

### 路由元信息
* 定义路由的时候可以配置 meta 字段：
```
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      children: [
        {
          path: 'bar',
          component: Bar,
          // a meta field
          meta: { requiresAuth: true }
        }
      ]
    }
  ]
})
```
那么如何访问这个 meta 字段呢？

首先，我们称呼 routes 配置中的每个路由对象为 路由记录。路由记录可以是嵌套的，因此，当一个路由匹配成功后，他可能匹配多个路由记录

例如，根据上面的路由配置，/foo/bar 这个 URL 将会匹配父路由记录以及子路由记录。

一个路由匹配到的所有路由记录会暴露为 $route 对象 (还有在导航守卫中的路由对象) 的 $route.matched 数组。因此，我们需要遍历 $route.matched 来检查路由记录中的 meta 字段。


下面例子展示在全局导航守卫中检查元字段：
```
router.beforeEach((to, from, next) => {
  if (to.matched.some(record => record.meta.requiresAuth)) {
    // this route requires auth, check if logged in
    // if not, redirect to login page.
    if (!auth.loggedIn()) {
      next({
        path: '/login',
        query: { redirect: to.fullPath }
      })
    } else {
      next()
    }
  } else {
    next() // 确保一定要调用 next()
  }
})
```


### 路由对象

* 一个路由对象 (route object) 表示当前激活的路由的状态信息，包含了当前 URL 解析得到的信息，还有 URL 匹配到的路由记录 (route records)。
*  路由对象是不可变 (immutable) 的，每次成功的导航后都会产生一个新的对象。
* 路由对象出现在多个地方:

    * 在组件内，即 this.$route

    * 在 $route 观察者回调内

    * router.match(location) 的返回值
    #### 路由对象属性
     1. $route.path
     2. $route.params
     3. $route.query
     4. $route.hash 
     5. $route.fullPath
     6. $route.matched
     7. $route.name
     8. $route.redirectedFrom


### 数据获取

* 导航完成后获取数据
    * 组件的created 或者mounted中获取数据
* 在导航完成前获取数据
    ##### 用户会停留在当前的界面，因此建议在数据获取期间，显示一些进度条或者别的指示。如果数据获取失败，同样有必要展示一些全局的错误提醒。

```
export default {
  data () {
    return {
      post: null,
      error: null
    }
  },
  beforeRouteEnter (to, from, next) {
    getPost(to.params.id, (err, post) => {
      next(vm => vm.setData(err, post))
    })
  },
  // 路由改变前，组件就已经渲染完了
  // 逻辑稍稍不同
  beforeRouteUpdate (to, from, next) {
    this.post = null
    getPost(to.params.id, (err, post) => {
      this.setData(err, post)
      next()
    })
  },
  methods: {
    setData (err, post) {
      if (err) {
        this.error = err.toString()
      } else {
        this.post = post
      }
    }
  }
}
```
### 路由懒加载
1.首先，可以将异步组件定义为返回一个 Promise 的工厂函数 (该函数返回的 Promise 应该 resolve 组件本身)
```
const Foo = () =>
  Promise.resolve({
    /* 组件定义对象 */
  })
```
2.第二，在 Webpack 2 中，我们可以使用动态 import语法来定义代码分块点 (split point)：

```
import('./Foo.vue') // 返回 Promise
```
#### 注意: 如果您使用的是 Babel，你将需要添加 syntax-dynamic-import(语法动态导入) 插件，才能使 Babel 可以正确地解析语法。

在路由配置中什么都不需要改变，只需要像往常一样使用 Foo：

```
const router = new VueRouter({
  routes: [{ path: '/foo', component: Foo }]
})
```

### 把组件按组分块
* 有时候我们想把某个路由下的所有组件都打包在同个异步块 (chunk) 中。只需要使用 命名 chunk，一个特殊的注释语法来提供 chunk name (需要 Webpack > 2.4)。
```
const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
```
Webpack 会将任何一个异步模块与相同的块名称组合到相同的异步块中。

### 检测导航故障

https://router.vuejs.org/zh/guide/advanced/navigation-failures.html#%E6%A3%80%E6%B5%8B%E5%AF%BC%E8%88%AA%E6%95%85%E9%9A%9C