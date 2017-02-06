###alias与root的区别

**1.如果配置项为alias**
```nginx
location /img/ {
    alias /var/www/image/;
}
```
>若按照上述配置的话，则访问/img/目录里面的文件时，ningx会去/var/www/image/目录找文件

**2.如果配置项为root**
```nginx
location /img/ {
    root /var/www/image;
}
```
>若按照上述配置的话，则访问/img/目录里面的文件时，ningx会去/var/www/image/img/目录找文件


**3.图片胜过千言万语**
**for alias**
![nginx-alias](/assets/nginx-alias.png)
**for root:**
![nginx-root](/assets/nginx-root.png)


