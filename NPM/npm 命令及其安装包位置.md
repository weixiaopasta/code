npm可以在局部或全局范围内安装包，在局部范围内，包会安装到根目录下node_modules文件夹下面，属于当前用户的。全局范围的包会安装在{prefix}/lib/node_modules/下，{prefix}通常是/usr/或/usr/local。

npm 命令

npm start | test 可以省略run 参数



#### 罗列全局路径下的包

```
 npm list -g --depth=0

```


#### 寻找包

npm search something --registry=https://registry.npmjs.org/ 


#### 管理缓存

npm安装包时会保存副本，这样下次再安装时，就不需要连网，缓存存储在默认用户目录下的.npm文件夹

```
ls ~/.npm
```

#### 清除缓存：

```
$ npm cache clean

```

#### npm 一次可安装多个包

```
$ npm i express momemt lodash mongoose body-parser webpack
```

