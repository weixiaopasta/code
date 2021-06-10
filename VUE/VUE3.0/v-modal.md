```
<ChildComponent v-model="pageTitle" />

<!-- 是以下的简写: -->

<ChildComponent
  :modelValue="pageTitle"
  @update:modelValue="pageTitle = $event"
/>
```
#### v-modal 参数
若需要更改 model 名称，而不是更改组件内的 model 选项，那么现在我们可以将一个 argument 传递给 model

```
<ChildComponent v-model:title="pageTitle" />

<!-- 是以下的简写: -->

<ChildComponent :title="pageTitle" @update:title="pageTitle = $event" />
```
### 应用
```
<ChildComponent v-model="pageTitle" />

```
### 组件内
```
// ChildComponent.vue

export default {
  props: {
    modelValue: String // 以前是`value：String`
  },
  emits: ['update:modelValue'],
  methods: {
    changePageTitle(title) {
      this.$emit('update:modelValue', title) // 以前是 `this.$emit('input', title)`
    }
  }
```
