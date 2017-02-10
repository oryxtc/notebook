```
<Directory "E:/mywork">
    Options Indexes FollowSymLinks   //允许访问目录
    AllowOverride all          //设置htaccess
    Require all granted       //要求所有批准
    Order allow,deny         //允许 与 阻止
    Allow from all            //允许所有用户
    # 开启 mod_rewrite 用于美化 URL 功能的支持（译注：对应 pretty URL 选项)
    RewriteEngine on
    # 如果请求的是真实存在的文件或目录，直接访问
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    # 如果请求的不是真实文件或目录，分发请求至 index.php
    RewriteRule . index.php
</Directory>
<VirtualHost *:80>
    ServerAdmin www.mywork.com
    DocumentRoot "E:/mywork"
    ServerName mywork
</VirtualHost>
```

