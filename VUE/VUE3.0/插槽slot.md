1.接受插槽的组件定义

```
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```
2. 使用组件 - 插槽传值

我们可以在一个 <template> 元素上使用 v-slot 指令
```
<base-layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <template v-slot:default>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </template>

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>

```



### 作用域插槽

让插槽内容能够访问子组件中才有的数据是很有用

1. 接收插槽的组件传值的组件定义

```
app.component('todo-list', {
  data() {
    return {
      items: ['Feed a cat', 'Buy milk']
    }
  },
  template: `
    <ul>
      <li v-for="(item, index) in items">
        <slot :item="item"></slot>
      </li>
    </ul>
  `
})
```

2. 使用

在这个例子中，我们选择将包含所有插槽 prop 的对象命名为 slotProps，但你也可以使用任意你喜欢的名字。
```
<todo-list v-slot:default="slotProps">
  <i class="fas fa-check"></i>
  <span class="green">{{ slotProps.item }}</span>
</todo-list>
```
或

```
<todo-list v-slot="slotProps">
  <i class="fas fa-check"></i>
  <span class="green">{{ slotProps.item }}</span>
</todo-list>
```


### 多个插槽
只要出现多个插槽，请始终为所有的插槽使用完整的基于 <template> 的语法

```
<todo-list>
  <template v-slot:default="slotProps">
    <i class="fas fa-check"></i>
    <span class="green">{{ slotProps.item }}</span>
  </template>

  <template v-slot:other="otherSlotProps">
    ...
  </template>
</todo-list>
```

### 具名插槽的缩写

```
<base-layout>
  <template #header>
    <h1>Here might be a page title</h1>
  </template>

  <template #default>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </template>

  <template #footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>

```