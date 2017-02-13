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
