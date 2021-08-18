Git中.gitignore文件不起作用的解决以及Git中的忽略规则介绍

在.gitignore文件中的每一行保存一个匹配的规则例如:

# 此为注释 – 将被 Git 忽略
```
*.a       # 忽略所有 .a 结尾的文件
!lib.a    # 但 lib.a 除外
/TODO     # 仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
build/    # 忽略 build/ 目录下的所有文件
doc/*.txt # 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
```
.gitignore中已经标明忽略的文件目录下的文件，当我想git push的时候还会出现在push的目录中，原因是因为在Studio的git忽略目录中，

新建的文件在git中会有缓存，如果某些文件已经被纳入了版本管理中，

就算是在.gitignore中已经声明了忽略路径也是不起作用的，这时候我们就应该先把本地缓存删除，然后再进行git的push，这样就不会出现忽略的文件了。

git清除本地缓存命令如下：

```
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
```