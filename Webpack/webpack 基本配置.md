```
const path = require('path');
module.exports={
    mode: "production",
    entry: "./app/entry", // string | array | object
    // entry: {
        a: "./app/entry-a",
        b: ["./app/entry-b1", "./app/entry-b2"]
    }, 
    output:{
        path:,
        // 输出解析文件的目录 
        publicPath:,
        filename:
    },
    module:{
        //这些规则能够对模块（module）应用loader,或者修改解析器 
        rules:[
            {
                // 这里是匹配条件，每个选项都接收一个正则表达式或字符串
                // test 和 include 具有相同的作用，都是必须匹配选项
                // exclude 是必不匹配选项（优先于 test 和 include）
                // 最佳实践：
                // - 只在 test 和 文件名匹配 中使用正则表达式
                // - 在 include 和 exclude 中使用绝对路径数组
                // - 尽量避免 exclude，更倾向于使用 include
                test: /\.jsx?$/,
                include: [
                    path.resolve(__dirname, "app")
                ],
                exclude: [
                    path.resolve(__dirname, "app/demo-files")
                ],
                loader: "babel-loader",
                options: {
                    presets: ["es2015"]
                },
            },
            {
                test: /\.css$/,
                use: [
                    'style-loader',
                    {
                        loader: 'css-loader',
                        options: {
                        importLoaders: 1
                        }
                    },
                    {
                        loader: 'less-loader',
                        options: {
                        noIeCompat: true
                        }
                    }
                ]
            }
        ]


    },
    resolve:{
        // 解析模块请求的选项
        // 告诉 webpack 解析模块时应该搜索的目录
        // 用于查找模块的目录
        // 如果你想要添加一个目录到模块搜索目录，此目录优先于 node_modules/ 搜索：
        modules:[path.resolve(__dirname, "src"), "node_modules"]

        // 使用的扩展名
        extensions: [".js", ".json", ".jsx", ".css"],
        alias: {
            // 模块别名相对于当前上下文导入
            // 模块别名列表
            "module": "new-module",
            // 起别名："module" -> "new-module" 和 "module/path/file" -> "new-module/path/file"

            "only-module$": "new-module",
            // 起别名 "only-module" -> "new-module"，但不匹配 "only-module/path/file" -> "new-module/path/file"

            "module": path.resolve(__dirname, "app/third/module.js"),
            // 起别名 "module" -> "./app/third/module.js" 和 "module/file" 会导致错误
        },

    },
    // 性能
    // 这些选项可以控制 webpack 如何通知「资源(asset)和入口起点超过指定文件限制」。
    // 配置如何展示性能提示。例如，如果一个资源超过 250kb，webpack 会对此输出一个警告来通知你。
    performance:{
        hints: "warning", // 枚举
        hints: "error", // 性能提示中抛出错误
        hints: false, // 关闭性能提示
        maxAssetSize: 200000, // 整数类型（以字节为单位）
        maxEntrypointSize: 400000, // 整数类型（以字节为单位）
        assetFilter: function(assetFilename) {
        // 提供资源文件名的断言函数
        // 此属性允许 webpack 控制用于计算性能提示的文件
         return assetFilename.endsWith('.css') || assetFilename.endsWith('.js');
        }
  },

  devtool: "source-map", // enum
  devtool: "inline-source-map", // 嵌入到源文件中
  devtool: "eval-source-map", // 将 SourceMap 嵌入到每个模块中
  devtool: "hidden-source-map", // SourceMap 不在源文件中引用
  devtool: "cheap-source-map", // 没有模块映射(module mappings)的 SourceMap 低级变体(cheap-variant)
  devtool: "cheap-module-source-map", // 有模块映射(module mappings)的 SourceMap 低级变体
  devtool: "eval", // 没有模块映射，而是命名模块。以牺牲细节达到最快。
  // 通过在浏览器调试工具(browser devtools)中添加元信息(meta info)增强调试
  // 牺牲了构建速度的 `source-map' 是最详细的。


    // 基础目录，绝对路径，用于从配置中解析入口起点(entry point)和 loader
    // 默认使用当前目录，但是推荐在配置中传递一个值。这使得你的配置独立于 CWD(current working directory - 当前执行路径)。
  context: path.resolve(__dirname, "app")
 


  target: "web", // 枚举
  target: "webworker", // WebWorker
  target: "node", // node.js 通过 require
  target: "async-node", // Node.js 通过 fs and vm
  target: "node-webkit", // nw.js
  target: "electron-main", // electron，主进程(main process)
  target: "electron-renderer", // electron，渲染进程(renderer process)
  target: (compiler) => { /* ... */ }, // 自定义
  // 包(bundle)应该运行的环境
  // 更改 块加载行为(chunk loading behavior) 和 可用模块(available module)


 // 不要遵循/打包这些模块，而是在运行时从环境中请求他们
  externals: "react", // string（精确匹配）
  externals: /^[a-z\-]+($|\/)/, // 正则
  externals: { // 对象
    angular: "this angular", // this["angular"]
    react: { // UMD
      commonjs: "react",
      commonjs2: "react",
      amd: "react",
      root: "React"
    }
  },
  externals: (request) => { /* ... */ return "commonjs " + request }
  

    // 这些选项决定了如何处理项目中的不同类型的模块。 
    // 防止 webpack 解析那些任何与给定正则表达式相匹配的文件。忽略的文件中不应该含有 import, require, define 的调用，或任何其他导入机制。忽略大型的 library 可以提高构建性能。
    noParse:/jquery|lodash/,

    // 从 webpack 3.0.0 开始
    noParse: function(content) {
        return /jquery|lodash/.test(content);
    },
    devServer: {
        proxy: { // proxy URLs to backend development server
        '/api': 'http://localhost:3000'
        },
        contentBase: path.join(__dirname, 'public'), // boolean | string | array, static file location
        compress: true, // enable gzip compression
        historyApiFallback: true, // true for index.html upon 404, object for multiple paths
        hot: true, // hot module replacement. Depends on HotModuleReplacementPlugin
        https: false, // true for self-signed, object for cert authority
        noInfo: true, // only errors & warns on hot reload
    // ...
    plugins:[]
  },

}

```

### entry
命名：

如果传入一个字符串或字符串数组，chunk 会被命名为 main。
filename: '[name].js' 
entry: './index.js'
打包之后 name 命名为main



如果传入一个对象，则每个键(key)会是 chunk 的名称，该值描述了 chunk 的入口起点。