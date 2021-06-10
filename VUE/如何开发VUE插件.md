https://mp.weixin.qq.com/s?__biz=MzU5NDM5MDg1Mw==&mid=2247483874&idx=1&sn=ac6c9cf2629068dec3e5da8aa3e29364&chksm=fe00bbc8c97732dea7be43e903a794229876d8ab6c9381f2388ba22886fba7776b7b34b7af86&token=1885963052&lang=zh_CN#rd

1.普通的组件
<img src="https://mmbiz.qpic.cn/mmbiz_png/FaeDdIfeuq6NAyXZzGxib0Hk5qLDICokxOodBmY7z07gEBApLr43ly1V6HJ72nQb9Mfxlicq6YgXLUzCa7CiaciaVQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1">

2.为普通组件添加install方法并导出
<img src="https://mmbiz.qpic.cn/mmbiz_png/FaeDdIfeuq6NAyXZzGxib0Hk5qLDICokxPsMMlcREXR8ia7Lib0S8l8CQGQexhswBgTww3K2pfjOicdT6VlJCzszLw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1">

3. 使用

<img src="https://mmbiz.qpic.cn/mmbiz_png/FaeDdIfeuq6NAyXZzGxib0Hk5qLDICokxicLIpNfZej84Fjzm7J1h0P1oGpeGhdDiaGqdGWibCMZpXo0qNKEGNibfvg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1">

### 总结
创世按钮插件完成了，让我们再来理下思路：
创建一个插件(组件)
给该插件添加一个install方法，install方法里编写相关的注册逻辑，比如：把该插件注册为全局组件，这样就可以在整个项目中使用了。
在实例化Vue前，使用Vue.use(插件) 注册你的插件。