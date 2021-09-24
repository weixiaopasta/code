#### 用途

npm ci和npm install命令一样，是用来安装依赖的命令，但他可以比常规的 npm 安装快得多，也比常规安装更严格，他可以npm依赖安装的一致和稳定 (锁版本)。

在package.json中，每次install后，对应的版本前都有个 ^ 符号。在这种情况下，你再次install时安装的包的版本可能与前次不一样，具体的，你可以到package-lock.json中查看实际的包版本。

^的匹配规则是：>= 当前版本， “兼容版本号”。匹配到的版本号要大于指定版本号，但是主版本号必须一致。

主版本号 是第一个非零的数字。

举例说明：
```
如：^1.1.2 ，表示>=1.1.2 <2.0.0，可以是1.1.2，1.1.3，…..，1.1.n，1.2.n，…..，1.n.n

如：^0.2.3 ，表示>=0.2.3 <0.3.0，可以是0.2.3，0.2.4，…..，0.2.n

如：^0.0，表示 >=0.0.0 <0.1.0，可以是0.0.0，0.0.1，…..，0.0.n
```
若我们一直使用install命令时，便会遇到开发和测试、发布时包版本不同的问题，这种细微的差别往往会导致严重的结局。

#### 用法
在npm i(install)的地方改用npm ci，当然项目中必须有一个package-lock.json或npm-shrinkwrap.json。

注：npm版本要>=5.7

#### 三、区别
npm ci与npm i主要有以下的区别。

npm i依赖package.json，而npm ci依赖package-lock.json。
当package-lock.json中的依赖于package.json不一致时，npm ci退出但不会修改package-lock.json。
npm ci只可以一次性的安装整个项目依赖，但无法添加单个依赖项。
npm ci安装包之前，会删除掉node_modules文件夹，因此他不需要去校验已下载文件版本与控制版本的关系，也不用校验是否存在最新版本的库，所以下载的速度更快。

#### npm ci命令比npm install命令快2至10倍
npm 5.7.1的发布给我们带了一系列新的功能。

其中我最喜欢的就是npm ci命令了。

npm ci命令

1.npm ci命令只根据lock-file去下载node_modules. 如果你的package.json文件与lock-file不同步，则会抛出错误。

2.每次运行npm ci命令时，它都会删掉你的node_modules文件夹，然后重新下载。

3.它比npm install命令快2至10倍，因为它不必在去比对node_modules中已经下好node_modules进行版本比对。

 