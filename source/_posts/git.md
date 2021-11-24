---
title: git
date: 2021-08-27 17:22:26
tags: [git]
category: []
---

<!-- toc -->

# 参考

https://backlog.com/git-tutorial/cn/intro/intro1_1.html

https://www.liaoxuefeng.com/wiki/896043488029600

# 常用

```zsh
# git clone

# 查看远程库的信息
# 远程仓库的默认名称是origin
# git remote
# git remote -v

# 查看分支情况
# git branch -a

# 切换到分支
# git checkout 分支

# 当前状态
# git status

# 提交记录
# git log
# git log --pretty=oneline
```

# 新增功能

```zsh
# 新建并切换到分支 branch_name，新建的分支还没有关联到指定的远程分支的
git checkout -b branch_name

# 添加进缓存
git add .

# 提交
git commit -m "fix(price_manage):修正 price_manage 目录下的"

# 将分支推到远程
git push origin branch_name
```

# dev 合共 feature 分支

```zsh
# 切换到 dev 分支 （已关联到远程 dev）
git checkout dev
git pull
# 在当前分支即本地 dev 分支上，合并 branch_name 分支
git merge branch_name
git push
```

# 修复 BUG

```zsh
# 确定好在哪个分支上修复 bug,比如 dev,切换到 dev，从 dev 上创建临时分支 issue-xxx
# 修复完成，切换到 dev ，在 dev 上合并 issue-xxx 并删除该临时分支
```

# 删除分支

```zsh
# 删除本地分支 new_store
git branch -d new_store

# 删除远程分支 new_store
git push origin --delete new_store
```

# 修改用户信息

```zsh
#修改 user.name
git config --global user.name huanglanying

#修改 user.email
git config --global user.email huanglanying@hitour.cc

#查看
git config user.name
git config user.email
```

# 工作栈

```zsh
#暂存工作到 stash 栈
git stash

#stash 栈中只有一个工作时，从栈中恢复该工作到分支中，并从栈中删除之
git stash pop

#查看栈中的所有工作
git stash list

#恢复指定工作
git stash apply stash@{0}
```

# 合并

```zsh
# git merge --abort #中止合并
# git reset --merge #撤销合并
# git pull
```

# 版本回退

```zsh
git reset --hard 3e744
# 3e744 为版本号前几位
```

## 查看远程与本地分支对应关系

`git remote show origin`
再运行一次 `git branch -a` 就能看到变化了

## 刷新远程分支（有的远程分支已删除但本地仍显示）

`git remote prune origin`

```sh
lanying@lanyingdeMacBook-Pro some-project % git remote prune origin
Pruning origin
URL: git@xxx/some-project.git
 * [pruned] origin/dev
 * [pruned] origin/feat-0910
 * [pruned] origin/test
lanying@lanyingdeMacBook-Pro some-project %
```

# 查看 git config

```zsh
git config --global --list
# or
vim ~/.gitconfig
```
