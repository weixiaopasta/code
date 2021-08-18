### Git-merge的–ff和–no-ff。

假设合并前的分支是这样
<img src="./imgs/gitbranch.png" />


这是一个很常见的用例，功能开发分支是iss53，在开发新功能，master分支是线上分支，出现了问题，开辟了hotfix分支进行修复，修复完成，进行合并，需要把hotfix合并回master。

```
$ git checkout master
$ git merge hotfix
Updating f42c576..3a0874c
Fast-forward
 index.html | 2 ++
 1 file changed, 2 insertions(+)

```

步骤如下：

切换回master分支。
将hotfix分支合并会master分支。
然后看到了Fast-forward 的字样，这个词组的意思就是快进，播放电影的时候，可以注意一下，快进按钮上面就是这个词组。
那么实际变成了什么样呢？

<img src="./imgs/gitbranch2.png" />

仅仅是master指针指向了这个提交C4。这样是一种比较快的合并方式，轻量级，简单。
这个时候，我们往往会删掉hotfix分支，因为它的历史作用已经结束，这个时候，我们的iss53这个功能又向前开发，进行了一次提交，到了C5，那么变成了这样：

<img src="./imgs/gitbranch3.png" />

然后，我们要把iss53 这个分支合并回master，就变成了这样：

<img src="./imgs/gitbranch4.png">

这个时候生成了一个新的commit号，这种提交就不是fast-forward（这个时候也无法生成fast-forward提交，因为要将两个版本的内容进行合并，只有在没有需要合并内容的时候，会有这个fast-forward 方式的提交）。
如果我们对第一次合并，使用了--no-ff参数，那么也会产生这样的结果，生成一个新的提交，实际上等于是对C4 进行一次复制，创建一个新的commit，这就是--no-ff的作用。
