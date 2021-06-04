https://blog.csdn.net/zjk325698/article/details/112226422

简单点说，就是 computed 在运行的时候，首先将全局的 Dep.target 设置当前 computed 的 watcher，然后在运行 computed 代码，里面用到的数据会调用它们自己的 get 方法，在 get 方法里，他们会将自己的 dep.id 存到当前的 Dep.target 里，然后还要将当前的 Dep.target（也就是当前的 watcher） 存到自己的 dep.subs 属性 中，每当自己数据更新触发 set 方法，就会把自己 dep.subs 中的每个watcher 拿出来进行数据更新，从而更新 computed 。
