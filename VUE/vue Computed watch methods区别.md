#### 区别 
* computed是用来计算一个值的，使用时不需要加括号，可以直接当属性使用。computed拥有依赖缓存特性，如果依赖值不变，computed不会重新计算


*  watch是用来监听的，有两个选项，immediate 和 deep，当immediate: true时，表示会在第一次运行是执行这个函数，当 deep：true时，如果监听一个对象，会同时监听其内部属性。watch没有依赖缓存特性。




一、computed 和 methods

*  computed是计算属性，methods是方法，都可以实现对 data 中的数据加工后再输出。
*  不同的是 computed 计算属性是基于它们的依赖进行缓存的。计算属性 computed 只有在它的相关依赖发生改变时才会重新求值。这就意味着只要data 中的数据 message 还没有发生改变，多次访问 reversedMessage（对message 进行加工的处理函数） 计算属性会立即返回之前的计算结果，而不必再次执行函数。而对于method ，只要发生重新渲染，method 调用总会执行该函数。
*  当有一个性能开销比较大的的计算属性 A ，它需要遍历一个极大的数组和做大量的计算。然后我们可能有其他的计算属性依赖于 A ，这时候，我们就需要缓存。也就是使用 computed 而不是 methods。但对于每次都需要进行重新计算的属性比如下面这个函数的返回值 function () { return Date.now() } ，我们最好使用 methods。

#### 总之：数据量大，需要缓存的时候用 computed ；每次确实需要重新加载，不需要缓存时用 methods 。


#### watch 

computed 对于其中变量的依赖是多个的，它的函数中使用了多个 this.xxx ,只要其中一个发生了变化，都会触发这个函数。而 watch 的依赖则是单个的，它每次只可以对一个变量进行监控。 