## 1.为了更好遵循`Compose file version 3`,且我的项目使用本地目录,这里我先引入了`Volume plugins`
`Volume plugins`官方列举的清单地址为:[https://docs.docker.com/engine/extend/legacy_plugins/#volume-plugins](https://docs.docker.com/engine/extend/legacy_plugins/#volume-plugins)  

## 2.我这里使用容器来实现`local-persist`驱动
创建容器命令行输入:
```bash
docker run -d --restart always --name volume-plugin-local-persist -v /run/docker/plugins/:/run/docker/plugins/   cwspear/docker-local-persist-volume-plugin
```

## 3.创建`docker-compose.yml`文件,输入:
```
version: '3'
services:
    nginx:
        image: nginx:1.10.3
        container_name: xxx-nginx
        restart: always    #重启方案
        ports:    #端口映射
        - 80:80    
        - 443:443
        links:    #容器链接
        - php
        volumes:
        - /d/www/docker/dhb168/nginx-config:/etc/nginx/conf.d  #nginx配置文件目录
        - oryxtc-volume:/home
    php:
        image: xxx/xxx-php
        container_name: xxx-php
        restart: always
        volumes:
        - oryxtc-volume:/home 
volumes:    #这里会自动创建docker volume
    oryxtc-volume:
        driver: local-persist #卷驱动使用local-persist
        driver_opts:
            mountpoint: /d/www/oryxtc    #项目总目录
```
>注意:因为这里卷的创建依赖卷插件`local-persist` 所以要保证之前创建的容器`volume-plugin-local-persist`在运行中

## 4.建立项目服务
命令行进入项目根目录,命令行输入:
```bash
docker-compose build
```
## 5.启动并运行项目
命令行输入:
```bash
docker-compose up
```