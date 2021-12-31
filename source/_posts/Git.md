---
title: Git
abbrlink: 69c3279c
date: 2021-08-18 10:01:21
tags:
---

# Git

## install

```bash
yum install git
git version
```

## Config

**set the name and email**

```bash
git config --global user.name "xxx"
git config --global user.email "xxx@xxx"
git config --list
```

**ssh**

```bash
ssh-keygen -t rsa -C "user.email"
cd ~/.ssh/

# Configure the corresponding SSH in GitHub according to the content in id_rsa.pua

ssh git@github.com
```

## References

- [Linux 下使用 Git 教程(一)](https://blog.csdn.net/HcJsJqJSSM/article/details/82941340)

## References

- [「Githug」Git 游戏通关流程](https://www.jianshu.com/p/482b32716bbe)
