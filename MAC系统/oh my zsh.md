#### oh my zsh mac 自带命令行

相比于默认的 Bash，Zsh 有更多的自定义选项，并支持扩展。因此 Zsh 可以实现更强大的命令补全，命令高亮等一系列酷炫功能

https://zhuanlan.zhihu.com/p/58073103


#### 安装

如果你不想看官方的安装说明，请看这里：
```
第一步 → 把 oh-my-zsh 项目 Clone 下来：

git clone https://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
第二步 → 复制 .zshrc

cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
第三步 → 更改你的默认 Shell

chsh -s /bin/zsh
```
#### 主题配置

编辑 ~/.zshrc 文件

你会看到有一行教ZSH_THEME="robbyrussell"的脚本，把它替换成ZSH_THEME="agnoster"。然后回到终端，输入 source ~/.zshrc，你会发现你的Zsh主题变了

现在你的主题名称是Agnoster，如果你觉得不太好看，你可以改。前往 oh-my-zsh 的 Wiki 就可以看到大多数 oh-my-zsh 的内置主题以及它们的截图。如果你看中的其中的一款，可以重复上面的步骤，编辑~/.zshrc，并更改ZSH_THEME="xxx"。


P.S. 这些主题都保存在 "~/.oh-my-zsh/themes" 目录中

#### 语法高亮插件安装

安装zsh-syntax-highlighting语法高亮插件



```
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git 
echo "source ${(q-)PWD}/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
```

### 生效
```
source ~/.zshrc

```