---
title: nginx配置支持https
date: 2017/10/27
categories: nginx
tags: nginx https
description: 使用开源的certbot生成https证书,并配置nginx.conf以支持https
---

### automated方式安装certbot

> 网址链接:[certbot](https://certbot.eff.org/),根据自己系统选择,我这里选择的是 Debian 8

```bash
sudo apt-get install certbot -t jessie-backports
```
### 配置nginx.conf
**因为cerbot会对`域名/.well-known/acme-challenge/`发送一条请求,以验证你的域名与对应的项目目录是否匹配,这里对nginx.conf配置新增以下代码**
```nginx
server {
  ###原配置
  
  ### 新增对/.well-known/acme-challenge/请求的代理
  location ^~ /.well-known/acme-challenge/ {
	default_type "text/plain";
  }
}
```

### 开始生成证书
```bash
sudo certbot certonly
```

### 验证方式
**以什么方式验证身份,这里推荐输入 1,选择项目根目录方式.**
```bash
root@iZwz978masqmg60f7ex16yZ:/home/docker/website/nginx# sudo certbot certonly
Saving debug log to /var/log/letsencrypt/letsencrypt.log

How would you like to authenticate with the ACME CA?
-------------------------------------------------------------------------------
1: Place files in webroot directory (webroot)
2: Spin up a temporary webserver (standalone)
-------------------------------------------------------------------------------
Select the appropriate number [1-2] then [enter] (press 'c' to cancel): 1
```

### 输入你的域名
```bash
Please enter in your domain name(s) (comma and/or space separated)  (Enter 'c'
to cancel):outer-performance.oryxtc.xyz
```

### 选择域名对应的项目根目录
```bash
Select the webroot for outer-performance.oryxtc.xyz:
-------------------------------------------------------------------------------
1: Enter a new webroot
-------------------------------------------------------------------------------
Press 1 [enter] to confirm the selection (press 'c' to cancel): 1
Input the webroot for outer-performance.oryxtc.xyz: (Enter 'c' to cancel):/home/www/outer-performance/public
```
> 我使用的laravel框架,入口文件在`public`文件夹下,这里根目录地址填写`public`文件夹路径

### 验证成功
**域名验证完成,会在`/etc/letsencrypt/live/域名名称` 文件下生成密钥**
```bash
Waiting for verification...
Cleaning up challenges
Generating key (2048 bits): /etc/letsencrypt/keys/0002_key-certbot.pem
Creating CSR: /etc/letsencrypt/csr/0002_csr-certbot.pem

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at
   /etc/letsencrypt/live/outer-performance.oryxtc.xyz/fullchain.pem.
   Your cert will expire on 2018-01-25. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot
   again. To non-interactively renew *all* of your certificates, run
   "certbot renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```

### 修改nginx.conf
```nginx
server {
  ### 原配置
  
  ###新增对https的代理
  listen       443 ssl;
  
  ### 证书路径
  ssl_certificate /etc/letsencrypt/live/outer-performance.oryxtc.xyz/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/outer-performance.oryxtc.xyz/privkey.pem;
}
```

### 输入`https://域名`,查看已成功
![](http://ooqid2far.bkt.clouddn.com/myblog/nginx%E9%85%8D%E7%BD%AE%E6%94%AF%E6%8C%81https.png)


### 证书自动延期
**检测证书是否合法**
```bash
sudo certbot renew --dry-run
```
**
如果有显示`Congratulations, all renewals succeeded`,执行证书自动延期**
```bash
certbot renew 
```
