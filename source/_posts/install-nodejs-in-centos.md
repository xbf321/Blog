title: 在 Centos 中编译安装 nodejs
date: 2017-05-18
categories: tech
tags: [nodejs,centos]
---

且看且珍惜。

<!--more-->

## 准备依赖包

```
sudo yum -y install gcc make gcc-c++ openssl-devel wget
```

## 下载源码并解压

```
sudo wget http://nodejs.org/dist/v6.10.3/node-v6.10.3.tar.gz
sudo tar -zvxf node-v6.10.3.tar.gz
```

## 配置

```
sudo ./configure
```

## 编译及安装

```
sudo make
sudo make install
```

## 验证

```
node -v
```