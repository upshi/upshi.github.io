---
title: Docker镜像 CentOS 7 安装中文字符集
date: 2018-01-10 22:21:06
tags: Docker
category: 运维
---

# Docker镜像 CentOS 7安装中文字符集

使用Docker在创建CentOS 7的容器时，发现默认的语言不是英文，通过执行`locale`命令可以看到
```
[root@92e9997ed8fb /]# locale
LANG=
LC_CTYPE="POSIX"
LC_NUMERIC="POSIX"
LC_TIME="POSIX"
LC_COLLATE="POSIX"
LC_MONETARY="POSIX"
LC_MESSAGES="POSIX"
LC_PAPER="POSIX"
LC_NAME="POSIX"
LC_ADDRESS="POSIX"
LC_TELEPHONE="POSIX"
LC_MEASUREMENT="POSIX"
LC_IDENTIFICATION="POSIX"
LC_ALL=
```

查看所有的语言包
```
[root@92e9997ed8fb /]# locale -a
C
POSIX
en_US.utf8
```

发现并没有中文，所以要安装一下中文语言包
```
yum -y install kde-l10n-Chinese
yum -y reinstall glibc-common
localedef -c -f UTF-8 -i zh_CN zh_CN.utf8
```

再次执行`locale -a`可以看到`zh_CN.utf-8`


网上搜索，修改`/etc/locale.conf`文件，就可以了
```
LANG="zh_CN.UTF-8"
LC_ALL="zh_CN.UTF-8"
```
但是再次执行`locale`命令，不知道为什么还是没有生效

于是通过修改`/etc/profile`的方式，export环境变量的来实现
```
export LC_ALL=zh_CN.UTF-8
```

