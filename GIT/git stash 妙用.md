### 开发时经常碰到-同一个项目 不停的切分支，发现功能写错分支了，如何处理呢
https://www.jianshu.com/p/e9764e61ef90
#### git-stash - 将一个修改后的工作区中的改动保存起来，将工作区恢复到改动前的状态。

#### git stash命令参考
（1）git stash save "save message": 执行存储时，添加备注说明。
（2）git stash list：查看stash列表。
（3）git stash show：显示具体做了哪些改动，默认显示第一个stash存储，如果要显示其他存储，后面加stash@{$num}，比如第二个git stash show stash@{1}。
（4）git stash apply：应用某个存储,但不会把存储从存储列表中删除，默认使用第一个存储,即stash@{0}，如果要使用其他个，添加git stash apply stash@{$num}，比如第二个git stash apply stash@{1}。
（5） git stash pop：命令恢复之前缓存的工作目录，将缓存堆栈中的对应stash删除，并将对应修改应用到当前的工作目录下，默认为第一个stash,即stash@{0}，如果要应用并删除其他stash存储，命令：git stash pop stash@{$num}。
（6）git stash drop stash@{$num}：删除stash@{$num}存储。
（7）git stash clear：删除所有缓存的stash存储。


