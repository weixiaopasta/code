
```
git branch branch_0.1 master 从主分支master创建branch_0.1分支
git branch -m branch_0.1 branch_1.0 将branch_0.1重命名为branch_1.0
git checkout branch_1.0/master 切换到branch_1.0/master分支
```

```
查看当前的本地分支与远程分支的关联关系
git branch -vv

2.如果远程新建了一个分支，本地没有该分支。
可以利用 git checkout --track origin/branch_name ，
这时本地会新建一个分支名叫 branch_name ，会自动跟踪远程的同名分支 branch_name。

```

