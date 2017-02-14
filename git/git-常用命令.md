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
git clone [url][mylibrary]
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
git commit -a -m <"msg">
```

### 13.移除文件
```git
git rm
```

### 14.强制移除已放到暂存区域的文件
```git
git rm -f
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
