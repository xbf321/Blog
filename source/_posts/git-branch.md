title: 在Mac、Linux 终端显示 Git 当前所在分支
date: 2015-06-16
categories: git
tags: [git]
---

小技巧，不过如果安装 zsh ，可能就不要这个过程了。

<!--more-->

## 进入你的`home`目录

```
cd ~
```

## 编辑`.bashrc`文件

```
vi .bashrc
```

## 将下面代码加入到文件的最后处

```
function git_branch {
  branch="`git branch 2>/dev/null | grep "^\*" | sed -e "s/^\*\ //"`"
  if [ "${branch}" != "" ];then
      if [ "${branch}" = "(no branch)" ];then
          branch="(`git rev-parse --short HEAD`...)"
      fi
      echo " ($branch)"
  fi
}

export PS1='\u@\h \[\033[01;36m\]\W\[\033[01;32m\]$(git_branch)\[\033[00m\] \$ '
```

## 保存退出

## 执行加载命令

```
source ./.bashrc
```

## 完成

> Mac 下面启动的 shell 是 login shell，所以加载的配置文件是.bash_profile，不会加载.bashrc。如果你是 Mac 用户的话，需要再执行下面的命令，这样每次开机后才会自动生效：

```
echo "[ -r ~/.bashrc ] && source ~/.bashrc" >> .bash_profile
```