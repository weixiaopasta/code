https://blog.csdn.net/zemprogram/article/details/103186021?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-4.base&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-4.base


#### 拦截请求和响应及转换数据

```
// 将拦截器放在第一个，undefined放在第二个
// 在后面的处理中，是两个两个处理的
// 因为使用了promise.then，其第一个参数对应的是resolve第二个参数对应的是rejected
// 拦截器会对应于resolve，undefined会对应于rejected
var chain = [dispatchRequest, undefined];
var promise = Promise.resolve(config);
// 将请求拦截器中间件添加到chain数组头部
this.interceptors.request.forEach(function unshiftRequestInterceptors(interceptor) {
  chain.unshift(interceptor.fulfilled, interceptor.rejected);
});
// 将响应拦截器中间件添加到chain数组尾部
this.interceptors.response.forEach(function pushResponseInterceptors(interceptor) {
  chain.push(interceptor.fulfilled, interceptor.rejected);
});
// 依次使用promise处理chain数组内的东西
while (chain.length) {
  // 这里使用shift方法，每次调用两次这个方法，即是每次拿出数组中的两个拦截器
  promise = promise.then(chain.shift(), chain.shift());
}

```


### 缺点

不支持jsonp，需要自己封装
基于XMLHttpRequest实现，所以无法在service worker，web worker中使用