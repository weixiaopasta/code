#### 不正确用法

```
const webpack = require("webpack");
const path = require('path');
module.exports = {
  entry: {
    app: "./src/main.js",
    vendor: ["react","react-dom", "redux", "react-redux", "react-router-redux"]
  },
  output: {
    path: path.resolve(__dirname, 'output'),
    filename: "[name].[chunkhash].js"
  },
  plugins: [
    new webpack.optimize.CommonsChunkPlugin({names: ["vendor"]})
  ]
};
```
上面将项目一些基础库打包成一个名为vendor的chunk中，并将业务相关的代码打包到一个名为app的chunk中；
我们对其中的业务代码app.js进行修改后，重新编译
可以发现，在CommonsChunkPlugin这种配置下，当业务代码app发生变化，而库代码也跟着变化，vender的chunkhash也跟着变化，这样vendor的引用的名称跟着变化，导致浏览器端的长缓存机制失效。

#### 引起问题的原因

引起webpack每次打包编译时vendor跟着变化的原因：

webpack每次build的时候都会生成一些运行时代码。当只有一个文件时，运行时代码直接塞到这个文件中。当有多个文件时，运行时代码会被提取到公共文件中，也就是上面CommonsChunkPlugin配置的vendor chunk中。


所以，上面webpack的CommonsChunkPlugin配置中，每次编译时这些代码都会打包到vendor中，导致每次vendor的chunkhash每次都会变化。

那么，我们可以在对vendor chunk进行配置，抽取其中的公共代码，即webpack运行时代码，这样就可以将项目依赖的基础库模块与业务模块隔离开来，因为不会对这些文件进行修改，所以这些文件可达到长缓存的作用。具体配置如下：
```
module.exports = {
  entry: {
    app: "./app.js",
    vendor: ["react","react-dom", "redux", "react-redux", "react-router-redux"]
  },
  ....
  plugins: [
    new webpack.optimize.CommonsChunkPlugin({names: ["vendor"]}),
    new webpack.optimize.CommonsChunkPlugin({
        name: 'manifest',
        chunks: ['vendor']
    })
  ]
};

```
这样，即使修改业务app代码，项目依赖的基础库vendor chunk也不会发生变化；只是抽取的manifest chunk每次还会变化，但是这个文件体积非常小，相比vendor来说这种方式的收益更大


#### 在webpack中配置CommonsChunkPlugin时需要注意几点：

1. 配置webpack的output项时，其filename和chunkFilename必须使用chunkhash(chunk)。不要使用hash(build)，否则即使按照上面的配置也不能达到预期的效果。

2. 对于图片、字体等静态资源抽离使用的file-loader，其配置的hash表示的是静态文件的内容hash值，不是webpack每次打包编译生成的hash值, 切记！！！

3. 对于抽取的css样式文件，需要使用contenthash, 与file-loader中的hash意义相同。此处不能为chunkhash，否则其与抽取该样式文件的entry chunk的chunkhash保持一致，打不到缓存的目的。

```
module.exports = {
  ...
  plugins: [
     new ExtractTextPlugin({
        filename: 'static/[name]_[contenthash:7].css',
        disable: false,
        allChunks: true
     })
  ...
  ]
```

实现图片/字体的缓存

对于图片、字体等静态资源，在使用webpack构建提取时，其实是使用了file-loader来完成的，生成对应的文件hash值也就是由对应的file-loader来计算的。那么这些静态文件的hash值使用的是什么hash值呢，其实就是hash属性值。如下面代码所示：

```
module.exports = {
 ...
 rules: [
   ...
    {
      test: /\.(gif|png|jpe?g)(\?\S*)?$/,
      loader: require.resolve('url-loader'),
      options: {
        limit: 10000,
        name: path.posix.join('static',  '[name]_[hash:7].[ext]')
      }
    },
    font: {
      test: /\.otf|ttf|woff2?|eot(\?\S*)?$/,
      loader: require.resolve('url-loader'),
      options: {
        limit: 10000,
        name: path.posix.join('static', '[name]_[hash:7].[ext]')
      }
    }
 ]
}
```
可以看到上面使用的是hash属性值，此hash非webpack每次项目构建的hash，它是由file-loader根据文件内容计算出来的，不要误认为是webpack构建的hash。