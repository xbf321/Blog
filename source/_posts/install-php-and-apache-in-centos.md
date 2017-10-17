title: 在 Centos 中编译安装 php 和 Apache
date: 2017-10-12
categories: tech
tags: [php, centos, apache]
---

最近需要部署 piwik 统计系统，无奈使用的是 php 语言，好几年都没有部署过 php 环境了，折腾了两天才搞定。

最先想到的方式是用 yum 方式安装，但是不知道为何，总提示 Requires: libcrypto.so.10(OPENSSL_1.0.2)(64bit)，看到这个，第一反应是查看 openssl 的版本，确实比要求的要低，在官网下载一个最新的安装上去，可还是不行，在网上查找了好多方案，依旧不行，就单单因为这个问题，浪费了好长时间。

<!--more-->

yum 方式不行，只能退回 编译安装了，自己很少用编译安装，总觉得这个麻烦，但是经历过这次，发现编译安装的方式虽然慢点，但是自由度高，没想到最后安装成功了。

下面记录的安装过程，最好是先安装 Apache ，在安装 php ，因为 libphp.so 这个文件，需要写入到 Apache 的 modules 里边。



# 安装 Apache

```
cd /home/q/
sudo wget http://mirrors.tuna.tsinghua.edu.cn/apache/httpd/httpd-2.4.28.tar.gz
sudo tar -xf httpd-2.4.28.tar.gz
cd httpd-2.4.28
sudo ./configure --enable-so
```

如果提示：configure: error: APR not found. Please read the documentation. 则按以下步骤安装

```
cd ~

// 安装 apr
sudo wget http://archive.apache.org/dist/apr/apr-1.4.5.tar.gz
sudo tar -zxf apr-1.4.5.tar.gz
cd apr-1.4.5
sudo ./configure --prefix=/usr/local/apr
sudo make
sudo make install

// 安装 apr-util
sudo wget http://archive.apache.org/dist/apr/apr-util-1.3.12.tar.gz
sudo tar -zxf apr-util-1.3.12.tar.gz 
cd apr-util-1.3.12
sudo ./configure --prefix=/usr/local/apr-util -with-apr=/usr/local/apr 
sudo make
sudo make install

// 安装 pcre
sudo wget http://jaist.dl.sourceforge.net/project/pcre/pcre/8.10/pcre-8.10.zip
sudo unzip -o pcre-8.10.zip
cd pcre-8.10
sudo ./configure --prefix=/usr/local/pcre
sudo make
sudo make install
```

再次安装 Apache

```
sudo ./configure --enable-so --with-apr=/usr/local/apr --with-apr-util=/usr/local/apr-util/ --with-pcre=/usr/local/pcre
sudo make
sudo make install
```

## 测试安装是否成功

```
sudo /usr/local/apache2/bin/apachectl start
sudo /usr/local/apache2/bin/apachectl stop
```

访问 http://机器ip，如果提示 It works! ，证明启动成功。


# 安装 php7

## 安装依赖

```
sudo yum install epel-release libmcrypt-devel libxml2 libxml2-devel openssl openssl-devel bzip2 bzip2-devel libcurl libcurl-devel libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel gmp gmp-devel libmcrypt libmcrypt-devel readline readline-devel libxslt libxslt-devel
```


## 下载php

```
cd /home/q/
sudo wget http://hk1.php.net/get/php-7.1.10.tar.gz/from/this/mirror
sudo tar -xvf php-7.1.10.tar.gz
```

## 编译

```
cd php-7.1.10
sudo ./configure --prefix=/usr/local/php --with-config-file-path=/etc --with-apxs2=/usr/local/apache2/bin/apxs --with-mysql --with-gd --enable-mbstring --with-curl --enable-mbstring --enable-simplexml --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --with-zlib --with-zlib-dir
```

## 安装

```
sudo make
sudo make install
```


# 配置环境变量

```
sudo vim /etc/profile

// 结尾加入以下内容
PATH=$PATH:/usr/local/php/bin
export PATH

// 生效
source /etc/profile

// 运行
php -v 
```

# 配置 apache，启用 php


在 httpd.conf 中添加以下映射（sudo vim /usr/local/apache2/conf/httpd.conf）


```
<FilesMatch \.php$>
    SetHandler application/x-httpd-php
</FilesMatch>
```
测试 php 是否能解释，进入到 */usr/local/apache2/htdocs* 目录，新建 *info.php* 文件，添加 <?php phpinfo(); ?>


```
cd /usr/local/apache2/htdocs
sudo vim info.php
```


重启 Apache


```
sudo /usr/local/apache2/bin/apachectl stop
sudo /usr/local/apache2/bin/apachectl start
```

访问 http://机器ip/info.php ，如果能正常显示，说明配置成功。

参考：
* [http://php.net/manual/en/install.unix.apache2.php](http://php.net/manual/en/install.unix.apache2.php)
* [http://www.cnlvzi.com/index.php/Index/article/id/101](http://www.cnlvzi.com/index.php/Index/article/id/101)

