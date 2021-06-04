http://www.dennisgo.cn/Articles/Vue/vueRouter.html

<img src="http://www.dennisgo.cn/images/Vue/VueRouter/image-20200119151237561.png">
###  vue-router的工作流程有如下几步

1. url改变
2. 触发监听事件
3. 改变vue-router里面的current变量
4. 监视current变量（变量的监视者）
5. 获取对应的组件
6. render新组件

### Vue-Router的路由模式有两种：hash和history，这两种模式的监听方法不一样

#### 监听url改变事件

* hash模式的值可以通过location.hash拿到，监听改变可以使用onhashchange事件；
* history的值可以用location.pathname拿到，可以用onpopstate事件来监听改变。