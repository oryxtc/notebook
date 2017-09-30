---
title: github自动部署
date: 2017/9/11 18:15:25
categories: linux
description: '通过node.js+webhook 实现github的自动部署'
---

### 以下命令,均是在Debian系统环境下,若是其他系统,使用对应系统命令即可

### 安装node.js版本8.x
```bash
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get install -y nodejs
```

### 我这里新建了一个目录作为自动部署服务的根目录,并进入该目录
```
mkdir /home/webhook;
cd /home/webhook;
```

### 在当前文件夹下,新建一个`deploy.js`作为监听程序,内容如下
> 以下文件已上传gist 可使用命令
>```bash
curl -O https://gist.githubusercontent.com/oryxtc/b0bb50c210e25207dc67132d778714b6/raw/0b75f714064e232594fbaaf0f753bc9bf25d43df/deploy.js
```

```js
var http = require('http')
var createHandler = require('github-webhook-handler')
var handler = createHandler({ path: '/', secret: 'your-secret' }) 
// 上面的 path 即是github中填写的url的path部分
// 上面的 secret 保持和 GitHub 后台设置的一致
 
function run_cmd(cmd, args, callback) {
  var spawn = require('child_process').spawn;
  var child = spawn(cmd, args);
  var resp = "";
 
  child.stdout.on('data', function(buffer) { resp += buffer.toString(); });
  child.stdout.on('end', function() { callback (resp) });
}
 
http.createServer(function (req, res) {
 handler(req, res, function (err) {
  res.statusCode = 404
  res.end('no such location')
  })
}).listen(7777)
// listen(7777)指监听7777端口,可以根据实际情况改成你自己的
 
handler.on('error', function (err) {
    console.error('Error:', err.message)
})
 
handler.on('push', function (event) {
 var name=event.payload.repository.name;
 console.log('Received a push event for %s to %s',
    event.payload.repository.name,
    event.payload.ref);
  //如果是notebook 则为hexo自动部署进入mybloy(根据自己情况修改文件夹名)
  if(name==='notebook'){
      run_cmd('sh', ['./deploy_hexo.sh','myblog'], function(text){ console.log(text) });
  }else {
      run_cmd('sh', ['./deploy.sh',name], function(text){ console.log(text) });
  }
})
//这里为了实现不同仓库的自动部署,传了仓库名给shell脚本 

handler.on('issues', function (event) {
  console.log('Received an issue event for % action=%s: #%d %s',
    event.payload.repository.name,
    event.payload.action,
    event.payload.issue.number,
    event.payload.issue.title)
})
```
### 在当前文件夹下新建一个`deploy.sh`脚本执行自动拉取,内容如下
> 以下文件已上传gist 可使用命令
>```bash
curl -O https://gist.githubusercontent.com/oryxtc/3850a573f0b6a0e7eb783658863d08cb/raw/2c34639a6eef24c10fb6f111fc0129d48036028c/deploy.sh
```

```bash
WEB_NAME="$1"
WEB_PATH='/home/www/'${WEB_NAME}
WEB_USER='root'
WEB_USERGROUP='root'

echo "Start deployment"
cd $WEB_PATH
echo "pulling source code..."
git reset --hard origin/master
git clean -f
git pull
git checkout master
echo "changing permissions..."
#chown -R $WEB_USER:$WEB_USERGROUP $WEB_PATH;
echo "Finished."
```
### 在当前文件夹下新建一个`deploy_hexo.sh`脚本执行自动拉取,以及hexo部署和提交.(根据自己情况 修改路径等)
> 以下文件已上传gist 可使用命令
>```bash
curl -O https://gist.githubusercontent.com/oryxtc/eac1ec324ed295cddcae7c6767bc09f8/raw/dc73f5ee12cd9346530b8b415df3fb0d081b5e93/deploy_hexo.sh
```

```bash
WEB_NAME="$1"
WEB_PATH='/home/www/'${WEB_NAME}
WEB_SOURCE_PATH=${WEB_PATH}'/source/_posts'
WEB_USER='root'
WEB_USERGROUP='root'
echo "Start deployment"
cd $WEB_SOURCE_PATH
echo "pulling source code..."
git reset --hard origin/master
git clean -f
git pull
git checkout master
echo "changing permissions..."
#chown -R $WEB_USER:$WEB_USERGROUP $WEB_PATH;
echo "start hexo deployment"
cd $WEB_PATH
hexo d -g
echo "Finished."
```
>  这里要注意,如果是在Windows环境下编写文件,换行符会跟linux不一样!要用编辑器转换成UNIX格式

### 这里需要用到node.js的中间件`github-webhook-handler`,安装到当前目录下
```bash
npm install github-webhook-handler
```

### 为了自动部署能后台自动运行,并且断线自动重运行,这里使用`pm2`组件
```bash
npm install -g pm2
```
>如果出现 `/usr/bin/pm2 -> /usr/lib/node_modules/pm2/bin/pm2`
>使用软连接 `ln -s /usr/lib/node_modules/pm2/bin/pm2 /usr/bin/pm2`

### 用`pm2`运行该进程
```bash
pm2 start deploy.js --name auto-deploy # 命名进程
```

### 最后在github的项目中配置webhook
![](http://ooqid2far.bkt.clouddn.com/myblog/github%E8%87%AA%E5%8A%A8%E9%83%A8%E7%BD%B2-github.png)