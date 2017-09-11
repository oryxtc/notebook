---
title: github自动部署
date: 2017/9/11 18:15:25
categories: linux
description: '通过node.js+webhook 实现github的自动部署'
---

### 以下命令,均是在Debian系统环境下,若是其他系统,使用对应命令即可

### 安装node.js
```bash
sudo apt-get install nodejs
```

### 安装npm
```bash
sudo apt-get install npm
```

### 我这里新建了一个目录作为自动部署服务的根目录,并进入该目录
```
mkdir /home/webhook;
cd /home/webhook;
```

### 在当前文件夹下,新建一个`deploy.js`作为监听程序,内容如下
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
  run_cmd('sh', ['./deploy.sh',name], function(text){ console.log(text) });
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
### 在当前文件夹下新建一个`deploy.sh`脚本作为执行,内容如下
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
### 这里需要用到node.js的中间件`github-webhook-handler`
```bash

