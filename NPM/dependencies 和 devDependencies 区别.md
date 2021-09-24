首先我们来看一下字面的意思：

devDependencies 用于本地开发环境。

dependencies 用于发布环境

devDependencies 是只会在开发环境下依赖的模块，生产环境不会被打入包内。通过 NODE_ENV=developement 或 NODE_ENV=production 指定开发还是生产环境。

dependencies 依赖的包不仅开发环境能使用，生产环境也能使用。其实这句话是重点，按照这个观念很容易决定安装模块时是使用--save还是--save-dev。

比如我们写一个项目要依赖于react，没有这个包的依赖运行就会报错，这时候就把这个依赖写入dependencies ；

而我们使用的一些构建工具比如babel、eslint、webpack这些只是在开发中使用的包，上线以后就和他们没关系了，所以将它写入devDependencies。
devDependencies：这些包通常是单元测试或者打包工具等，例如gulp, grunt, webpack, moca, coffee等



