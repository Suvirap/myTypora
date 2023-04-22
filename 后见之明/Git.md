## 合并commit

`git rebase -i HEAD~3`
p是选择当前分支，s是将该commit和前一个commit合并。可能要`git push -f origin dev-lbs`。

## 落后于远端

> 远端已经合入了master，本地分支做了更改，但是落后于远端

`git pull --rebase origin master`

`git mergetool`处理冲突

`git rebase --continue`

`git add`

`git commit`

`git push -f origin xxx`

## 追加提交

`git commit --amend --no-edit`

git commit --amend也叫追加提交，它可以在不增加一个新的commi-id的情况下将新修改的代码追加到前一次的commit-id中。

如果要修改上一条的message，那么去掉`--no-edit`选项即可，`git commit --amend -m "xxx"`。同理，commit记录同样只会有一条。

## 基于远端分支建立本地分支

`git checkout -b fix-lib origin/upgrade-lib`

> `git checkout -b ac_branch`这是在当前分支上创建一个ac_branch分支
>
> `git checkout -b ac_branch origin/ac_branch` 这才是拉取远程分支ac_branch到本地

## 高频指令

```bash
6. git clone 默认是下载了所有分支的代码
7. git branch -a 查看项目所有分支
8. git branch -r 查看项目所有远程分支
9. git checkout '分支名' 切换分支
10. git branch 打印出来所有的分支，以及当前所在分支
11. git log 查看提交记录，退出 英文状态下 Q
12. git reflog 可查看修改记录（包括git reset 的回退记录）
13. git reset --hard {commit id} 回退版本
14. git stash //代码放进暂存区(未被commit的代码)
15. git stash apply 还原
16. git stash drop 清除最近一次的stash记录
17. git stash pop 还原并清除最近一次 stash
18. git stash list 查看暂存列表
19. git stash clear 清空所有 stash 的记录
20. git remote -v 显示所有远程仓库
21. git remote add url 添加一个远程仓库
22. git remote rm name # 删除远程仓库
23. git remote rename old_name new_name # 修改仓库名
```

## 版本回退

1、运行git reflog 命令查看你的历史变更记录，如下：

```bash
fdb70fe HEAD@{0}: pull origin newpbft: Fast-forward
40a9a83 HEAD@{1}: checkout: moving from guan to master
b3fa4c3 HEAD@{2}: commit: copy from newpbft, first init
71bf0ec HEAD@{3}: checkout: moving from newpbft to guan
71bf0ec HEAD@{4}: commit: 1. add moveStore() to clean up certStore and blockStore.
1006d67 HEAD@{5}: commit: 1. Add PBFT branch to Puppeth.
fa3fb56 HEAD@{6}: commit: 1. change some errors about packages and vars
5f40fdc HEAD@{7}: checkout: moving from master to newpbft
40a9a83 HEAD@{8}: clone: from https://github.com/yeongchingtarn/geth-pbft.git
```

2、然后用 `git reset --hard HEAD@{n}`，（n是你要回退到的引用位置）回退；比如上图可运行 `git reset --hard 40a9a83`

> git reset与git revert的区别？

git reset --hard {commitHashId} *// 回退到某一个版本* 

git revert -n {commitHashId} *// 回退某一个 commit, 会生成一个新的版本，反转覆盖掉原来的提交代码*

**注意：关于版本回退记录！！**

一个commit对应这一个版本，有一个commit id，40位的16进制数字，通过SHA1计算得到，不同的文件计算出来的SHA1值不同(有很小的几率相同，可忽略)，这样每一个提交都有其独特的id。每提交一个新版本，实际上 Git 就会把它们自动串成一条时间线。

在Git中，HEAD 表示当前版本，也就是e620a6ff0940a8dff…，HEAD^ 表示上一个版本，HEAD^^ 表示上上一个版本，往上100个版本可以写成HEAD加连续100个 ^ ，也可以写成：HEAD~100

## 拉取远程分支

```bash
// 拉取远程分支并建立本地分支，但不会自动切换到此本地分支
git fetch origin 远程分支名x:本地分支名x

git checkout -b 本地分支名 origin/远程分支名
```

## 分支合并

`git merge a`，将a合并到当前分支

注意事项：合并分支需要先更新本地分支代码，然后将本地分支1合并到本地分支2，不能直接合并远程分支1到本地分支2。

## 暂存区

当你需要紧急切换到主分支，执行紧急任务的时候，可以使用

```bash
git stash // 当前分支代码加入暂存区
git stash list // 查看暂存记录id
git stash apply // 还原
git stash drop 删除最后一条暂存区信息
```

## 删除本地分支和远程分支

```bash
1. git branch -d '分支名'
2. git branch -D '分支名' //强制删除
3. git push origin --delete '分支名' // 删除远程分支
```

## git rebase和git merge

rebase 翻译为变基，他的作用和 merge 很相似，用于把一个分支的修改合并到当前分支上, 不同于 git rebase的是，git merge 在不是 fast-forward（快速合并）的情况下，会产生一条额外的合并记录，类似Merge branch 'xxx' into 'xxx'的一条提交信息。

## 取消本地文件修改

### 1. 未使用 git add 缓存代码时。

可以使用 git checkout -- filepathname (比如： git checkout -- readme.md  ，不要忘记中间的 “--” ，不写就成了检出分支了！！)。

放弃所有的文件修改可以使用 git checkout .  命令。

此命令用来放弃掉所有还没有加入到缓存区（就是 git add 命令）的修改：内容修改与整个文件删除。但是此命令不会删除掉刚新建的文件；因为刚新建的文件还没已有加入到 git 的管理系统中。所以对于git是未知的。自己手动删除就好了。

### 2. 已经使用了  git add 缓存了代码。

可以使用  git reset HEAD filepathname （比如： git reset HEAD readme.md）来放弃指定文件的缓存，放弃所以的缓存可以使用 git reset HEAD . 命令。

此命令用来清除 git  对于文件修改的缓存。相当于撤销 git add 命令所在的工作。在使用本命令后，本地的修改并不会消失，而是回到了如（一）所示的状态。继续用（一）中的操作，就可以放弃本地的修改。

### 3. 已经用 git commit  提交了代码。

可以使用 git reset --hard HEAD^ 来回退到上一次commit的状态。此命令可以用来回退到任意版本：git reset --hard  commitid

你可以使用 git log 命令来查看git的提交历史。git log 的输出如下,之一这里可以看到第一行就是 commitid：

```sql
commit cf0d692e982d8e372a07aaa6901c395eec73e356 (HEAD -> master)
Author: toyflivver <2440659688@qq.com>
Date:   Thu Sep 28 14:07:14 2017 +0800
    多余的空行
commit 14aa4d7ad4ac6fba59b4b8261d32e478e8cc99ff
Author: toyflivver <2440659688@qq.com>
Date:   Thu Sep 28 14:06:44 2017 +0800
    正常的代码
commit da3a95c84b6a92934ee30b6728e258bcda75f276
Author: toyflivver <2440659688@qq.com>
Date:   Thu Sep 28 13:58:12 2017 +0800
    qbf
commit 267466352079296520320991a75321485224d6c6
Author: toyflivver <2440659688@qq.com>
Date:   Thu Sep 28 13:40:09 2017 +0800
    qbf
复制代码
```

可以看出现在的状态在 commitid 为 cf0d692e982d8e372a07aaa6901c395eec73e356 的提交上（有 HEAD -> master 标记）。

## git pull --rebase

`git pull` 先git fetch再git merge HEAD

`git pull --rebase` 先git fetch再git rebase HEAD

从远程拉取代码后会和本地的代码进行合并，由上面的原理可知，使用git merge拉取最新的另一个分支的commit后，还会多产生一个合并的commit；而rebase则直接在最新的commit后，即在当前本地分支的基础上，应用远程最新分支的修改，在当前分支上产生一个commit，不会再多一个commit。