#### Babel
Babel 是一个工具链，主要用于将 ECMAScript 2015+ 版本的代码转换为向后兼容的 JavaScript 语法，以便能够运行在当前和旧版本的浏览器或其他环境中。

Babel 是代码转换器，比如将 ES6 转成 ES5，或者将 JSX 转成 JS 等。
借助 Babel，开发者可以提前用上新的 JS 特性，这对生产力的提升大有帮助。

@babel/cli是Babel的命令行工具，我们一般用不到，因为我们通常都是用babel-loader，里边使用的是@babel/core的api形式，我们只需要关心Babel的配置，如果有需要在编译阶段对代码进行处理 也可以写自己的插件，但是大部分场景是需要我们把Babel的配置搞清楚。


#### bable 默认只转换语法

 Babel默认只转换新的JavaScript语法（syntax），如箭头函数等，而不转换新的API，比如Iterator、Generator、Set、Maps、Proxy、Reflect、Symbol、Promise等全局对象，以及一些定义在全局对象上的方法（比如Object.assign）都不会转码；因此我们需要polyfill；
       因为这是一个 polyfill （它需要在源代码之前运行），我们需要让它成为一个 dependency（上线时的依赖）,而不是一个 devDependency（开发时的依赖）；



####  Babel的配置文件


Babel6的阶段 最常用的是.babelrc,但是现在Babel7支持了更多格式：
```
const RELATIVE_CONFIG_FILENAMES = [".babelrc", ".babelrc.js", ".babelrc.cjs", ".babelrc.mjs", ".babelrc.json"];
和package.json files with a "babel" key。
```

配置文件的格式如下：
```
{
    "presets": [
      [
        "@babel/preset-env",
        {
          "modules": "commonjs"
        }
      ]
    ],
    "plugins": [
      [
        "@babel/plugin-transform-runtime",
        {
          "corejs": 3
        }
      ],
      "@babel/plugin-syntax-dynamic-import",
    ]
  }
}

```

#### Babel 中的 plugins 与 presets

####  plugins
先引入一个例子：
```
// index.js
// 箭头函数
[1,2,3].map(n => n + 1);

// 模板字面量
let nick = 'Rain';
let desc = `hello ${nick}`;

```
这里用到了两个ES6才支持的新特性：箭头函数、模版字符串。在只支持 ES5 的浏览器里，这两段代码会报错。

因此，可以借助插件将代码转成 ES5。


// .babelrc
{
  "plugins": [
    "transform-es2015-arrow-functions",
    "transform-es2015-template-literals"
  ]
}
这样，安装了对应的依赖之后，上面两段代码就可以正常执行了。

#### presets
Babel插件一般尽可能拆成小的力度，开发者可以按需引进。比如对 ES6 转 ES5 的功能，Babel 官方拆成了20+个插件。

这样的好处显而易见，既提高了性能，也提高了扩展性。比如开发者想要体验ES6 的箭头函数特性，那他只需要引入 transform-es2015-arrow-functions 插件就可以，而不是加载 ES6 全家桶。

但很多时候，逐个插件引入的效率比较低下。比如在项目开发中，开发者想要将所有 ES6 的代码转成 ES5，插件逐个引入的方式令人抓狂，不单费力，而且容易出错。

这个时候，可以采用 Babel Preset。

可以简单的把 Babel Preset 视为 Babel Plugin 的集合。比如 babel-preset-es2015 就包含了所有跟 ES6 转换有关的插件。
```

// .babelrc
{
  "presets": [ "es2015" ]
}

```


#### plugins 与 presets 的执行顺序
可以同时使用多个 Plugin 和 Preset，此时，它们的执行顺序非常重要。

先执行完所有 Plugin，再执行 Preset。
多个 Plugin，按照声明次序顺序执行。
多个 Preset，按照声明次序逆序执行。
比如 .babelrc配置如下，那么执行的顺序为：

Plugin：transform-react-jsx、transform-async-to-generator
Preset：es2016、es2015

```
// .babelrc
{
  "plugins": [ 
    "transform-react-jsx",
    "transform-async-to-generator"
  ],
  "presets": [ 
    "es2015",
    "es2016"    
  ]
}
```

####  传参数
plugins和preset的配置是数组的形式，如果不需要传参数，最基本的就是字符串名称，如果需要传参数，把它写成数组的形式，数组第一项是字符串名称，第二项是要传的参数对象。





