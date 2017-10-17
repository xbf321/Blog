title: 学习树莓派（Pi 3 Model B）
date: 2017-06-1
categories: tech
tags: [raspberry, mac]
---

介绍如何安装系统，设置Wifi 等等。

<!--more-->

## 把系统复制到 TF 卡中

插入SD卡，使用 *df* 查看当前已经挂载的卷

```
$ df
Filesystem    512-blocks      Used Available Capacity  iused   ifree %iused  Mounted on
/dev/disk1     233269248 218788512  13968736    94% 27412562 1746092   94%   /
devfs                374       374         0   100%      648       0  100%   /dev
map -hosts             0         0         0   100%        0       0  100%   /net
map auto_home          0         0         0   100%        0       0  100%   /home
/dev/disk2s1    31100416      4992  31095424     1%        0       0  100%   /Volumes/Pi
```

因为已经命名了SD卡为 Pi ，所以SD卡的分区对应的设备文件为：/dev/disk2s1

使用diskutil unmount卸载

```
$ diskutil unmount /dev/disk2s1
Volume Pi on disk2s1 unmounted
```

diskutil list 确认设备 我买的是16G的卡

```
$ diskutil list
/dev/disk2
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *15.9 GB    disk2
   1:                 DOS_FAT_32 Pi                      15.9 GB    disk2s1
```

使用dd命令将系统镜像写入
PS /dev/disk2s1是分区，/dev/disk2是块设备，/dev/rdisk2是原始字符设备

```
$ dd bs=4m if=pi.img of=/dev/rdisk2
781+1 records in
781+1 records out
3276800000 bytes transferred in 194.134151 secs (16879050 bytes/sec)
```

至此，SD卡上已经刷入了 Raspbian 系统

再用diskutil unmountDisk卸载设备

```
$ diskutil unmountDisk /dev/disk2
Unmount of all volumes on disk2 was successful
```

## 远程 ss

默认系统是关闭 ssh ，系统烧录到 **SD** 卡后，系统会提示 **拒绝链接**，解决办法是，在根目录下新建一个空的 `ssh` 文件，即可。

```
touch ssh
```

## 修改国内源

修改 sources.list

```
sudo vim /etc/apt/sources.list
```

把原来的注释，或者备份下源文件都可以，我注释掉了。填入国内源：

```
deb http://mirrors.neusoft.edu.cn/raspbian/raspbian jessie main contrib non-free rpi
```

修改 raspi.list

```
sudo vim /etc/apt/sources.list.d/raspi.list
```

填入：

```
deb http://mirrors.ustc.edu.cn/archive.raspberrypi.org/debian/ jessie main ui
```

## 更新软件源和软件

```
sudo apt-get update
sudo apt-get upgrade
```

## 启用 root 账号

root 默认没有密码，但是用户锁定了。我们需要重新开启 root 账号

1. root 账号设置密码，会提示输入两次
```
sudo passwd root
```

2. 启用 root 账号

```
sudo passwd --unlock root
```

提示：passwd: password expiry information changed. 说明还需要修改配置文件。

3. 更改配置文件

```
sudo vi /etc/ssh/sshd_config
```

找到： PermitRootLogin without-passwor 更改为：PermitRootLogin yes

4. reboot 重启系统

```
sudo reboot
```

## 设置 WiFi


```
# 搜索 无线 网络信号，找到 ESSID
iwlist scan

# 编辑配置文件
sudo vim /etc/wpa_supplicant/wpa_supplicant.conf    
# 在该文件最后添加下面的话
network={
  ssid="WIFINAME"
  psk="password"
}

# 引号部分分别为wifi的名字和密码
# 保存文件后几秒钟应该就会自动连接到该wifi
# 查看是否连接成功
ifconfig wlan0

# 停用 wifi
ifdown wlan0
# 启用 wifi
ifup wlan0
```