### git diff HEAD -- youfileName

命令可以查看工作区和版本库里面最新版本的区别


### 撤销修改

git checkout -- file

```

命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：

一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次git commit或git add时的状态。

```

// 放弃单个文件修改,注意不要忘记中间的"--",不写就成了检出分支了!
git checkout -- filepathname
// 放弃所有的文件修改
git checkout .  


```
Git语法之Checkout使用

情况一:未使用 git add 缓存代码时:
// 放弃单个文件修改,注意不要忘记中间的"--",不写就成了检出分支了!
git checkout -- filepathname
// 放弃所有的文件修改
git checkout .  
此命令用来放弃掉所有还没有加入到缓存区（就是 git add 命令）的修改：内容修改与整个文件删除。但是此命令不会删除掉刚新建的文件。因为刚新建的文件还没已有加入到 git 的管理系统中。所以对于git是未知的。自己手动删除就好了。

情况二:已经使用了 git add 缓存了代码:
可以使用 git reset HEAD filepathname （比如： git reset HEAD readme.md）来放弃指定文件的缓存，放弃所有的缓存可以使用 git reset HEAD . 命令。

此命令用来清除 git 对于文件修改的缓存。相当于撤销 git add 命令所在的工作。在使用本命令后，本地的修改并不会消失，而是回到了如（一）所示的状态。继续用（一）中的操作，就可以放弃本地的修改。

情况三:已经用 git commit 提交了代码:
可以使用 **git reset --hard HEAD^ 来回退到上一次commit的状态。
此命令可以用来回退到任意版本：git reset --hard commitid **

你可以使用 **git log **命令来查看git的提交历史。git log 的输出如下,之一这里可以看到第一行就是 commitid：

```

### 文件删除 git rm

一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了：
这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，git status命令会立刻告诉你哪些文件被删除了：

现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit：

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：
```
 git checkout -- test.txt
```

注意：从来没有被添加到版本库就被删除的文件，是无法恢复的！


#### 本地Git仓库和远程仓库的创建及关联
```
git remote add origin git@github.com:YotrolZ/helloTest.git
 // 此命令在你的本地仓库中操作
 备注:origin就是我们的远程库的名字，这是Git默认的叫法，也可以改成别的;
git@github.com:YotrolZ/helloTest.git是我们远程仓库的路径(
```
第一次把本地内容推送到远程

```
git push -u origin master

备注:origin:远程仓库名字; master:分支
注意:我们第一次push的时候,加上-u参数,Git就会把本地的master分支和远程的master分支进行关联起来,我们以后的push操作就不再需要加上-u参数了

```

### 删除远程仓库 - 取消本地和远程仓库的关联
```
如果添加的时候地址写错了，或者就是想删除远程库，可以用git remote rm <name>命令。使用前，建议先用git remote -v查看远程库信息：

$ git remote -v
origin  git@github.com:michaelliao/learn-git.git (fetch)
origin  git@github.com:michaelliao/learn-git.git (push)
然后，根据名字删除，比如删除origin：

$ git remote rm origin
此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动。要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除。

```

### 创建分支 切换分支

```
$ git checkout -b dev
相当于以下两条命令
$ git branch dev
$ git checkout dev

```

### git branch 

git branch命令会列出所有分支，当前分支前面会标一个*号。


###  git branch -d  youbranch

删除本地某分支

### git push origin --delete 远程分支名

### git log

  带参数的Git log 可以查看分支的合并情况

  ```
  git log --graph --pretty=oneline --abbrev-commit

  // -graph：显示ASCII图形表示的分支合并历史
  // --pretty=oneline：一行显示，只显示哈希值和提交说明
  // -abbrev-commit：仅显示SHA-1的前几个字符，而非所有的40个字符
  // 用git log --graph命令可以看到分支合并图。
  ```
  <image src="./img/git_log.png" />


