---
title: Set maximum number of connections in MySQL
tags: MySQL
abbrlink: 9d45af52
date: 2021-02-18 23:07:51
---

# Set maximum number of connections in MySQL

**MySQL sets maximum number of connections during installation, but sometimes we need to modify the value to meet our need.**

## Temporary measure

Restart the original settings after restarting MySQL.

```cmd
# Enter in the context of MySQL connection
set GLOBAL max_connections=max_value;
```

## Permanent measure

Modify the configuration of MySQL.

1. Find the '*my.ini'*' file in MySQL directory.

2. Modify the value of '*max_connections*'.

3. Restart the MySQL.

## References

[mysql最大连接数设置技巧总结](https://www.jb51.net/article/157967.htm)