---
title: git常用命令
date: 2017/1/14 20:46:25
categories: git
tags: git
description: 整理了下git常用的命令
---

### 获取Git的升级
```git
git clone git://git.kernel.org/pub/scm/git/git.git
```

### 列出所有能找到的配置
```git
git config --list
```

### 检查某一项配置
```git
git config <key>
```
### 获取Git命令的使用手册
```git
git help <verb>
git <verb> --help
man git-<verb>
```

### 在现有目录中初始化仓库
```git
git init
```

### 克隆现有的仓库
```git
git clone [url] [mylibrary]
```

### 检查当前文件状态
```git
git status
```
### 添加内容到下一次提交中
```git
git add <file>
```

### 查看尚未暂存的文件更新了哪些部分
```git
git diff
```

### 查看已暂存的将要添加到下次提交里的内容
```git
git diff --cached
```

### 提交更新
```git
git commit -m <"msg">
```

### 跳过使用暂存区域
```git
git commit -a -m [msg]
```

### 移除文件
```git
git rm
```

### 强制移除已放到暂存区域的文件
```git
git rm -f
```

### 想让文件保留在磁盘，但是并不想让 Git 继续跟踪
```git
git rm --cached <file>
```

### 移动文件(重命名文件)
```git
git mv
```
### 查看提交历史
```git
git log [-p] [-num] [--stat]
```
### 重新提交
```git
git commit --amend
```

### 取消暂存文件
```git
git reset HEAD <file>
```

### 撤消对文件的修改
```git
git checkout -- <file>
```

### 远程仓库使用的简写与其对应的URL
```git
git remote -v
```

### 添加远程仓库
```git
git remote add <shortname> <url>
```

### 从远程仓库中抓取与拉取
```git
git fetch [remote-name]
```

### 推送到远程仓库
```git
git push [remote-name] [branch-name]
```
### 查看远程仓库
```git
git remote show [remote-name]
```

### 远程仓库重命名
```git
git remote rename [remote-name] [new-remote-name]
```

### 远程仓库删除
```git
git remote rm [remote-name]
```

### 列出标签
```git
git tag
```

### 创建附注标签
```git
git tag -a [tagname] -m [msg]
```

### 创建轻量标签
```git
git tag [tagname]
```

### 后期打标签
```git
git tag -a [tagname] <key>
```

### 检出标签
```git
git checkout -b [branchname] [tagname]
```

### 创建别名
```git
git config --global [alias.alias-name] [command]
```

### 分支创建
```git
git branch [branc-name]
```

### 分支切换
```git
git checkout [branch-name]
```

### 删除本地分支
```git
git branch -d [branch-name]
```

### 列出所有分支
```git
git branch
```
### 推送分支至远程
```git
git push (remote) (branch)
```

### 删除远程分支
```git
git push (remote) --delete (branch)
```

### 变基
```git
git rebase [basebranch]
```

### 将指定分支变基到目标分支
```git
git rebase [basebranch] [topicbranch]
```

### 取出 client 分支，找出处于 client 分支和 server 分支的共同祖先之后的修改，然后把它们在 master 分支上重放一遍
```git
git rebase --onto master server client
```

### 43.用变基解决变基
```git
git pull --rebase
```

### 44.把现有仓库导出为裸仓库
```git
git clone --bare [library-name] [newlibrary-name]
```

### 45.复制你的裸仓库来创建一个新仓库
```git
scp -r my_project.git user@git.example.com:/opt/git
```

### 46.强制提交
```git
git push [remote-name] [branch-name] --force
```

### 3.丢弃当前全部暂存
```git
git checkout .
```
### 4.查看当前版本号
```git
git rev-parse HEAD
```
