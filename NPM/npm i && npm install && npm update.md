
####  npm install && npm i 
1、用npm i 安装的模块无法用npm uninstall卸载，需要用npm uninstall i命令

2、npm i 会帮助检测与当前node版本最匹配的npm包 版本号，并匹配出来相互依赖的npm包应该提升的版本号

3、部分npm包在当前node版本下无法使用，必须使用建议版本

4、安装报错时intall肯定会出现npm-debug.log 文件，npm i不一定



#### npm install && npm update
两者最大的区别是在对待已经安装过的模糊版本时候

npm install会忽略模糊版本
npm update会更新模糊版本至最新
另外: install and update 处理 devDependencies 方式也不同

npm install 会安装/更新devDependencies，除非你指定 –production标志
npm update 会忽略 devDependencies，除非你指定 –dev 标志
