---
title: linux常用命令
date: 2017/1/12 20:46:25
categories: linux
description: '归纳总结了一些linux常用命令'
---

### 文件、目录类
|命令| 详解|
|---|---|
|cd|# 返回 home 目录（相当于cd ~）|
|cd ..|# 返回上一级目录|
|cd -|# 返回上一次所在目录,并显示其目录名|
|cd xxx|# 进入到指定目录xxx|
|pwd|# 显示当前目录的绝对路径|
|ls -l|# 列出文件的详细信息，相当于（ll）|
|ls \| grep "xxx"|# 列出包含 "xxx" 关键字的文件|
|mkdir dir|# 创建一个目录|
|mkdir -p dir/dir|# 创建多级目录|
|mkdir -m 777 dir|# 创建权限为 777 的目录|
|touch file|# 创建新的空文件|
|rmdir dir|# 删除空目录|
|rmdir -p dir/bin|# 删除子空目录 bin 和其父空目录 dir|
|rm -rf dir/bin|# 删除一个目录中的一个或多个文件或目录（慎用）|
|rm -rf xxx *.log|# 删除当前目录下所有 ".log" 的文件（慎用）|
|find fileName -name  *.txt \| xargs rm -rf|# 将查找出来的文件全部删除（慎用）|
|cp file dir/file|# 将文件拷贝到另一文件中|
|cp -R dir1 dir2|# 拷贝多个目录 (含子目录) 到指定目录|
|mv dir1 dir2|# 将文件或目录重新命名,或者将文件从一个目录移到另一个目录中|

### 文件查看、处理
|命令| 详解|
|---|---|
|cat file|# 显示文件的内容|
|cat -n file|# 显示文件的行数编号|
|cat file1 file2 > file3|# 将文件 file1 和 file2 的内容合并之后放入 新文件 file3 中|
|head file|# 显示文件的头 10 行内容|
|tail file|# 显示文件的最后 10 行内容|
|tail -f file|# 显示文件最新追加的内容,并监视文件的变化,常用来跟踪日志文件|
|more file|# 基于vi编辑器文本过滤器,它以全屏幕的方式按页显示文本文件的内容|
|less file|# 作用与 more 十分类似, pageUp 向上翻页, pageDown 向下翻页, 按 q 退出|
|cat file1 > file2|# 覆盖导入|
|cat file1 >> file|# 追加导入|
|wc -l [-m][-c][-w] file|# 统计行数、字符数、字节数、单词数|

### 文件查看、处理
|命令| 详解|
|---|---|
|grep keyWord|# 与 cat 或者其他命令搭配使用 cat file \| grep keyWord|
|find dir -name "*.log"|# 搜索指定目录下的后缀为 .log 的文件|
|find dir -name "*.log" -o -name "*.pid"|# 搜索指定目录下的 ".log" 文件和 ".pid" 文件
|find dir -user user1|# 搜索指定目录下属于 user1 用户的文件|

### 文本编辑 vi / vim 底行模式下
|命令| 详解|
|---|---|
|:set nu|# 显示行号|
|:set nonu|# 不显示行号|
|:n|# 跳转到指定第 n 行|
|:w file|# 另存为|
|:n1,n2 s/str1/str2/g|# 从 n1 行到 n2 行, 将 str1 替换为 str2（从开头到结束 1,$ s/str1/str2/g）|
|:wq|# 保存并退出|
|:q!|# 强制退出不保存|

### 文本编辑 vi / vim 命令模式下
|命令| 详解|
|---|---|
|G|# 到末行（Shift + g）|
|gg|# 到首行|
|dd|# 删除行或剪切行|
|u|# 撤销|
|y|# 在使用 v 模式选定了某一块的时候，复制选定块到缓冲区用|
|yw|# 复制一个word （nyw或者ynw，复制n个word，n为数字）|
|yy|# 复制一行|
|nyy|# 向下复制 n 行|
|p|# 粘贴|

### 权限管理
|命令| 详解|
|---|---|
|chmod 755 dir/file|# 修改指定文件、文件夹的权限|
|chmod -R 755 dir|# 递归修改目录及其子文件、目录的权限|
|chown user file|# 改变文件的所有者|
|chown -R user dir|# 改变目录的所有者|
|chgrp group1 file|# 改变文件的所有者|
|chgrp -R group1 dir|# 改变目录的所属组|
|chown user1:group1 file|# 同时改变文件的所有者和所属组|
|chown -R user1:group1 dir|# 同时改变目录的所有者和所属组|
|whoami|# 查看当前操作用户|
|who|# 查看当前已登录系统的用户|
|id user1|# 查看用户 user1 的归属 id 信息|

### 压缩、解压
|命令| 详解|
|---|---|
|tar -cvf test.tar test.log|# 仅打包,不压缩|
|tar -xvf test.tar|# 直接解包|
|tar -zcvf test.tar.gz test.log|# 打包后,以 gzip 压缩|
|tar -zcvf test.tar.gz *|# 将当前目录下所有文件压缩|
|tar -zxvf test.tar.gz|# 直接解压|
|tar -zxvf test.tar.gz -C dir|# 解压到新目录,只能是 dir 且已经存在|
|zip test.zip *|# 将当前目录下所有文件压缩为 zip 包|
|unzip test.zip|# 解压缩 zip 包|

### 用户管理
|命令| 详解|
|---|---|
|groupadd group1|# 创建用户组|
|groupdel group1|# 删除用户组|
|groupmod  -n group2 group1|# 将 group1 重命名 group2|
|useradd user1|# 创建用户|
|useradd -g group1 user1|# 创建 user1 并将其分配到 group1 组下|
|userdel -r user1|# 删除 user1, "-r" 参数表示同时也删除 home 目录下的相关目录|
|usermod -g group2 user1|# 改变 user1 的组为 group2|
|usermod -G group2 user1|# 将 user1 的添加到 group2 组中来，同时保留原来的主组|
|passwd|# 修改当前用户密码|
|passwd user1|# 修改 user1 用户的密码，仅限 root 用户执行|
|su user1|# 切换到用户 user1|
|groups user1|# 查看用户 user1 所属的组|

### 查看命令帮助
|命令| 详解|
|---|---|
|help cd|# 用于内部命令, 如 exit、history、cd、echo 等常驻内存|
|ls --help|# 主要用于外部命令,可通过 "echo $PATH" 命令查看外部命令的存储路径,如 ls,vi 等|
|man ls|# 命令手册,可用于所有命令,输入"q"可退出|
|type cd|# 查看命令类型,内部 or 外部及命令位置|

### 系统相关命令
|命令| 详解|
|---|---|
|shutdown  -h now|# 立即关机|
|shutdown  -r now|# 立即重启|
|uname -r|# 查看内核版本|
|cal|# 日历|
|date|# 时间、日期|
|date -s "2015-12-22 08:00"|# 修改时间|
|ntpdate time.nist.gov|# 同步当前时间|
|history|# 查看历史命令记录,运行时 "!"+ 命令号,如 !123 运行 编号为 123 的命令|
|ps -ef|# 查看进程|
|kill pid|# 终止进程|
|kill -9 pid|# 强制终止进程|
|top|# 查看当前系统资源使用率|
|df -h|# 查看磁盘信息|
|free -m|# 查看内存信息|
|du -h file/dir|# 查看单个文件/目录大小 -h 表示以 K,M,G|
|du -sh file/dir|# 查看文件/目录总大小|
|mount -o loop linux.iso /mnt/dir|# 加载文件系统到指定的加载点|
|umount /mnt/dir|# 卸载已经加载的文件系统|
|rpm -ivh xxx.rpm|# 安装 rpm 包|
|rpm -e xxx.rpm|# 卸载 rpm 包|
|yum install xxx|# 安装安装包xxx|
|yum remove xxx|# 删除已安装的xxx|
|wget http://xxxx|# 下载远端 zip 包|
|echo '' > xxx|# 清空xxx文件内容|
|netstat -an \| grep 80|# 80端口是否被监听|


