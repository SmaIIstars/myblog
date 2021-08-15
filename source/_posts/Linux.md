---
title: Linux
abbrlink: 53d0684b
date: 2021-08-15 19:24:20
tags: Linux
---

# Linux

**Here, I will slowly update some knowledge points about Linux**

## Git

### install

```bash
yum install git
git version
```

### Config

**username and email**

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

### References

- [Linux 下使用 Git 教程(一)](https://blog.csdn.net/HcJsJqJSSM/article/details/82941340)



## Nohup(no hang up)

**It's used to run commands in the background of system hang up. Exiting the terminal will not affect the running of the program**

```bash
# nohup Command [ Arg … ] [　& ]
# Command：the command will be executed
# Arg：some parameters, we can specify the output file
# &：hang the program in the background

# running the python program
nohup python bot.py &

# specify the output file to runoob.log
nohup python bot.py > runoob.log &

# 0 – stdin (standard input)
# 1 – stdout (standard output)
# 2 – stderr (standard error)
# 2>&1: Redirect the standard error 2 to standard output &1, and &1 is redirected to the runoob.log file
nohup python bot.py > runoob.log 2>&1 &
```

### References

- [Linux nohup](https://www.runoob.com/linux/linux-comm-nohup.html)
