#### 1. 安装Homebrew
> Homebrew安装macOS中没有预装的软件。更换国内源可以加快安装速度
```Shell
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/HomebrewUninstall.sh)"
```

#### 2. 光标移动
> 推荐使用Emacs系的快捷键，而不是command+，因为Emacs快捷键在所有系统级别的输入框内通用

> 行级操作
* Ctrl + A 移动到行首
* Ctrl + E 移动到行尾
* Ctrl + K 删除到行尾
* Ctrl + N 移动到下一行
* Ctrl + P 移动到上一行

> 字母级操作
* Ctrl + F 向右（forward）移动一个字母
* Ctrl + B 想左（backward）移动一个字母
* Ctrl + D 向右删除一个字母
* Ctrl + H 向左删除一个字母

> 按单词操作
* option + <-  光标向左移动一个单词
* option + -> 光标向右移动一个单词
* option + Delete  删除一个单词

> 选中操作
* 按住shift键，分别点击两个光标位置，就可以选中之间的内容