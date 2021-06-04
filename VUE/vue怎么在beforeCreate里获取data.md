异步方式获取data。
`this.$nextTick`或者`setTimeout`都行。相当于在初始化前告诉容器，等全执行完了再跑里面的代码。这种方式别说拿data了，拿渲染完DOM都OK~
同步方式的话，是要了解框架内部原理的。在beforeCreate前，所有的options都会先存到vm.$options中，在beforeCreate之后，将$options里的data啦，props啦，methods啦等等一个个附到vm上，然后再触发created钩子。所以在beforeCreate的时候，通过this.message是拿不到值的，在created的时候就能通过this.message拿到值了。

一定要在beforeCreate的时候就同步去拿data里的值的话，就是直接从this.`$options.data`里去拿。如果data中的初始值是简单的string，那直接`this.$options.data()["message"]`就好.涉及到复杂点的情况，建议看看源码里是怎么处理的，具体在core/instance/render.js中的initRender(vm)里。

