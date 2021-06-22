从表面上来说， store 注入和使用方式有一些区别 。在 Vuex 中， $store 被直接注入到了组件实例中 ，因此可以比较灵活的使用：

使用dispatch和commit提交更新
通过mapState或者直接通过this.$store来读取数据
在 Redux 中， 我们每一个组件都需要显示的用 connect 把需要的 props 和 dispatch 连接起来。

另外 Vuex 更加灵活一些， 组件中既可以 dispatch action 也可以 commit updates ，而 Redux 中只能进行 dispatch，并不能直接调用 reducer 进行修改。

从实现原理上来说，最大的区别是两点：

Redux 使用的是不可变数据，而Vuex的数据是可变的。 Redux每次都是用新的state替换旧的state，而Vuex是直接修改
Redux 在检测数据变化的时候，是通过 diff 的方式比较差异的，而Vuex其实和Vue的原理一样，是通过 getter/setter来比较的（如果看Vuex源码会知道，其实他内部直接创建一个Vue实例用来跟踪数据变化）