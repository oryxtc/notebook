## 1.在目录中新建`Dockerfile`文件

## 2.编辑`Dokcerfile`输入
```
#基于php-5.6-fpm基础镜像
FROM php:5.6-fpm

#执行命令 
RUN  apt-get update \	#更新软件包
	&& pecl install redis-2.2.7 \	#pecl方式安装redis扩展
	&& docker-php-ext-install -j$(nproc) bcmath pdo pdo_mysql \	#php基础镜像内置方法安装其他核心扩展
	&& docker-php-ext-enable redis \	#php基础镜像内置方法开启redis扩展
```
