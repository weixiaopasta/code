### 解析(resolve) 
https://www.webpackjs.com/configuration/resolve/

这些选项能设置模块如何被解析。webpack 提供合理的默认值，但是还是可能会修改一些解析的细节。



#### resolve.alias

创建 import 或 require 的别名，来确保模块引入变得更简单。

也可以在给定对象的键后的末尾添加 $，以表示精准匹配
```
alias: {
  xyz$: path.resolve(__dirname, 'path/to/file.js')
}
import Test1 from 'xyz'; // 精确匹配，所以 path/to/file.js 被解析和导入
import Test2 from 'xyz/file.js'; // 非精确匹配，触发普通解析./node_modules/xyz/file.js
```

####  resolve.enforceExtension

boolean

如果是 true，将不允许无扩展名(extension-less)文件。默认如果 ./foo 有 .js 扩展，require('./foo') 可以正常运行。但如果启用此选项，只有 require('./foo.js') 能够正常工作。默认：

```
enforceExtension: false
```

#### resolve.enforceModuleExtension

boolean

对模块是否需要使用的扩展（例如 loader）。默认：
```
enforceModuleExtension: false
```

#### resolve.extensions
array

自动解析确定的扩展。默认值为：

```
extensions: [".js", ".json"]
```
能够使用户在引入模块时不带扩展：
```
import File from '../path/to/file'
```



