【实战】Axios取消请求 - CancelToken

https://www.jianshu.com/p/1662e2524c64

```
import request from '@/utils/request'
import axios from 'axios'

export function queryUserInfo(data, _this) {
  const CancelToken= axios.CancelToken
  return request({
    url: '/users/userInfo',
    method: 'get',
    params: data,
    cancelToken: new CancelToken(function executor(c) {
      _this.cancelAjax = c
    })
  })
}
```
  以上我们利用executor生成了取消函数，并赋值给外部真实请求传入的环境中，形参_this的实参为Vue的this变量。

```
// 选择人员列表查询
getUserInfo() {
  if (typeof this.cancelAjax === 'function') {
    this.cancelAjax()
  }
  this.allUsers.loading = true
  queryUserInfo({ userId: '20192430139' }, this).then(res => {
    if (res.code === 1) {
      this.userInfo.data = res.data
      this.allUsers.loading = false
    } else {
      this.$message.error(res.msg || 'get userInfo error')
    }
  })
}
```

 思路就是，当请求发起时cancelToken会派发函数给data中的cancelAjax，可以利用数据类型侦查是否请求正在发生。cancelAjax在页面初始化时定义为null即可

 ```
 <template>

</template>
<script>
export default {
  data() {
    return {
      cancelAjax: null
    }
  }
}
</script>
 ```