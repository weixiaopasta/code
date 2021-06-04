语法： let const  箭头函数

全局函数和方法： Array.from, Promise, Array.prototype.includes



1、 基本用法介绍

是一系列插件的集合，包含了我们在babel6中常用的es2015,es2016, es2017等最新的语法转化插件，允许我们使用最新的js语法，比如 let，const，箭头函数等等，但不包括stage-x阶段的插件。

2、 @babel/polyfill

也就是babel6 中的 babel-polyfill , 使用 preset-env 能将最新的语法转换为ecmascript5的写法，当我们需要使用新增的全局函数(比如promise, Array.from)和实例方法(比如 Array.prototype.includes )时就需要引入 polyfill ，一般用法 在 index.js文件的最上层引入，或是在打包文件的entry 入口处引入，这样做的缺点是会全局引入整个 polyfill包，比如promise 会全局引入，污染全局环境。

3、 @babel/polyfill 和 @babel/preset-env 的关系

@babel/preset-env 中与 @babel/polyfill 的相关参数有 targets 和 useBuiltIns 两个

targets: 支持的目标浏览器的列表

useBuiltIns: 参数有 “entry”、”usage”、false 三个值。默认值是false，此参数决定了babel打包时如何处理@babel/polyfilll 语句。

“entry”: 会将文件中 import‘@babel/polyfilll’语句 结合 targets ，转换为一系列引入语句，去掉目标浏览器已支持的polyfilll 模块，不管代码里有没有用到，只要目标浏览器不支持都会引入对应的polyfilll 模块。

“usage”: 不需要手动在代码里写import‘@babel/polyfilll’，打包时会自动根据实际代码的使用情况，结合 targets 引入代码里实际用到 部分 polyfilll 模块

false: 对 import‘@babel/polyfilll’不作任何处理，也不会自动引入 polyfilll 模块。

需要注意的是在webpack打包文件配置的entry 中引入的 @babel/polyfill 不会根据useBuiltIns 配置任何转换处理。

总结：在业务项目中需要用到polyfill时, 可以使用和 @babel/preset-env 的 targets 和 useBuiltIns: usage来根据目标浏览器的支持情况，按需引入用到的 polyfill 文件。

不建议使用 import‘@babel/polyfilll’一次性引入所有的polyfill 文件。

