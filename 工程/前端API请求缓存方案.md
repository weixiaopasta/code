https://mp.weixin.qq.com/s?__biz=MzAxODE2MjM1MA==&mid=2651563582&idx=1&sn=38f712b34b363eb1db56bbcda4d201c3&chksm=802571ffb752f8e95fdda74c8c3393c01d6f36ee71952b2c9bc0a4cd1072098a69b6ed20f6da&scene=21#wechat_redirect


在开发 web 应用程序时，性能都是必不可少的话题。对于webpack打包的单页面应用程序而言，我们可以采用很多方式来对性能进行优化，比方说 tree-shaking、模块懒加载、利用 extrens 网络cdn 加速这些常规的优化。

甚至在vue-cli 项目中我们可以使用 --modern 指令生成新旧两份浏览器代码来对程序进行优化。
而事实上，缓存一定是提升web应用程序有效方法之一，尤其是用户受限于网速的情况下。提升系统的响应能力，降低网络的消耗。
当然，内容越接近于用户，则缓存的速度就会越快，缓存的有效性则会越高。
以客户端而言，我们有很多缓存数据与资源的方法，例如 标准的浏览器缓存 以及 目前火热的 Service worker。
但是，他们更适合静态内容的缓存。例如 html，js，css以及图片等文件。而缓存系统数据，我采用另外的方案。
那我现在就对我应用到项目中的各种 api 请求方案，从简单到复杂依次介绍一下。


#### 方案一、 数据缓存

简单的 数据 缓存，第一次请求时候获取数据，之后便使用数据，不再请求后端api。 
代码如下：

#### 方案二、 promise缓存
方案一本身是不足的。因为如果考虑同时两个以上的调用此 api，会因为请求未返回而进行第二次请求api。

当然，如果你在系统中添加类似于 vuex、redux这样的单一数据源框架，这样的问题不太会遇到，但是有时候我们想在各个复杂组件分别调用api，而不想对组件进行组件通信数据时候，便会遇到此场景。

```

const promiseCache = new Map()

getWares() {
    const key = 'wares'
    let promise = promiseCache.get(key);
    // 当前promise缓存中没有 该promise
    if (!promise) {
        promise = request.get('/getWares').then(res => {
            // 对res 进行操作
            ...
        }).catch(error => {
            // 在请求回来后，如果出现问题，把promise从cache中删除 以避免第二次请求继续出错S
            promiseCache.delete(key)
            return Promise.reject(error)
        })
    }
    // 返回promise
    return promise
}
```
该代码避免了方案一的同一时间多次请求的问题。同时也在后端出错的情况下对promise进行了删除，不会出现缓存了错误的promise就一直出错的问题。

调用方式:
```
getWares().then( ... )
// 第二次调用 取得先前的promise
getWares().then( ... )
```

####  方案三、 多promise 缓存

```

const querys ={
    wares: 'getWares',
    skus: 'getSku'
}
const promiseCache = new Map()

async queryAll(queryApiName) {
    // 判断传入的数据是否是数组
    const queryIsArray = Array.isArray(queryApiName)
    // 统一化处理数据，无论是字符串还是数组均视为数组
    const apis = queryIsArray ? queryApiName : [queryApiName]

    // 获取所有的 请求服务
    const promiseApi = []

    apis.forEach(api => {
        // 利用promise 
        let promise = promiseCache.get(api)

        if (promise) {
            // 如果 缓存中有，直接push
            promiseApi.push(promise)
        } else {
             promise = request.get(querys[api]).then(res => {
                // 对res 进行操作
                ...
                }).catch(error => {
                // 在请求回来后，如果出现问题，把promise从cache中删除
                promiseCache.delete(api)
                return Promise.reject(error)
            })
            promiseCache.set(api, promise)
            promiseApi.push(promise)
        }
    })
    return Promise.all(promiseApi).then(res => {
        // 根据传入的 是字符串还是数组来返回数据，因为本身都是数组操作
        // 如果传入的是字符串，则需要取出操作
        return queryIsArray ? res : res[0]
    })
}
```

该方案是同时获取多个服务器数据的方式。可以同时获得多个数据进行操作，不会因为单个数据出现问题而发生错误。

```
queryAll('wares').then( ... )
// 第二次调用 不会去取 wares，只会去skus
queryAll(['wares', 'skus']).then( ... )
```


####  方案四 、添加时间有关的缓