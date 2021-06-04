https://www.cnblogs.com/lzkwin/p/11878509.html

什么是死代码
很简单：就是 Webpack 没看到你使用的代码。Webpack 跟踪整个应用程序的 import/export 语句，因此，如果它看到导入的东西最终没有被使用，它会认为那是“死代码”，并会对其进行 tree-shaking 。

DCE (Dead code elimination)
代码不会被执⾏，不可到达
代码执⾏的结果不会被⽤到
代码只会影响死变量（只写不读）

下面是一些死代码和“活”代码的例子，希望能让你更明白。请记住，在某些情况下，Webpack 会将某些东西视为死代码，尽管它实际上并不是。请参阅《副作用》一节，了解如何处理。

```
// 导入并赋值给 JavaScript 对象，然后在下面的代码中被用到
// 这会被看作“活”代码，不会做 tree-shaking
import Stuff from './stuff';
doSomething(Stuff);
// 导入并赋值给 JavaScript 对象，但在接下来的代码里没有用到
// 这就会被当做“死”代码，会被 tree-shaking
import Stuff from './stuff';
doSomething();
// 导入但没有赋值给 JavaScript 对象，也没有在代码里用到
// 这会被当做“死”代码，会被 tree-shaking
import './stuff';
doSomething();
// 导入整个库，但是没有赋值给 JavaScript 对象，也没有在代码里用到
// 非常奇怪，这竟然被当做“活”代码，因为 Webpack 对库的导入和本地代码导入的处理方式不同。
import 'my-lib';
doSomething();
```
用支持 tree-shaking 的方式写 import
在编写支持 tree-shaking 的代码时，导入方式非常重要。你应该避免将整个库导入到单个 JavaScript 对象中。当你这样做时，你是在告诉 Webpack 你需要整个库， Webpack 就不会摇它。

以流行的库 Lodash 为例。一次导入整个库是一个很大的错误，但是导入单个的模块要好得多。当然，Lodash 还需要其他的步骤来做 tree-shaking，但这是个很好的起点。

```
// 全部导入 (不支持 tree-shaking)
import _ from 'lodash';
// 具名导入(支持 tree-shaking)
import { debounce } from 'lodash';
// 直接导入具体的模块 (支持 tree-shaking)
import debounce from 'lodash/lib/debounce';
```

#### Tree-shaking 原理
利⽤ ES6 模块的特点:

只能作为模块顶层的语句出现
import 的模块名只能是字符串常量
import binding 是 immutable(不可改变的) 的
代码擦除： uglify 阶段删除⽆⽤代码

### 基本的 Webpack 配置

使用 Webpack 进行 tree-shaking 的第一步是编写 Webpack 配置文件。你可以对你的 webpack 做很多自定义配置，但是如果你想要对代码进行 tree-shaking，就需要以下几项。

首先，你必须处于生产模式。Webpack 只有在压缩代码的时候会 tree-shaking，而这只会发生在生产模式中。

其次，必须将优化选项 “usedExports” 设置为true。这意味着 Webpack 将识别出它认为没有被使用的代码，并在最初的打包步骤中给它做标记。

最后，你需要使用一个支持删除死代码的压缩器。这种压缩器将识别出 Webpack 是如何标记它认为没有被使用的代码，并将其剥离。TerserPlugin 支持这个功能，推荐使用。

下面是 Webpack 开启 tree-shaking 的基本配置：

```
// Base Webpack Config for Tree Shaking
const config = {
 mode: 'production',
 optimization: {
  usedExports: true,
  minimizer: [
   new TerserPlugin({...})
  ]
 }
};
```

有什么副作用
仅仅因为 Webpack 看不到一段正在使用的代码，并不意味着它可以安全地进行 tree-shaking。有些模块导入，只要被引入，就会对应用程序产生重要的影响。一个很好的例子就是全局样式表，或者设置全局配置的JavaScript 文件。

Webpack 认为这样的文件有“副作用”。具有副作用的文件不应该做 tree-shaking，因为这将破坏整个应用程序。Webpack 的设计者清楚地认识到不知道哪些文件有副作用的情况下打包代码的风险，因此默认地将所有代码视为有副作用。这可以保护你免于删除必要的文件，但这意味着 Webpack 的默认行为实际上是不进行 tree-shaking。

幸运的是，我们可以配置我们的项目，告诉 Webpack 它是没有副作用的，可以进行 tree-shaking。

如何告诉 Webpack 你的代码无副作用
package.json 有一个特殊的属性 sideEffects，就是为此而存在的。它有三个可能的值：

true 是默认值，如果不指定其他值的话。这意味着所有的文件都有副作用，也就是没有一个文件可以 tree-shaking。

false 告诉 Webpack 没有文件有副作用，所有文件都可以 tree-shaking。

第三个值 […] 是文件路径数组。它告诉 webpack，除了数组中包含的文件外，你的任何文件都没有副作用。因此，除了指定的文件之外，其他文件都可以安全地进行 tree-shaking。

每个项目都必须将 sideEffects 属性设置为 false 或文件路径数组。在我公司的工作中，我们的基本应用程序和我提到的所有共享库都需要正确配置 sideEffects 标志。

下面是 sideEffects 标志的一些代码示例。尽管有 JavaScript 注释，但这是 JSON 代码：

```
// 所有文件都有副作用，全都不可 tree-shaking
{
 "sideEffects": true
}
// 没有文件有副作用，全都可以 tree-shaking
{
 "sideEffects": false
}
// 只有这些文件有副作用，所有其他文件都可以 tree-shaking，但会保留这些文件
{
 "sideEffects": [
  "./src/file1.js",
  "./src/file2.js"
 ]
}
```

全局 CSS 与副作用
首先，让我们在这个上下文中定义全局 CSS。全局 CSS 是直接导入到 JavaScript 文件中的样式表(可以是CSS、SCSS等)。它没有被转换成 CSS 模块或任何类似的东西。基本上，import 语句是这样的：
```
// 导入全局 CSS
import './MyStylesheet.css';
```
因此，如果你做了上面提到的副作用更改，那么在运行 webpack 构建时，你将立即注意到一个棘手的问题。以上述方式导入的任何样式表现在都将从输出中删除。这是因为这样的导入被 webpack 视为死代码，并被删除。

幸运的是，有一个简单的解决方案可以解决这个问题。Webpack 使用它的模块规则系统来控制各种类型文件的加载。每种文件类型的每个规则都有自己的 sideEffects 标志。这会覆盖之前为匹配规则的文件设置的所有 sideEffects 标志。

所以，为了保留全局 CSS 文件，我们只需要设置这个特殊的 sideEffects 标志为 true，就像这样:

```
// 全局 CSS 副作用规则相关的 Webpack 配置
const config = {
 module: {
  rules: [
   {
    test: /regex/,
    use: [loaders],
    sideEffects: true
   }
  ]
 } 
};
```


Webpack 的所有模块规则上都有这个属性。处理全局样式表的规则必须用上它，包括但不限于 CSS/SCSS/LESS/等等


#####  划重点：所有可需要 tree-shaking 的代码必须以这种方式编译。因此，如果你有要导入的库，则必须将这些库编译为 es2015 模块以便进行 tree-shaking 。如果它们被编译为 commonjs，那么它们就不能做 tree-shaking ，并且将会被打包进你的应用程序中。许多库支持部分导入，lodash 就是一个很好的例子，它本身是 commonjs 模块，但是它有一个 lodash-es 版本，用的是 es2015模块。

此外，如果你在应用程序中使用内部库，也必须使用 es2015 模块编译。为了减少应用程序包的大小，必须将所有这些内部库修改为以这种方式编译。