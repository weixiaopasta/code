### git rebase -i 将本地的多次提交合并为一个，以简化提交历史

https://blog.csdn.net/nrsc272420199/article/details/85555911
### git使用rebase和merge的正确姿势


其实只要看 graph 时间轴就行，如果你的分支落后于一个分支，并且你想把这个分支的内容合并到你的分支，这时候就 rebase 领先你的那个分支，使用 merge 的场景就一个原则，必须让你的分支领先于你要合并的那个分支，并且你的分支和你要合并的那个分支必须在一条时间轴上（使用 rebase 就能让两个分支在一条时间轴上）


https://zhuanlan.zhihu.com/p/34197548