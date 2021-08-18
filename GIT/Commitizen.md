### Commitizen
Commitizen是一个撰写合格 Commit message 的工具。

安装命令如下。

```
$ npm install -g commitizen
```

然后，在项目目录里，运行下面的命令，使其支持 Angular 的 Commit message 格式。

```
$ commitizen init --save-exact --save-dev cz-conventional-changelog --force
```



以后，凡是用到git commit命令，一律改为使用git cz。这时，就会出现选项，用来生成符合格式的 Commit message。