### 1.获取Git的升级
```git
git clone git://git.kernel.org/pub/scm/git/git.git
```

### 2.列出所有能找到的配置
```git
git config --list
```

### 3.检查某一项配置
```git
git config <key>
```
### 4.获取Git命令的使用手册
```git
git help <verb>
git <verb> --help
man git-<verb>
```

### 5.在现有目录中初始化仓库
```git
git init
```

### 6.克隆现有的仓库
```git
git clone [url] [mylibrary]
```

### 7.检查当前文件状态
```git
git status
```
### 8.添加内容到下一次提交中
```git
git add <file>
```

### 9.查看尚未暂存的文件更新了哪些部分
```git
git diff
```

### 10.查看已暂存的将要添加到下次提交里的内容
```git
git diff --cached
```

### 11.提交更新
```git
git commit -m <"msg">
```

### 12.跳过使用暂存区域
```git
git commit -a -m [msg]
```

### 13.移除文件
```git
git rm
```

### 14.强制移除已放到暂存区域的文件
```git
git rm -f
```

### 15.想让文件保留在磁盘，但是并不想让 Git 继续跟踪
```git
git rm --cached <file>
```

### 16.移动文件(重命名文件)
```git
git mv
```
### 17.查看提交历史
```git
git log [-p] [-num] [--stat]
```
### 18.重新提交
```git
git commit --amend
```

### 19.取消暂存文件
```git
git reset HEAD <file>
```

#### 20.撤消对文件的修改
```git
git checkout -- <file>
```

### 21.远程仓库使用的简写与其对应的URL
```git
git remote -v
```

### 22.添加远程仓库
```git
git remote add <shortname> <url>
```

### 23.从远程仓库中抓取与拉取
```git
git fetch [remote-name]
```

### 24.推送到远程仓库
```git
git push [remote-name] [branch-name]
```
### 25.查看远程仓库
```git
git remote show [remote-name]
```

### 26.远程仓库重命名
```git
git remote rename [remote-name] [new-remote-name]
```

### 27.远程仓库删除
```git
git remote rm [remote-name]
```

### 28.列出标签
```git
git tag
```

### 29.创建附注标签
```git
git tag -a [tagname] -m [msg]
```

### 30.创建轻量标签
```git
git tag [tagname]
```

### 31.后期打标签
```git
git tag -a [tagname] <key>
```

### 32.检出标签
```git
git checkout -b [branchname] [tagname]
```

### 1.本地代码彻底回退到某一版本
```git
git reset --hard <HARD>
```

### 2.强制提交
```git
git push origin master --force
```

### 3.丢弃当前全部暂存
```git
git checkout .
```
### 4.查看当前版本号
```git
git rev-parse HEAD
```
