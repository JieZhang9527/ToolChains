#### 1. 配置文件
* gitconfig 对git的行为进行配置。对于某个git仓库而言，一般有两个gitconfig文件生效，一个是`～/.gitconfig`，为全局配置；另一个位于每个git仓库下的`.git/config`。如果有重复的配置项，项目配置的优先级高于全局配置。
* .gitignore 指定文件的忽略规则，如果命中规则，就不会添加到git的版本管理中，同样分为全局和项目两个配置。

#### 2. GPG密钥
> 可以确保提交的安全性，类似于https，使用GPG密钥的提交在GitHub上会显示成Verified字样
* `brew install gpg`
* `gpg --full-generate-key`，第三步选择时长时，个人使用选择0表示永不过期。最后填写密码可以不填写
* 安装完成输入`gpg --list-keys` 查看刚刚生成的密钥，在pub下有一长串数字和字母，这是**GPG-ID**
* `gpg --armor --export pub GPG-ID` 将公钥复制到[GitHub GPG Keys](https://github.com/settings/keys)
* `git config --global user.signingkey GPG-ID` 来配置使用哪个key
* 单次提交时使用`git commit -s`参数开启GPG key，或使用`git config --global commit.gpgsign true`设置为全局使用

#### 3. 配置个人信息
* `git config --global user.name “zhangjie”`
* `git config --global user.email “2686534991@qq.com”`

#### 4. 配置忽略规则
>   `.gitignore`文件中可以配置我们需要忽略的文件和文件夹，但是这个文件仅对还没有被纳入git版本管理的文件生效。
* 如果某个文件被暂存，再配置`.gitignore`就不会生效，此时需要把所有文件取消暂存：
  * `git rm -r --cached .`
  * `git add .`

#### 5. git工作原理简介
* 每个git目录下都有一个名为`.git`的隐藏目录，关于git的一切都存储在这个目录中（全局配置除外），子目录有：
  * info 其中的`.gitignore`不会被纳入版本控制
  * hooks git钩子，在指定任务触发后做一些自定义配置
  * objects 存放所有git中的对象
  * logs 记录各个分支的移动情况
  * refs 记录所有引用

#### 6. git objects
> git是面向对象的！  
> git并不存储完整的提交历史，而是通过父提交对象不断向前查找，得出完整的历史
假设空仓库内编辑了两个文件：
* 会有两个数据对象，每个文件对应一个数据对象。当文件被修改时会生成新的数据对象。
* 会有一个树对象用来维护一系列数据对象，树对象不仅可以持有数据对象还可以持有另一个树对象，即可表示任意层次文件
* 会有一个提交对象：
  * 包含一个树对象用来表示某一次提交涉及的对象
  * 有自己的父提交，指向上一次提交的对象
  * 包含提交时间、提交者姓名、邮箱等额外信息

#### 7. git refs
> git面向对象，有指针，分支和标签都是只想提交对象的指针

#### 8. git logs
> 日志

#### 9. 查看提交记录
> 更细的命令可自查  
> git log
* `git log` 查看过去的提交记录
* `git log --stat` 查看每次提交具体改动的文件
* `git log -p` 查看文件的具体改动
* `git log --all` 展示所有分支
* 上述命令可添加`-n`子命令指定显示前n个提交

> git diff
* `git diff` 查看工作区的变动
* `git diff --staged` 暂存区的变动，也就是被`git add`文件的变动

#### 10. 分支
> 分支的本质可以理解为指向某个提交的指针，而每个提交都记录了父提交，从而形成一个链表  
>  查看分支
* `git branch` 显示本地的所有分支
* `git branch -vv` 额外显示每个分支的最后一次提交和这个分支跟踪的远程分支
* `git branch name` 创建一个名为name的分支，但不会切换分支
* `git branch -a` 查看本地和远程分支
* `git branch --remote` 查看远程分支

> 删除分支
* `git branch -d name` 删除名为name的分支
* `git branch --merged` 查看已经合并到某个指针（默认HEAD）的分支，即可以通过这个指针回溯到的分支
* `git branch --no-merged` 列出不可达的分支
* `git branch -D` 强行删除

> 切换分支
* `git checkout` 切换到某个分支上，注意：如果有未提交的改动，不要切换分支。
* `git checkout -b name` 创建名为name的分支，并切换到这个分支上

#### 11. reset命令
> git会管理三块不同的区域：工作区、暂存区和历史区。  
> clone下来一个项目时，三个区域内的内容一样，`git status`时刻比较**工作区和暂存区**以及**暂存区和历史区**之间的差异，从而得出待暂存和待提交的文件列表。  
> 写代码会更改工作区中的代码，但是暂存区和历史区一致，因此`git status`会显示有文件需要暂存，但没有文件需要提交。`git add`会把改动的文件和要提交的部分拷贝到暂存区。此时工作区和暂存区一致，但是暂存区和历史区不一致，所以显示没有文件需要暂存，有文件要提交。`git commit`后三个区域又保持一致。
* `git reset --soft`重置历史区，暂存区不变，相当于撤销`git commit`
* `git reset --mixed`将暂存区和历史区重置，相当于撤销`git add`和`git commit`
* `git reset --hard`将工作区、暂存区和历史区重置，撤销所有改动，注意：若工作区内文件未提交会丢失！

#### 12. 代码同步
* merge 合并分支代码
* 