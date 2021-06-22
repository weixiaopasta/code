### 形式上

hash 模式url里边永远带着#号，开发中默认使用这个模式，如果用户考虑url的规范那么就需要使用history模式，因为history模式没有#号，是个正常的url，适合推广宣传；

### 功能上

比如我们在开发app的时候有分享页面，那么这个分享出去的页面就是用vue或是react做的，咱们把这个页面分享到第三方的app里，有的app里面url是不允许带有#号的，所以要将#号去除那么就要使用history模式，

但是使用history模式还有一个问题就是，在访问二级页面的时候，做刷新操作，会出现404错误，那么就需要和后端人配合，让他配置一下apache或是nginx的url重定向，重定向到你的首页路由上就ok了


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

### hash 改变请求是不会发送到服务端的，不会重新刷新页面，但是可以通过onhashchange事件监听到hash变化，从而触发组件的更新，绘制出相应的页面。

### history



history.pushState() 方法向当前浏览器会话的历史堆栈中添加一个状态


popstate事件
popstate事件会在点击后退、前进按钮(或调用history.back()、history.forward()、history.go()方法)时触发。前提是不能真的发生了页面跳转,而是在由history.pushState()或者history.replaceState()形成的历史节点中前进后退
注意:用history.pushState()或者history.replaceState()不会触发popstate事件。

