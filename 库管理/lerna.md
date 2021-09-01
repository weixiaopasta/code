### 概要

lerna是GitHub上面开源的一款js代码库管理软件， 用来对一系列相互耦合比较大、又相互独立的js git库进行管理。解决各个库之间修改混乱、难以跟踪的问题。lerna可以优化这种情形下的工作流。

对于一些功能比较全的库，我们往往会把各个小功能拆分成独立的npm库，他们直接有比较强的依赖关系。比如：Babel、React等开源代码都是按照这种方式进行处理的。

#### 代码库结构

lerna管理的库文件结构类似如下这样

```
my-lerna-repo/
  package.json
  packages/
    package-1/
      package.json
    package-2/
      package.json
```

#### lerna主要做了什么

通过lerna的命令lerna bootstrap 将会把代码库进行link。
通过lerna publish发布最新改动的库

#### lerna如何工作的
默认lerna有两种管理模式， 固定模式和独立模式

##### 固定模式
固定模式，通过lerna.json的版本进行版本管理。当你执行lerna publish命令时， 如果距离上次发布只修改了一个模块，将会更新对应模块的版本到新的版本号，然后你可以只发布修改的库。

这种模式也是Babel使用的方式。如果你希望所有的版本一起变更， 可以更新minor版本号，这样会导致所有的模块都更新版本。

##### 独立模式
独立模式，init的时候需要设置选项 --independent. 独立模式允许管理者对每个库单独改变版本号，每次发布的时候，你需要为每个改动的库指定版本号。这种情况下， lerna.json的版本号不会变化了， 默认为independent。

#### lerna.json解析

```
{
  "version": "1.1.3",
  "npmClient": "npm",
  "command": {
    "publish": {
      "ignoreChanges": [
        "ignored-file",
        "*.md"
      ]
    },
    "bootstrap": {
      "ignore": "component-*",
      "npmClientArgs": ["--no-package-lock"]      
    }
  },
  "packages": ["packages/*"]
}
```

* version , 当前库的版本
* npmClient , 允许指定命令使用的client， 默认是 npm， 可以设置成 yarn
* command.publish.ignoreChanges ， 可以指定那些目录或者文件的变更不会被publish
* command.bootstrap.ignore ， 指定不受 bootstrap 命令影响的包
* command.bootstrap.npmClientArgs ， 指定默认传给 lerna bootstrap 命令的参数
* command.bootstrap.scope ， 指定那些包会受 lerna bootstrap 命令影响
* packages ， 指定包所在的目录


### 命令行
####lerna publish
发布新的库版本

```
lerna publish  // 发布最新commit的修改
lerna publish <commit-id> // 发布指定commit-id的代码
```

https://www.jianshu.com/p/8b7e6025354b


#### lerna bootstrap
bootstrap作了如下工作

* 为每个包安装依赖
* 链接相互依赖的库到具体的目录(例如：如果 lerna1 依赖 lerna2，且版本刚好为本地版本，那么会在 node_modules 中链接本地项目，如果版本不满足，需按正常依赖安装)
* 执行 npm run prepublish
* 执行 npm run prepare

```
可以通过 -- 后添加选项, 设置npm cient的参数

lerna bootstrap -- --production --no-optional
也可以在lerna.json中配置


{
  ...
  "npmClient": "yarn",
  "npmClientArgs": ["--production", "--no-optional"]
}

```

#### Options
--hoist

这个选项，会把共同依赖的库安装到根目录的node_modules下， 统一版本

--ignore
忽略部分目录， 不进行安装依赖

```
lerna bootstrap --ignore component-*
```

也可以在lerna.json中配置
```
{
  "version": "0.0.0",
  "command": {
    "bootstrap": {
      "ignore": "component-*"
    }
  }
}
```

--ignore-scripts
不执行声明周期脚本命令， 比如 prepare

--registry <url>
指定registry

--npm-client
指定安装用的npm client
```
lerna bootstrap --npm-client=yarn
```
也可以在lerna.json中配置
```
{
  ...
  "npmClient": "yarn"
}
```
### lerna list
列举当前lerna 库包含的包

#### Options
--json
显示信息为json格式

--all
显示包含private的包


#### https://www.jianshu.com/p/8b7e6025354b
