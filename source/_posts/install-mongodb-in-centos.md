title: 在CentOs中安装MongoDB(1)
date: 2015-01-29
categories: tech
---

备忘。

<!--more-->

## 安装说明

* 系统环境：CenteOS 6.4（64位）
* 下载地址：[http://fastdl.mongodb.org/linux/mongodb-linux-x86_64-2.6.7.tgz](http://fastdl.mongodb.org/linux/mongodb-linux-x86_64-2.6.7.tgz)
* 安装位置：/usr/local/mongodb
* 数据位置：/var/mongodb/data
* 日志位置：/var/mongodb/logs

## 配置防火墙

将27017端口加入防火墙

```
vi /etc/sysconfig/iptables
```

重启防火墙

```
service iptables restart
```

## 安装

源文件存放在 `/usr/local/src`目录

解压：

```
tar zxvf mongodb-linux-x86_64-2.6.7.tgz
```

移动至 `/usr/local/mongodb` 目录中

```
mv mongodb-linux-x86_64-2.6.7 /usr/local/mongodb
```

新建`data`和`logs`目录

```
mkdir /var/mongodb
mkdir /var/mongodb/data
mkdir /var/mongodb/logs
```

## 后台启动MongoDB

```
/usr/local/mongodb/bin/mongod --dbpath=/var/mongodb/data --logpath=/var/mongodb/logs/log.log --fork

```

显示以下内容说明成功

```
about to fork child process, waiting until server is ready for connections.
forked process: 27589
child process started successfully, parent exiting
```

## 设置开机启动

打开rc.local


```
vi + /etc/rc.d/rc.local
```

把MongoDB启动命令追加到文件中

```
/usr/local/mongodb/bin/mongod --dbpath=/var/mongodb/data --logpath=/var/mongodb/logs/log.log --fork
```


## 查看MongoDB进程

```
ps aux |grep mongodb
```

显示：

```
root     27589  0.2  0.8 633080 71924 ?        Sl   11:06   0:32 /usr/local/mongodb/bin/mongod --dbpath=/var/mongodb/data --logpath=/var/mongodb/logs/log.log --fork
```

## 测试

进入MongoDB的shell模式

```
/usr/local/mongodb/bin/mongo
```

查看数据列表

```
show dbs
```

显示当前db版本

```
db.version()
```