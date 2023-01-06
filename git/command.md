# 常用命令

 • git删除远程分支   git push origin -d 分支名
 • git删除本地分支   git branch -d 分支名
 • git刷新同步远程分支   git remote update origin --prune
 • 回滚到某次提交  git reset  commitId
 • git reset --soft  提交之后的修改会被退回到暂存区
 • git reset --hard  提交之后的修改不做任何保留;git status 查看工作区是没有记录的

## 修改commit

### 修改刚commit，还没有push的commit信息

> git commit --amend

### 修改刚push的最近一次commit信息

```git
## 修改最近提交的 commit 信息
$ git commit --amend --message="modify message by daodaotest" --author="jiangliheng <jiang_liheng@163.com>"
## 仅修改 message 信息
$ git commit --amend --message="modify message by daodaotest"
## 仅修改 author 信息
$ git commit --amend --author="jiangliheng <jiang_liheng@163.com>"
```

## 修改历史提交记录

git rebase -I

## git stash

## 合并comit

> git rebase -i HEAD~3 #合并最近提交的3个commit

```git
Commands解释如下：
 pick：保留该commit（缩写:p）
 reword：保留该commit，但我需要修改该commit的注释（缩写:r）
 edit：保留该commit, 但我要停下来修改该提交(不仅仅修改注释)（缩写:e）
 squash：将该commit和前一个commit合并（缩写:s）
 fixup：将该commit和前一个commit合并，但我不要保留该提交的注释信息（缩写:f）
 exec：执行shell命令（缩写:x）
 drop：我要丢弃该commit（缩写:d）
```

## gitignore新增文件

git rm -r --cached .
git add .
git commit -m ''

## 安装SourceTree

<https://blog.csdn.net/RedaTao/article/details/104171068>
