---
title: git
date: 2018-06-06 21:08:13
tags:
---
### 本地创建一个git仓库
1. 新建文件夹
2. 进入文件夹
3. 使用git init命令初始化一个git仓库
4. 进行文件的编写	<!--more-->
5. 使用git add &lt;file&gt;添加文件到暂存区
6. 使用git commit -m "wrote a readme file",将文件提交到版本库。

### 常用命令
+ git status 查看工作区的状态
+ git log 显示从最近到最远的提交日志
	+ --pretty=oneline 可以简化输出内容
	+ --graph 可以看到分支合并图。
	+ 最实用的写法
	```
    git log --graph --pretty=oneline --abbrev-commit
	```
+ 版本回退
	+ git reset --hard HEAD^ 　　　回退到上一个版本
	>(head^^回退上两次版本)（head~100 回退上一百次版本）
	+ git reset --hard 454cbc4  　　回退到commit id为454cbc4的版本，commit id不必输全，只要有唯一辨识度即可。 
	>HEAD指向的版本就是当前版本(指向最后一次提交的结果)，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id（版本号，输入部分即可）
+ git reflog 记录了你的每一次命令
>Git的版本回退速度非常快，因为Git在内部有个指向当前版本的HEAD指针，当你回退版本的时候，Git仅仅是把HEAD指向你想回退的版本，然后顺便把工作区的文件更新了。所以你让HEAD指向哪个版本号，你就把当前版本定位在哪。
注意：所有的版本控制系统，其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等，Git也不例外。版本控制系统可以告诉你每次的改动，比如在第5行加了一个单词“Linux”，在第8行删了一个单词“Windows”。而图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道，也没法知道。

+ git add -u
将当前已跟踪且已修改文件更新

### 撤销更改
#### 丢弃工作区的修改
```
git checkout -- file 丢弃工作区的修改
```
这里分两种情况：
1. 文件只是在工作区做了修改，还未添加进暂存区中，使用*git checkout -- file*，file将回到和版本库一模一样的状态。
2. 文件已经添加到暂存区后，又做了修改，file将回到添加到暂存区后的状态。

#### 撤销暂存区的修改
```
git reset HEAD file 撤销暂存区的修改
```
将暂存区中的file文件删除，file文件回到未add时的状态

### 删除文件
```
rm test.txt 从工作区删除文件
```
这时，工作区和版本库就不一致了。
我们有两种选择：
1. 把版本库中的该文件也删除,使工作区和版本库一致
```
git rm test.txt
git commit -m ""
```
2. 用版本库中的该文件还原工作区的该文件
```
git checkout -- test.txt
```

### 分支
#### 分支原理
HEAD严格来说不是指向提交，而是指向master，master才是指向提交的，所以，HEAD指向的就是当前分支。
![head](https://raw.githubusercontent.com/fallinsilence/usedPictures/master/git/head.png)
每次提交，master分支都会向前移动一步，这样，随着你不断提交，master分支的线也越来越长：
当我们创建新的分支，例如dev时，Git新建了一个指针叫dev，指向master相同的提交，再把HEAD指向dev，就表示当前分支在dev上：
![dev](https://github.com/fallinsilence/usedPictures/blob/master/git/dev.png?raw=true)
你看，Git创建一个分支很快，因为除了增加一个dev指针，改改HEAD的指向，工作区的文件都没有任何变化！

不过，从现在开始，对工作区的修改和提交就是针对dev分支了，比如新提交一次后，dev指针往前移动一步，而master指针不变：
![dev-forward](https://github.com/fallinsilence/usedPictures/blob/master/git/dev_forward.png?raw=true)
假如我们在dev上的工作完成了，就可以把dev合并到master上。Git怎么合并呢？最简单的方法，就是直接把master指向dev的当前提交，就完成了合并：
![merge](https://github.com/fallinsilence/usedPictures/blob/master/git/merge.png?raw=true)

所以Git合并分支也很快！就改改指针，工作区内容也不变！

合并完分支后，甚至可以删除dev分支。删除dev分支就是把dev指针给删掉，删掉后，我们就剩下了一条master分支：
![delete branch](https://github.com/fallinsilence/usedPictures/blob/master/git/delete_branch.png?raw=true)

#### 分支常用命令
+ git branch 查看分支

+ git branch name 创建分支

+ git branch -d name 删除分支

+ git branch -D name 强行删除一个没有被合并过的分支

+ git checkout name 切换分支
 
+ git checkout -b name 创建分支并切换到该分支

+ git merge name 合并指定分支到当前分支

+ 创建远程origin的dev分支到本地
```
git checkout -b dev origin/dev
```

### 远程仓库
+ 将本地的仓库和远程库关联起来
```
git remote add origin(远程库的名字，可以自定义) git@github.com:michaelliao/learngit.git
```

+ 删除已关联的GitHub远程库
```
git remote rm(remove) origin
```

+ 将本地的masetr分支内容推送到远程库的master分支
```
git push (-u) origin master
```
由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送到远程库的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

+ 把本地分支推送到远程分支
```
git push origin local_branch:remote_branch 
```

+ 从远程库克隆一个仓库
```
git clone git@github.com:michaelliao/gitskills.git
```
+ 查看远程库的信息
```
git remote
```
+ 显示更详细的信息
```
git remote -v
origin  git@github.com:michaelliao/learngit.git (fetch)
origin  git@github.com:michaelliao/learngit.git (push)
```
如果没有推送权限，就看不到push的地址。

+ 推送分支
```
git push origin branch_name
```
将本地的branch_name分支推送到远程库的branch_name分支

### 分支管理策略
通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

```
git merge --no-ff -m "merge with no-ff" dev
```
合并分支时，加上--no-ff参数就可以用普通模式合并（禁用Fast  forward模式），合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

不用--no-ff，实际上只是将master的指针update成dev分支而已，用的还是dev的commit ID，而使用--no-ff之后，则是重新commit了一哈，有了新的commit ID.
简单地说就是 -no-ff 模式进行了一次新的 git commit 操作。
这也是为什么采用 -no-ff 模式要加入参数 -m 的原因。

### 变基
![rebase](https://github.com/fallinsilence/usedPictures/blob/master/git/rebase.png?raw=true)
你可以提取在 C4 中引入的补丁和修改，然后在 C3 的基础上应用一次。 在 Git 中，这种操作就叫做 变基。 你可以使用 rebase 命令将提交到某一分支上的所有修改都移至另一分支上，就好像“重新播放”一样。
在上面这个例子中，运行：
```
git checkout experiment
git rebase master	将在experiment分支上的修改，应用到master分支
```
它的原理是首先找到这两个分支（即当前分支 experiment、变基操作的目标基底分支 master）的最近共同祖先 C2，然后对比当前分支相对于该祖先的历次提交，提取相应的修改并存为临时文件，然后将当前分支指向目标基底 C3, 最后以此将之前另存为临时文件的修改依序应用。（译注：写明了 commit id，以便理解，下同）

现在回到 master 分支，进行一次快进合并。
```
git checkout matser
git merge experiment
```
![ff](https://github.com/fallinsilence/usedPictures/blob/master/git/ff.png?raw=true)

**总的原则是，只对尚未推送或分享给别人的本地修改执行变基操作清理历史，从不对已推送至别处的提交执行变基操作，这样，你才能享受到两种方式带来的便利。**

### 存储现场
当你在当前分支上工作时，因为一些原因你需要切换分支，但此时你当前分支的工作并没有完成，所以你不能提交该分支修改的内容。
你想到的可能是直接切换到另一个分支，你是个果断的人，于是你就这么做了，然后你惊奇的发现你在切换分支前分支上做的修改，新建的文件，
在切换到另一个分支后，竟然也能看到，你立刻意识到，专属于我的分支的文件别的分支也能看到了，这不是会给别人带来疑惑吗。
而且别人也能对该文件进行删除，万一他把你这个文件删除了，你只能去版本中找该文件的旧版本还原，就会丢失你辛辛苦苦写的代码。

幸好，Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：
```
git stash
```
现在，用git status查看工作区，就是干净的（除非有没有被Git管理的文件）。

工作区是干净的，刚才的工作现场存到哪去了？用git stash list命令看看：
```
git stash list
stash@{0}: WIP on dev: f52c633 add merge
```
工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；

另一种方式是用git stash pop，恢复的同时把stash内容也删了：

你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令：
```
$ git stash apply stash@{0}
```

理解：工作区和暂存区是一个公开的工作台，任何分支都会用到，并能看到工作台上最新的内容，只要在工作区、暂存区的改动未能够提交到某一个版本库（分支）中，那么在任何一个分支下都可以看得到这个工作区、暂存区的最新实时改动。 使用git stash就可以将暂存区的修改藏匿起来，使整个工作台看起来都是干净的。所以要清理整个工作台，那么前提是必须先将工作区的内容都add到暂存区中去。之后在干净的工作台上可以做另外一件紧急事件与藏匿起来的内容是完全独立的

### 标签管理
发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。

Git有commit，为什么还要引入tag？

“请把上周一的那个版本打包发布，commit号是6a5819e...”

“一串乱七八糟的数字不好找！”

如果换一个办法：

“请把上周一的那个版本打包发布，版本号是v1.2”

“好的，按照tag v1.2查找commit就行！”

所以，tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。

在Git中打标签非常简单，首先，切换到需要打标签的分支上：
然后，敲命令*git tag name*就可以打一个新标签：
```
git tag v1.0
```

可以用命令*git tag*查看所有标签：
```
git tag
```

默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？

方法是找到历史提交的commit id，然后打上就可以了：
```
git tag v0.9 f52c633(某次提交的id)
```

注意，标签不是按时间顺序列出，而是按字母排序的。可以用*git show tagname*查看标签信息

还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：
```
git tag -a v0.1 -m "version 0.1 released" 1094adb
```

**注意：标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。**

如果标签打错了，也可以删除：
```
git tag -d v0.1
```

因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。

如果要推送某个标签到远程，使用命令
```
git push origin tagname
```

或者，一次性推送全部尚未推送到远程的本地标签：
```
git push origin --tags
```

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：
```
git tag -d v0.9
```
然后，从远程删除。删除命令也是push
```
git push origin :refs/tags/v0.9
```

### 自定义git
+ 让git显示颜色
```
git config --global color.ui true
```
+ 配置别名
我们只需要敲一行命令，告诉Git，以后st就表示status：
```
git config --global alias.st status
```
+ 配置文件
配置Git的时候，加上--global是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。
配置文件放哪了？每个仓库的Git配置文件都放在.git/config文件中：

### 忽略特殊文件
在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。

不需要从头写.gitignore文件，GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：https://github.com/github/gitignore

忽略文件的原则是：
>忽略操作系统自动生成的文件，比如缩略图等；
忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

最后一步就是把.gitignore也提交到Git，就完成了！

+ 强制add被忽略的文件
```
git add -f App.class
```
+ 找出来是哪个规则导致了文件被忽略
```
git check-ignore -v App.class
.gitignore:3:*.class    App.class
```

**注意：**.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。那么解决方法就是先把本地缓存删除（改变成未track状态），然后再提交：
```
git rm -r --cached .    把当前目录下的所有文件改变为未track状态
git add .          把当前目录下的除了忽略的文件之外的其他文件都变为track状态（加入暂存区）
git commit -m 'update .gitignore'   提交这次更改
```
#### git rm与git rm --cached

当我们需要删除暂存区或分支上的文件, 同时工作区也不需要这个文件了, 可以使用
```
 git rm file_path
 git commit -m 'delete somefile'
 git push
```

当我们需要删除暂存区或分支上的文件, 但本地又需要使用, 只是不希望这个文件被版本控制, 可以使用
```
git rm --cached file_path
git commit -m 'delete remote somefile'
git push
```

### 注意
1. 假设两个分支master和dev，都存在a文件，master分支不对a文件做更改，dev分支对a文件做了更改。这时，我们将dev分支合并到master分支，dev分支中的a文件内容就会覆盖master分支中的a文件内容。



