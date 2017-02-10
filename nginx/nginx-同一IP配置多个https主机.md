###nginx 同一个IP上配置多个HTTPS主机

如果在同一个IP上配置多个HTTPS主机，会出现一个很普遍的问题

```nginx
server {
    listen          443;
    server_name     www.example.com;
    ssl             on;
    ssl_certificate www.example.com.crt;
    ...
}
 
server {
    listen          443;
    server_name     www.example.org;
    ssl             on;
    ssl_certificate www.example.org.crt;
    ...
}
```
使用上面的配置，不论浏览器请求哪个主机，都只会收到默认主机www.example.com的证书。这是由SSL协议本身的行为引起的——先建立SSL连接，再发送HTTP请求，所以nginx建立SSL连接时不知道所请求主机的名字，因此，它只会返回默认主机的证书。
最古老的也是最稳定的解决方法就是每个HTTPS主机使用不同的IP地址：

```nginx
server {
    listen          192.168.1.1:443;
    server_name     www.example.com;
    ssl             on;
    ssl_certificate www.example.com.crt;
    ...
}
 
server {
    listen          192.168.1.2:443;
    server_name     www.example.org;
    ssl             on;
    ssl_certificate www.example.org.crt;
    ...
}
```

那么，在同一个IP上，如何配置多个HTTPS主机呢？
nginx支持TLS协议的SNI扩展（Server Name Indication，简单地说这个扩展使得在同一个IP上可以以不同的证书serv不同的域名）。不过，SNI扩展还必须有客户端的支持，另外本地的OpenSSL必须支持它。
如果启用了SSL支持，nginx便会自动识别OpenSSL并启用SNI。是否启用SNI支持，是在编译时由当时的 ssl.h 决定的（SSL_CTRL_SET_TLSEXT_HOSTNAME），如果编译时使用的OpenSSL库支持SNI，则目标系统的OpenSSL库只要支持它就可以正常使用SNI了。
nginx在默认情况下是TLS SNI support disabled。
启用方法：
需要重新编译nginx并启用TLS。步骤如下：
```
# wget http://www.openssl.org/source/openssl-1.0.1e.tar.gz
# tar zxvf openssl-1.0.1e.tar.gz 
# ./configure --prefix=/usr/local/nginx --with-http_ssl_module \
--with-openssl=./openssl-1.0.1e \
--with-openssl-opt="enable-tlsext" 
# make
# make install
```
查看是否启用：
```
# /usr/local/nginx/sbin/nginx -V
TLS SNI support enabled
```
这样就可以在 同一个IP上配置多个HTTPS主机了。
实例如下：

```
server  {
        listen 443;
        server_name   www.ttlsa.com;
        index index.html index.htm index.php;
        root  /data/wwwroot/www.ttlsa.com/webroot;
        ssl on;
        ssl_certificate "/usr/local/nginx/conf/ssl/www.ttlsa.com.public.cer";
        ssl_certificate_key "/usr/local/nginx/conf/ssl/www.ttlsa.com.private.key";   
        ...
} 
 
server  {
        listen 443;
        server_name   www.heytool.com;
        index index.html index.htm index.php;
        root  /data/wwwroot/www.heytool.com/webroot;
        ssl on;
        ssl_certificate "/usr/local/nginx/conf/ssl/www.heytool.com.public.cer";
        ssl_certificate_key "/usr/local/nginx/conf/ssl/www.heytool.com.private.key";   
        ...
}
```