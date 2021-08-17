### setup 函数可以说是 Vue 3 一个入口函数。
### 参数
    使用 setup 函数时，它将接受两个参数：
    1.props
    2.context

###  setup 函数中的 props 是响应式的，当传入新的 prop 时，它将被更新。

    因为 props 是响应式的，你不能使用 ES6 解构，因为它会消除 prop 的响应性

    如果需要解构 prop，可以通过使用 setup 函数中的 toRefs 来安全地完成此操作。

    ```
    setup(props) {
        const counter = ref(0)
        console.log('props===>', props)
        const {name} = toRefs(props.test)
        return {
        counter,
        name
        }
    }
    ```
### 上下文 - context
* 传递给 setup 函数的第二个参数是 context。context 是一个普通的 JavaScript 对象，它暴露三个组件的 property： attrs, slots, emit 
```
setup(props, { attrs, slots, emit }) {
    ...
}
```
* context 是一个普通的 JavaScript 对象，也就是说，它不是响应式的，这意味着你可以安全地对 context 使用 ES6 解构。

### *注意
* attrs 和 slots 是有状态的对象，它们总是会随组件本身的更新而更新。这意味着你应该避免对它们进行解构，并始终以 attrs.x 或 slots.x 的方式引用 property。

* 请注意，与 props 不同，attrs 和 slots 是非响应式的。如果你打算根据 attrs 或 slots 更改应用副作用，那么应该在 onUpdated 生命周期钩子中执行此操作。


### 访问组件的 property
    执行 setup 时，组件实例尚未被创建。因此，你只能访问以下 property：

    props
    attrs
    slots
    emit
    换句话说，你将无法访问以下组件选项：

    data
    computed
    methods
### 结合模板使用
如果 setup 返回一个对象，则可以在组件的模板中像传递给 setup 的 props property 一样访问该对象的 property：
```
<template>
  <div class="template-m-wrap" title="hhhhh">
    <div>{{ readersNumber }} {{ book.title }}</div>
  </div>
</template>
<script>
import { defineComponent, ref, reactive } from 'vue'
export default defineComponent({
  name: 'TemplateM',
  setup() {
    const readersNumber = ref(0)
    const book = reactive({ title: 'Vue 3 Guide' })

    // expose to template
    return {
      readersNumber,
      book,
    }
  },
})
</script>
```

### 注意，从 setup 返回的 refs 在模板中访问时是被自动解开的，因此不应在模板中使用 .value。
