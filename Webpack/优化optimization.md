https://zhuanlan.zhihu.com/p/152097785


####  optimization.splitChunks 和 optimization.runtimeChunk

#### splitChunks 插件

简单的来说就是Webpack中一个提取或分离代码的插件，主要作用是提取公共代码，防止代码被重复打包，拆分过大的js文件，合并零散的js文件。

默认情况下，它只会影响到按需加载的 chunks，因为修改 initial chunks 会影响到项目的 HTML 文件中的脚本标签。

webpack 将根据以下条件自动拆分 chunks：

新的 chunk 可以被共享，或者模块来自于 node_modules 文件夹
新的 chunk 体积大于 20kb（在进行 min+gz 之前的体积）
当按需加载 chunks 时，并行请求的最大数量小于或等于 30
当加载初始化页面时，并发请求的最大数量小于或等于 30
按需加载的代码块，最大数量应该小于或者等于5
初始加载的代码块，最大数量应该小于或等于3
当尝试满足最后两个条件时，最好使用较大的 chunks。


```
module.exports = {
  //...
  optimization: {
    splitChunks: {
      chunks: 'async', // all, init
      minSize: 20000,
      minRemainingSize: 0,
      minChunks: 1,
      maxAsyncRequests: 30,
      maxInitialRequests: 30,
      enforceSizeThreshold: 50000,
      cacheGroups: {
        defaultVendors: {
          test: /[\\/]node_modules[\\/]/,
          priority: -10,
          reuseExistingChunk: true,
        },
        default: {
          minChunks: 2,
          priority: -20,
          reuseExistingChunk: true,
        },
      },
    },
  },
};

```
### chunk
这表明将选择哪些 chunk 进行优化。当提供一个字符串，有效值为 all，async 和 initial。设置为 all 可能特别强大，因为这意味着 chunk 可以在异步和非异步 chunk 之间共享。


### minChunks

拆分前必须共享模块的最小 chunks 数


### eg1:
创建一个 commons chunk，其中包括入口（entry points）之间所有共享的代码。
```
module.exports = {
  //...
  optimization: {
    splitChunks: {
      cacheGroups: {
        commons: {
          name: 'commons',
          chunks: 'initial',
          minChunks: 2,
        },
      },
    },
  },
};

```

### eg2:
创建一个 vendors chunk，其中包括整个应用程序中 node_modules 的所有代码。
如下配置可能会导致包含所有外部程序包的较大 chunk。建议仅包括你的核心框架和实用程序，并动态加载其余依赖项。

```
module.exports = {
  //...
  optimization: {
    splitChunks: {
      cacheGroups: {
        commons: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          chunks: 'all',
        },
      },
    },
  },
};

```




### eg3:
创建一个 custom vendor chunk，其中包含与 RegExp 匹配的某些 node_modules 包。
这将导致将 react 和 react-dom 分成一个单独的 chunk。 如果你不确定 chunk 中包含哪些包，请参考 Bundle Analysis 部分以获取详细信息。
```
module.exports = {
  //...
  optimization: {
    splitChunks: {
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/](react|react-dom)[\\/]/,
          name: 'vendor',
          chunks: 'all',
        },
      },
    },
  },
};
```


### 缓存组（Cache Group）

默认模式会将所有来自node_modules的模块分配到一个叫vendors的缓存组；所有重复引用至少两次的代码，会被分配到default的缓存组。

一个模块可以被分配到多个缓存组，优化策略会将模块分配至更高优先级别（priority）的缓存组，或者会分配至可以形成更大体积代码块的组里。


在满足下述所有条件时，那些从相同代码块和缓存组来的模块，会形成一个新的代码块（译注：比如，在满足条件下，一个vendor可能会被分割成两个，以充分利用并行请求性能）。

有四个选项可以用于配置这些条件：

minSize(默认是30000)：形成一个新代码块最小的体积
minChunks（默认是1）：在分割之前，这个代码块最小应该被引用的次数（译注：保证代码块复用性，默认配置的策略是不需要多次引用也可以被分割）
maxInitialRequests（默认是3）：一个入口最大的并行请求数
maxAsyncRequests（默认是5）：按需加载时候最大的并行请求数。


配置缓存组(Configurate cache group)

这是默认的配置：
```
splitChunks: {
    chunks: "async",
    minSize: 30000,
    minChunks: 1,
    maxAsyncRequests: 5,
    maxInitialRequests: 3,
    name: true,
    cacheGroups: {
        default: {
            minChunks: 2,
            priority: -20,
            reuseExistingChunk: true,
        },
        vendors: {
            test: /[\\/]node_modules[\\/]/,
            priority: -10
        }
    }
}
```



#### optimization.runtimeChunk
通过optimization.runtimeChunk: true选项，webpack会添加一个只包含运行时(runtime)额外代码块到每一个入口。（译注：这个需要看场景使用，会导致每个入口都加载多一份运行时代码）

运行时(runtime)额外代码块。

可以理解为webpack运行时的模版代码，包含entry和各chunks的映射关系等



新增知识点：
https://www.cnblogs.com/pluslius/p/9941642.html

模块标识符(Module Identifiers)

每个 module.id 会基于默认的解析顺序(resolve order)进行增量。也就是说，当解析顺序发生变化，ID 也会随之改变。

当前模块的 ID。module.id === require.resolve("./file.js")

由于修改了module.id的值，vendor分配到的id也发生了变化

```
main bundle 会随着自身的新增内容的修改，而发生变化。
vendor bundle 会随着自身的 module.id 的修改，而发生变化。
manifest bundle 会因为当前包含一个新模块的引用，而发生变化。

修改文件前id增量变化
   [1] ./src/index.js 336 bytes {1} [built]
   [2] (webpack)/buildin/global.js 509 bytes {0} [built]
   [3] (webpack)/buildin/module.js 517 bytes {0} [built]
  == [4] multi lodash 28 bytes {0} [built]==
修改文件后id增量变化，发现lodash解析顺序变化了，4变成了5
   [1] ./src/index.js 421 bytes {1} [built]
   [2] (webpack)/buildin/global.js 509 bytes {0} [built]
   [3] (webpack)/buildin/module.js 517 bytes {0} [built]
   [4] ./src/print.js 62 bytes {1} [built]
   ==[5] multi lodash 28 bytes {0} [built]==
   


可以使用两个插件来解决这个问题。第一个插件是 NamedModulesPlugin，将使用模块的路径，而不是数字标识符。虽然此插件有助于在开发过程中输出结果的可读性，然而执行时间会长一些。第二个选择是使用 HashedModuleIdsPlugin，推荐用于生产环境构建：
```