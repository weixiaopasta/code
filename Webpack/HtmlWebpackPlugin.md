#### HtmlWebpackPlugin
HtmlWebpackPlugin 简化了 HTML 文件的创建，以便为你的 webpack 包提供服务。这对于那些文件名中包含哈希值，并且哈希值会随着每次编译而改变的 webpack 包特别有用。你可以让该插件为你生成一个 HTML 文件，使用 lodash 模板提供模板，或者使用你自己的 loader。

```
npm install --save-dev html-webpack-plugin

```

该插件将为你生成一个 HTML5 文件， 在 body 中使用 script 标签引入你所有 webpack 生成的 bundle。 只需添加该插件到你的 webpack 配置中，如下所示

```
const HtmlWebpackPlugin = require('html-webpack-plugin');
const path = require('path');

module.exports = {
  entry: 'index.js',
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'index_bundle.js',
  },
  plugins: [new HtmlWebpackPlugin()],
};
```

这将会生成一个包含以下内容的 dist/index.html 文件：

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>webpack App</title>
  </head>
  <body>
    <script src="index_bundle.js"></script>
  </body>
</html>
```

如果你有多个 webpack 入口，他们都会在已生成 HTML 文件中的 <script> 标签内引入。

如果在 webpack 的输出中有任何 CSS 资源（例如，使用 MiniCssExtractPlugin 提取的 CSS），那么这些资源也会在 HTML 文件 <head> 元素中的 <link> 标签内引入。

