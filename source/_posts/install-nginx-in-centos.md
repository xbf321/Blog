title: 在 Centos 中安装 nginx
date: 2017-05-18
categories: tech
tags: [nginx,centos]
---

且看且珍惜。

<!--more-->

## 准备依赖包

```
sudo yum install gcc-c++
sudo yum -y install zlib zlib-devel openssl openssl--devel pcre pcre-devel
```

## 检查是否已经安装

```
find -name nginx
```

## 卸载原有 nginx

```
sudo yum remove nginx
```

## 安装 nginx

```
sudo yum install nginx
```

## 基本命令

```
    启动：./nginx 
    停止：./nginx -s stop 
    重启：./nginx -s reload 
    测试：./nginx -t
    说明：nginx命令在nginx/sbin目录下,需要sudo权限。 要加上sudo来启动nginx。
         其中的reload可以重新加载配置文件，stop停止ngnix，-t用来检查配置文件的完整性和正确性。
	 执行完成命令后，如果没有任何提示则可以基本确认命令执行成功了（linux中，没有消息就是好消息）
```

## 启动过程中遇到的问题

首先执行以下命令，查看 nginx 缺失的包

```
ldd $(which /home/q/nginx/sbin/nginx)
```

显示：

```
    linux-vdso.so.1 =>  (0x00007ffecaca4000)
	libcrypt.so.1 => /lib64/libcrypt.so.1 (0x00007f546037e000)
	libluajit-5.1.so.2 => /home/q/luajit/lib/libluajit-5.1.so.2 (0x00007f546010f000)
	libm.so.6 => /lib64/libm.so.6 (0x00007f545fe0d000)
	libpcre.so.0 => /lib64/libpcre.so.0 (0x00007f545fbac000)
	libssl.so.6 => not found
	libcrypto.so.6 => not found
	libdl.so.2 => /lib64/libdl.so.2 (0x00007f545f9a7000)
	libz.so.1 => /lib64/libz.so.1 (0x00007f545f790000)
	libc.so.6 => /lib64/libc.so.6 (0x00007f545f3ce000)
	libfreebl3.so => /lib64/libfreebl3.so (0x00007f545f1cb000)
	libgcc_s.so.1 => /lib64/libgcc_s.so.1 (0x00007f545efb4000)
	/lib64/ld-linux-x86-64.so.2 (0x00007f54605bd000)
	libpthread.so.0 => /lib64/libpthread.so.0 (0x00007f545ed98000)
```

可知，缺少 libssl.so.6 和 libcrypto.so.6 两个 link，首先安装这两个包

```
sudo yum install libssl.so.6
sudo yum install libcrypto.so.6
```

如果已安装，进入到 */lib64* 目录下，建立 link

```
sudo ln -s libssl.so.1.0.1e libssl.so.6
sudo ln -s libcrypto.so.1.0.1e libcrypto.so.6
```

如果提示以下信息

> ./nginx: error while loading shared libraries: libpcre.so.0: cannot open shared object file: No such file or directory


解决方案：

```
sudo link /usr/lib64/libpcre.so.1 /lib64/libpcre.so.0
```

