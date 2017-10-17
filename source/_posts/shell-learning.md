title: Shell 学习
date: 2017-06-9
categories: tech
tags: [shell]
---

记录学习 shell 的知识，后续会不定期更新。

<!--more-->

## 变量和环境变量

* 不用声明，直接赋值使用
* 所有值都以字符串形式存储
* 格式 `name=value` ，不能写成 `name = value`，前者是赋值语句，后者是相等操作 
* 环境变量, HOME，PWD，USER，UID(root UID是0)，SHELL等
* 设置环境变量
    ```
    export PATH = "$PATH;/home/user/bin"
    ```

## 命令技巧
* pgrep 命令，获得进程ID
    ```
    pgrep xxx
    ```
* 获取字符串长度

    ```
    var=12345678
    echo ${#var}
    ```