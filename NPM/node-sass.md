### node-sass

node-sass 是一个 nodejs 环境下提供的一个 Bridge，它提供了调用 LibSass 的能力（而 LibSass 是一个 C++ 实现的样式预处理器）。

```
ps: 可以看到，node-sass 并不完全是 javascript 实现的，而是借助了 C++ 的能力，毕竟编译型语言还是速度快啊。
```

### node-sass和node版本不兼容
#### node-sass 与node 版本匹配表

https://www.npmjs.com/package/node-sass


 如果node版本有变 ，执行了下npm rebuild node-sass就好了
 ```
 这个命令在匹配的文件夹上运行 npm build 命令。当您安装一个新版本的 node，并且必须使用新的二进制文件重新编译所有 c + + 插件时，这是非常有用的

 ```
### sass官网力推工具

#### 使用Dart Sass
目前已知问题：
element-ui 图标错乱


