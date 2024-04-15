# 不要看！！！一篇学习 Git 使用 的个人记录  
#### ps. 顺带学习 Markdown 语法
<br>
让我们来打一段话： 这是只有一个空格间距的效果
这是只有回车的效果<br>

> 你会发现只有一个空格的效果和只有回车的效果一样  

这是两个空格加回车的效果

> 这才真正实现了换行

想要显示代码敲反引号，在键盘左上角`~`的下面


## 一、使用Git必须要有的模型了解
首先你要想象有以下几个区域：工作目录、暂存区、本地仓库、远程仓库。

- Git的每一次提交所保存的都是提交与上一次提交不同的地方 



## 二、Git 常用指令及含义

### 1. git status
> 查看当前git的状态
### 2. git add /git -rm
> 将文件添加到暂存区
### 3. git commit -m "一些你觉得值得写的信息(必填)"
> 将暂存区的文件提交到仓库
### 4. git push <远程主机名> <本地分支名>:<远程分支名>
> 
### 5. git restore
> 可以将更改但未`git -commit`的文件还原。
### 6.git log
> `git log`命令显示从最近到最远的提交日志.
> 每个版本一个专属ID
> 如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=online`参数
> `git log --graph`可以产看到分支合并图
### 7.git reset --hard '版本'
> 首先，Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示,当前版本上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个^比较容易数不过来，所以写成`HEAD~100`。
> 版本号也可以填写 `git log`中看到的十六进制ID(不用打全，只要输出开头几位即可)
### 8. git reflog
> 当忘记了新版本ID，想回到新版本，但当前不在新版本，可以使用该命令。该命令记录了你的每一次命令
### 9. git -checkout <name> 或 git switch <name>
> 切换分支
### 10. git checkout -b <name> 或 git switch -c <name>
> 创建+切换分支
### 11. git branch
> 查看分支(本地)。
> `-a` 查看本地和远程的所有分支
> `-r` 查看远程
### 12. git branch <name>
> 创建分支
### 13. git branch -d <name>
> 删除分支
### 14. git merge <name>
> 合并某分支`<name>`到当前分支
> 如果当前分支和某分支有一样地方但不同的修改，则会有冲突，详见[这里](https://www.liaoxuefeng.com/wiki/896043488029600/900004111093344)
>> 通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，**删除**分支后，会**丢掉分支信息**。
>>
>> 如果要强制禁用`Fast forward`模式，Git就会在`merge`时生成一个**新的commit**，这样，从分支历史上就可以看出分支信息。
>> **`git merge --no-ff -m "信息" dev`** `--no-ff`参数，表示禁用`Fast forward`
>> 因为会产生一个新的commit，所以也需要-m
### 15. git remote show
> 产看远程主机名字
### 16. git stash
> 可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作
### 17. git stash list
> 列出所有存储的stash
### 18. git stash apply <可选：指定的stash编号>
> 恢复，但是stash中内容并不删除，你需要用`git stash drop <可选：指定的stash编号>` 来删除，编号可以去查 stash list 查看
### 19. git stash pop
> 恢复，同时把stash内容也删了，出栈
### 20. git cherry-pick < commit >
> 可以用该命令，把某一个commit(用hash编号描述)“复制”到当前分支，避免重复劳动。
### 21. git branch -D < name >
> 如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。
### 22. git remote
> 查看远程库的信息
> -v显示更详细的信息,显示了可以抓取和推送的origin的地址。如果没有推送权限，就看不到push的地址。
### 23.  git push origin <分支名>
> 推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上
>> 但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？
>> * `master`分支是主分支，因此要时刻与远程同步；
>> * `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
>> * `bug`分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
>> * `feature`分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。
>>
>> 总之，就是在Git中，分支完全可以在本地自己藏着玩，是否推送，视你的心情而定！
> 
> 若你的**小伙伴**已经向origin/dev分支**推送**了他的**提交**，而碰巧**你**也对**同样的**文件作了**修改**，并**试图推送**，则会**推送失败**，因为你的小伙伴的最新提交和你试图推送的提交**有冲突**，解决办法也很简单，Git已经提示我们，先用git pull把**最新的提交**从origin/dev**抓下来**，然后，在**本地合并**，**解决冲突**，**再推送**：
### 24. git pull
> 拉取分支
> 若git pull失败，原因是没有指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接：`git branch --set-upstream-to=<对应远程分支> <本地分支>`，如`git branch --set-upstream-to=origin/dev dev`
### 25. git rebase
> 可以使得提交分支更为美观，详情参考[这里](https://www.liaoxuefeng.com/wiki/896043488029600/1216289527823648)(这里没太仔细去看desu，挠头)
### 26. git tag < tag名称 > < commit id >
> 设置标签。默认 (不设置commit id) 标签是打在最新**已**提交的commit上的。否则就标签在对应的commit上





## 三、Git SSH提交方式
### 1. SSH简介

### 2. 查询是否已有SSH key
1. 打开终端
2. 输入 ` ls -al ~/.ssh` 来查看是有已有SSH key
``` 
$ ls -al ~/.ssh
# 列出.ssh文件夹下面的文件，如果他们存在
```
>Tip: 如果收到 ~/.ssh 不存在的错误信息，说明在默认位置中没有现存的 SSH 密钥对。你可以在第4步创建一个新的 SSH 密钥对。

3. 检查检查目录列表，看你是否已经有了 SSH 公钥。默认情况下，GitHub 支持的公钥文件名如下。
```
id_rsa.pub
id_ecdsa.pub
id_ed25519.pub
# 虽然不清楚为什么我没有第三条
```
4. 生成新的SSH key 或者重载已有的key
- 

### 3.
&ensp;&ensp;sh
## 四、Git 使用中注意点

###  1. 如果你在Windows上使用的Git，最好目录请不要包括任何中文捏
###  2. 令牌只能复制一次！！！！
###  3. 分支合并时冲突了看[这里](https://www.liaoxuefeng.com/wiki/896043488029600/900004111093344)

### 4. 多人合作出问题了可以参考[这里](https://www.liaoxuefeng.com/wiki/896043488029600/900375748016320)和[这里](https://www.liaoxuefeng.com/wiki/896043488029600/1216289527823648)





## 参考文献
<div id="refer-anchor-1"></div>
- [1] [廖雪峰的 Git 教程](https://www.liaoxuefeng.com/wiki/896043488029600)

<div id="refer-anchor-2"></div>
- [2] [用 http push 文件发现密码输入失败可参考文献](https://stackoverflow.com/questions/68775869/message-support-for-password-authentication-was-removed)

<div id="refer-anchor-3"></div>
- [3] [Markdown官方教程](https://markdown.com.cn/basic-syntax/blockquotes.html)
