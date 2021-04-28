###  ios safari等 location 跳转问题
场景描述:

1.ios safari等 浏览器

2. 58M主站详情页跳转公共登录页面时，如果我们使用location.href实现登录页面跳转功能，当成功跳转登录页面后，如果不进行登录而是点击浏览器的后退按钮。=> 神奇的事情发生了: 页面会跳转到列表页(前提是我们是从列表页点击进入详情页的)
问题分析:

#### 原因： 应当是ios的兼容性问题导致location.href跳转的页面并没有被记录到history的栈当中，从而导致后退按钮直接跳转了history stack栈顶的列表页页面
解决方案:

使用
```
$(`<a href="${url}"></a>`).trigger('click');

```
代替之前的
```
location.href = url;
```