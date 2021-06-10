```
import { nextTick } from 'vue'

nextTick(()=>{
    //do something
})

```
### 受影响的 API

Vue 2.x 中的这些全局 API 受此更改的影响：
```
Vue.nextTick
Vue.observable (用 Vue.reactive 替换)
Vue.version
Vue.compile (仅全构建)
Vue.set (仅兼容构建)
Vue.delete (仅兼容构建)
```