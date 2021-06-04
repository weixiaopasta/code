webpack是用JS写的，运行在node环境，所以默认webpack打包的时候只会处理JS之间的依赖关系！！！

因为像 .css 这样的文件不是一个 JavaScript 模块，你需要配置 webpack 使用 css-loader 

css-loader会处理 import / require（） @import / url 引入的内容。
```
// base.css
.bg {
  background: #000;
}

const style = require('./base.css')
console.log(style, 'css')

```
<img src="https://img-blog.csdnimg.cn/20200228174101926.png">

但是这并不是我们想要的，因为是个数组，页面是无法直接使用，这时我们需要用到零外一个style-loader来处理。

style-loader 是通过一个JS脚本创建一个style标签，里面包含一些样式。style-loader是不能单独使用的，应为它并不负责解析 css 之前的依赖关系，每个loader的功能都是单一的，各自拆分独立。


#### 把通过loader处理好的css 提取出来的方法
##### webpack4 
ExtractTextPlugin = require('extract-text-webpack-plugin')
依赖style-loader
##### webpack5 中的  
https://github.com/webpack-contrib/mini-css-extract-plugin
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
仅需css-loader 不需要style-loader
