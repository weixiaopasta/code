在组件依赖开发中，项目作为依赖库没办法单独直接运行，需要依赖进别的项目执行，这时候最常用的方式就是npm link。但用npm link引入有时候会因为各种问题导致构建或者运行时会报错，此时如果直接将文件复制进依赖目录则能正常运行。对于这样的情况，意外的碰到了一个很适合的解决方案——yalc。

#### Yalc
yalc 可以在本地将npm包模拟发布，将发布后的资源存放在一个全局存储中。然后可以通过yalc将包添加进需要引用的项目中。

这时候package.json的依赖表中会多出一个file:.yalc/...的依赖包，这就是yalc创建的flie:软链接。同时也会在项目根目录创建一个yalc.lock确保引用资源的一致性。因此，测试完项目还需要执行删除yalc包的操作，才能正常使用。当然，yalc也是支持link:链接方式

安装
```
NPM:
npm i yalc -g

Yarn:
yarn global add yalc
```
发布依赖
在所开发的依赖项目下执行发布操作
```
yalc publish
```
此时如果存在npm 生命周期脚本：prepublish、prepare、prepublishOnly、prepack、preyalcpublish，会按此顺序逐一执行。如果存在：postyalcpublish、postpack、publish、postpublish，也会按此顺序逐一执行。

想要完全禁用脚本执行需要使用
```
yalc publish --no-scripts
```
此时就已经将依赖发布到本地仓库了。此命令只是发包并不会主动推送。

当有新修改的包需要发布并且推送时，可以使用推送命令快速更新所有依赖
```
yalc publish --push
yalc push // 简写
参数:

--changed，快速检查文件是否被更改
--replace，强制替换包
```
添加依赖
进入到项目执行
```
yalc add [my-package]
```
可以看到项目中添加了yalc.lock文件，package.json对应的包名会有个地址为file:.yalc/开头的项目。
也可以使用
```
yalc add [my-package@version]
```
将版本锁定，避免因为本地新包推送产生影响。

参数:
```
--dev，将依赖添加进dependency中
--pure，不会影响package.json文件
--link，使用link方式引用依赖包，yalc add [my-package] --link
--workspace (or -W)，添加依赖到workspace:协议中
```
更新依赖
```
yalc update
yalc update [my-package]
```
会根据yalc.lock查找更新所有依赖，当发包后并未主动push，可以用此命令在项目内单独更新依赖。

移除依赖
```
yalc remove [my-package]

yalc remove --all // 移除所有依赖并还原
```
查看仓库信息
当我们要查看本地仓库里存在的包时
```
yalc installations show
```
要清理不需要的包时

```
yalc installations clean [my-package]
```