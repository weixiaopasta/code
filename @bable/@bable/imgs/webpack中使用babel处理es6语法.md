```

index.js
const arr = [
　　new Promise(()=>{}),
　　new Promise(()=>{})
];

arr.map(item => {
　　console.log(item);
})

index.html

<!DOCTYPE html>
<html lang="en">
　　<head>
　　　　<meta charset="UTF-8">
　　　　<meta name="viewport" content="width=device-width, initial-scale=1.0">
　　　　<meta http-equiv="X-UA-Compatible" content="ie=edge">
　　　　<title>html template</title>
　　</head>
　　<body>
　　　　<div id='root'></div>
　　</body>
</html>
```
运行npx webpack(用dev-server打包放在了缓存里面，看不到最终的打包内容)。看到打包生成的main.js的最后几行，index里面写的js，原封不动的打包到了main.js里面。这个时候浏览器中运行，可以打印出promise对象。好像是没问题的，这是什么原因呢，这是因为chrome更新比较快，es6里面很多东西，他都做了实现，所以直接在chrome浏览器写es6语法没问题，但是比如在ie或者更新没那么快的浏览器，，，就会报错。。。

#### babel
这个时候需要借助babel,https://babeljs.io/。

先安装两个包
```
npm install --save-dev babel-loader @babel/core
```
一个是帮助webpack打包用的工具，一个是babel的核心库

webpack配置babel相关

```
module.exports = {
　　module: {
　　　　rules:[{
　　　　　　test: /\.js$/,
　　　　　　exclude: /node_modules/,
　　　　　　loader: "babel-loader"
　　　　}]
　　}
}
```
再继续安装
```
npm install @babel/preset-env --save-dev
```


为什么要安装这个模块，当我们使用babel－loader处理js文件的时候，实际上这个babel-loader只是webpack和babel做通信的一个桥梁，用了他之后，webpack和babel做了打通，但实际上，babel-loader并不会帮助我们把es6语法翻译成es5语法，还需要借助一些其它的模块才能够帮助我们把es6语法翻译成es5语法。babel/preset-env就是这样的一个模块，这里面包含了所有把es6转化成es5的规则。装好之后，还需要在webpack里面配置一下

```
module.exports = {
　　module: {
　　　　rules:[{
　　　　　　test: /\.js$/,
　　　　　　exclude: /node_modules/,
　　　　　　loader: "babel-loader",
　　　　　　options:{
　　　　　　　　"presets": ["@babel/preset-env"]
　　　　　　}
　　　　}]
　　}
}

```

再使用npx webpack，看转化后的main.js，发现es6的语法转化成了es5的语法。但是光做到这点不够。为什么呢？因为比如像Promise这样新的语法变量，包括数组里面这个map方法，在低版本的浏览器里，实际上还是不存在的。虽然了语法翻译，但只翻译了一部分。还有一些对象或者函数在一些低版本的浏览器里面还是没有的。

所以不仅要用preset-env翻译es6，还需要将缺失的语法补充到浏览器里，这个模块就是babel/polyfill。然后把polyfill引入到业务代码的最顶部

```
import "@babel/polyfill";

const arr = [
　　new Promise(()=>{}),
　　new Promise(()=>{})
];

arr.map(item => {
　　console.log(item);
});
```

处理好后，再运行，会发现原来打包好的main是28kb，现在是534kb。这多出来的内容就是polyfill弥补的内容，所以main.js一下子就变的很大。那么我不想要这么大，我只需要你在我需要补充语法的时候出来相应处理的代码就可以。
安装
```
 npm install core-js --save-dev
```

```
module.exports = {　　module: {
　　　　rules:[{
　　　　　　test: /\.js$/,
　　　　　　exclude: /node_modules/,
　　　　　　loader: "babel-loader",
　　　　　　options:{
　　　　　　　　presets: [['@babel/preset-env',{
　　　　　　　　/**
　　　　　　　　* 当我做polyfill填充的时候，去加一些低版本特性的时候，我不是把所有特性都加进来
　　　　　　　　* 是根据你的业务代码来决定要加什么
　　　　　　　　*/
　　　　　　　　useBuiltIns: 'usage',
　　　　　　　　corejs: 3
　　　　　　　　}]]
　　　　　　}
　　　　}]
　　}
}
```
主页打包出来的main就124kb，小了很多

当然preset也可以配置一些额外的参数

```
module.exports = {
　　module: {
　　　　rules:[{
　　　　　　test: /\.js$/,
　　　　　　exclude: /node_modules/,
　　　　　　loader: "babel-loader",
　　　　　　options:{
　　　　　　　　presets: [['@babel/preset-env',{
　　　　　　　　/**
　　　　　　　　* 意思是我的这个项目，打包会运行在>67这个版本的chrome浏览器下
　　　　　　　　* 比如chrome浏览器在67版本以上对es6语法支持很好了，就不需要翻译
　　　　　　　　*/
　　　　　　　　targets: {
　　　　　　　　　　chrome: "67",
　　　　　　　　},
　　　　　　　　/**
　　　　　　　　* 当我做polyfill填充的时候，去加一些低版本特性的时候，我不是把所有特性都加进来
　　　　　　　　* 是根据你的业务代码来决定要加什么
　　　　　　　　* @babel/polyfill，放在js入口
　　　　　　　　*/
　　　　　　　　useBuiltIns: 'usage',
　　　　　　　　corejs: 3
　　　　　　　　}]]
　　　　　　}
　　　　}]
　　}
}
```



关于bable的配置部分，可以单独建立一个新文件去配置.babelrc

然后把options对象拿出来放到.babelrc里面。然后删除babel配置的option



#### 重点

再强调一遍，如果我们写的只是业务代码，在配置的时候只要配置presets,同时引入polyfill就可以了。如果你写的是一个库项目代码的时候，这个时候要使用babel/plugin-transform-runtime。这个时候可以有效的避免presets,或者说是polyfill的问题。polyfill会污染全局环境。但是plugin-transform-runtime会以闭包的方式注入。这样写类库的时候是更好的方式。

