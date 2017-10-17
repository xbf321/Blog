title: Git 常用命令
date: 2015-06-16
categories: git
tags: [git]
---

总结 git 的一些知识点，防止自己忘记了。

<!--more-->

## 基本操作

* 初始化仓库

```
git init
```

* 克隆仓库

```
git clone 仓库地址
```

* 添加远程仓库地址

```
git remote add [仓库短名称] [仓库Url]
```
<!--more-->

## 分支相关

* 显示分支,加入 `-a` 参数,显示本地及远程分支

```
git branch
git branch -a
```

* 新增分支

```
git branch 分支名
```

* 删除本地分支,`-D` 强制删除本地分支

```
git branch -d 分支名
git branch -D 分支名
```

* 删除远程分支

```
git push origin  :[远程分支]
```

* 切换分支,分支不存在时,会报错,加入`-b`参数(新增并切换)

```
git checkout 分支名
git checkout -b 分支名

```

* 获取远程某个分支,此命令会自动track,push的时候,不必指定远程分支

```
git checkout -b 分支名 origin/分支名
```

## 重置，撤销

* 重置未提交文件

```
git reset --hard HEAD
```

* 重置已提交的文件

```
git revert HEAD
```

* 恢复一个文件

```
git checkout -- hello.rb
```

## 提交操作

* 查看状态

```
git status
```

* 提交,`-a`添加到缓存区

```
git commit -a -m '更改描述'
```

* push

```
git push origin 本地分支:远程分支
```

## 合并

* 合并

```
git merge 分支名
```

## 日志

* 查看当前分支最后提交记录

```
git branch -v
```

## 标签

* 打标签

```
git tag -a 标签名 -m '提交的描述'
```