### Before 

```
import Vue from 'vue'
import App from './App.vue'

Vue.config.ignoredElements = [/^app-/]
Vue.use(/* ... */)
Vue.mixin(/* ... */)
Vue.component(/* ... */)
Vue.directive(/* ... */)

Vue.prototype.customProperty = () => {}

new Vue({
  render: h => h(App)
}).$mount('#app')
```

### After

```
import { createApp } from 'vue'
import App from './App.vue'

const app = createApp(App)

app.config.isCustomElement = tag => tag.startsWith('app-')
// 任何以“app-”开头的元素都将被识别为自定义元素
app.use(/* ... */)
app.mixin(/* ... */)
app.component(/* ... */)
app.directive(/* ... */)

app.config.globalProperties.customProperty = () => {}
// globalProperties 添加可以在应用程序内的任何组件实例中访问的全局 property。属性名冲突时，组件的 property 将具有优先权。
app.mount(App, '#app')

```

#### 关于config
```
const app = Vue.createApp({})

app.config = {...}
```
config 是一个包含了 Vue 应用全局配置的对象。你可以在应用挂载前修改其以下 property：

https://vue3js.cn/docs/zh/api/application-config.html#errorhandler

####  挂载全局变量

before
```
    Vue.prototype.customProperty = () => {}
    或者：
    Object.defineProperty(vue.prototype,a,{
	    value:111
    })
```
after
```
    app.config.globalProperties.customProperty = () => {}
```