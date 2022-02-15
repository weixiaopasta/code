#### loader
webpack 只能理解 JavaScript 和 JSON 文件，这是 webpack 开箱可用的自带能力。

loader 让 webpack 能够去处理其他类型的文件，并将它们转换为有效 模块，以供应用程序使用，以及被添加到依赖图中。

#### 在更高层面，在 webpack 的配置中，loader 有两个属性：

* test 属性，识别出哪些文件会被转换。
* use 属性，定义出在进行转换时，应该使用哪个 loader。

webpack.config.js

```
const path = require('path');

module.exports = {
  output: {
    filename: 'my-first-webpack.bundle.js',
  },
  module: {
    rules: [{ test: /\.txt$/, use: 'raw-loader' }],
  },
};
```
以上配置中，对一个单独的 module 对象定义了 rules 属性，里面包含两个必须属性：test 和 use。这告诉 webpack 编译器(compiler) 如下信息：
```
“嘿，webpack 编译器，当你碰到「在 require()/import 语句中被解析为 '.txt' 的路径」时，
在你对它打包之前，先 use(使用) raw-loader 转换一下。”
```


### ps：添加位置为module对象的rules数组中


### 注意：
```
请记住，使用正则表达式匹配文件时，你不要为它添加引号。
也就是说，/\.txt$/ 与 '/\.txt$/' 或 "/\.txt$/" 不一样。
前者指示 webpack 匹配任何以 .txt 结尾的文件，
后者指示 webpack 匹配具有绝对路径 '.txt' 的单个文件; 
这可能不符合你的意图。
```

### 插件(plugin)

loader 用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。包括：打包优化，资源管理，注入环境变量。

想要使用一个插件，你只需要 require() 它，然后把它添加到 plugins 数组中。多数插件可以通过选项(option)自定义。你也可以在一个配置文件中因为不同目的而多次使用同一个插件，这时需要通过使用 new 操作符来创建一个插件实例
```
webpack.config.js

const HtmlWebpackPlugin = require('html-webpack-plugin'); // 通过 npm 安装
const webpack = require('webpack'); // 用于访问内置插件

module.exports = {
  module: {
    rules: [{ test: /\.txt$/, use: 'raw-loader' }],
  },
  plugins: [new HtmlWebpackPlugin({ template: './src/index.html' })],
};
```

在上面的示例中，html-webpack-plugin 为应用程序生成一个 HTML 文件，并自动将生成的所有 bundle 注入到此文件中。

