


如果我们已经提交并在提交日志消息中出错，我们可以执行 git commit -amend 修改前一次提交的日志消息，而不更改其快照。
我们可以使用 -m 选项从命令行传入新消息，而不会收到打开编辑器的提示。

```
git commit --amend -m "an updated commit message"
```