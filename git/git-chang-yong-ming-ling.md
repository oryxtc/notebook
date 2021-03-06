---
title: git常用命令
date: '2018/3/28 16:37:00'
categories: git
tags: git
description: 整理了下git常用的命令
---

# git常用命令

## 获取Git的升级

```text
git clone git://git.kernel.org/pub/scm/git/git.git
```

## 列出所有能找到的配置

```text
git config --list
```

## 检查某一项配置

```text
git config <key>
```

## 获取Git命令的使用手册

```text
git help <verb>
git <verb> --help
man git-<verb>
```

## 在现有目录中初始化仓库

```text
git init
```

## 克隆现有的仓库

```text
git clone [url] [mylibrary]
```

## 检查当前文件状态

```text
git status
```

## 添加内容到下一次提交中

```text
git add <file>
```

## 查看尚未暂存的文件更新了哪些部分

```text
git diff
```

## 查看已暂存的将要添加到下次提交里的内容

```text
git diff --cached
```

## 提交更新

```text
git commit -m <"msg">
```

## 跳过使用暂存区域

```text
git commit -a -m [msg]
```

## 移除文件

```text
git rm
```

## 强制移除已放到暂存区域的文件

```text
git rm -f
```

## 想让文件保留在磁盘，但是并不想让 Git 继续跟踪

```text
git rm --cached <file>
```

## 移动文件\(重命名文件\)

```text
git mv
```

## 查看提交历史

```text
git log [-p] [-num] [--stat]
```

## 重新提交

```text
git commit --amend
```

## 取消暂存文件

```text
git reset HEAD <file>
```

## 撤消对文件的修改

```text
git checkout -- <file>
```

## 远程仓库使用的简写与其对应的URL

```text
git remote -v
```

## 添加远程仓库

```text
git remote add <shortname> <url>
```

## 从远程仓库中抓取与拉取

```text
git fetch [remote-name]
```

## 从远程仓库分支拉取到本地\(会在本地新建分支，自动切换到该本地分支,并本地分支会和远程分支建立映射关系\)

```text
git checkout -b [branch-name] [remote-name]/[branch-name]
```

## 从远程仓库分支拉取到本地\(会在本地新建分支，但是不会自动切换到该本地分支,并本地分支不会和远程分支建立映射关系\)

```text
git fetch [remote-name] [branch-name]:[branch-name]
```

## 当前分支与远程仓库分支建立映射关系

```text
git branch --set-upstream-to [remote-name]/[branch-name]
```

## 指定分支与远程仓库分支建立映射关系

```text
git branch --set-upstream [branch-name] [remote-name]/[branch-name]
```

## 查看远程仓库

```text
git remote show [remote-name]
```

## 远程仓库重命名

```text
git remote rename [remote-name] [new-remote-name]
```

## 远程仓库删除

```text
git remote rm [remote-name]
```

## 列出标签

```text
git tag
```

## 创建附注标签

```text
git tag -a [tagname] -m [msg]
```

## 创建轻量标签

```text
git tag [tagname]
```

## 后期打标签

```text
git tag -a [tagname] <key>
```

## 检出标签

```text
git checkout -b [branchname] [tagname]
```

## 创建别名

```text
git config --global [alias.alias-name] [command]
```

## 分支创建

```text
git branch [branc-name]
```

## 分支切换

```text
git checkout [branch-name]
```

## 删除本地分支

```text
git branch -d [branch-name]
```

## 列出所有分支

```text
git branch
```

## 推送分支至远程

```text
git push (remote) (branch)
```

## 删除远程分支

```text
git push (remote) --delete (branch)
```

## 变基

```text
git rebase [basebranch]
```

## 将指定分支变基到目标分支

```text
git rebase [basebranch] [topicbranch]
```

## 取出 client 分支，找出处于 client 分支和 server 分支的共同祖先之后的修改，然后把它们在 master 分支上重放一遍

```text
git rebase --onto master server client
```

## 用变基解决变基

```text
git pull --rebase
```

## 把现有仓库导出为裸仓库

```text
git clone --bare [library-name] [newlibrary-name]
```

## 复制你的裸仓库来创建一个新仓库

```text
scp -r my_project.git user@git.example.com:/opt/git
```

## 强制提交

```text
git push [remote-name] [branch-name] --force
```

## 丢弃当前全部暂存

```text
git checkout .
```

## 查看当前版本号

```text
git rev-parse HEAD
```

## 全局提交用户名与邮箱

```text
git config --global user.name "Yuchen Deng"
git config --global user.email 邮箱名@gmail.com
```

## 中文编码支持

```text
git config --global gui.encoding utf-8
git config --global i18n.commitencoding utf-8
git config --global i18n.logoutputencoding utf-8
```

## 全局编辑器，提交时将COMMIT\_EDITMSG编码转换成UTF-8可避免乱码

```text
git config --global core.editor notepad2
```

## 差异工具配置

```text
git config --global diff.external git-diff-wrapper.sh
git config --global diff.tool tortoise
git config --global difftool.tortoise.cmd 'TortoiseMerge -base:"$LOCAL" -theirs:"$REMOTE"'
git config --global difftool.prompt false
```

## 合并工具配置

```text
git config --global merge.tool tortoise
git config --global mergetool.tortoise.cmd 'TortoiseMerge -base:"$BASE" -theirs:"$REMOTE" -mine:"$LOCAL" -merged:"$MERGED"'
git config --global mergetool.prompt false
```

## 别名设置

```text
git config --global alias.dt difftool
git config --global alias.mt mergetool
```

