---
title: MySQL
tags:
  - MySQL
  - Navicat
abbrlink: c24675b4
date: 2020-12-18 15:27:44
top:
---

# MySQL

## Install

[reference](https://blog.csdn.net/weixin_38239039/article/details/79629984)

## Issue

### Access denied for user 'root'@'localhost' (using password: YES)

[reference](https://blog.csdn.net/zoucui/article/details/96996554)

1. Add 'skip-grant-tables' to 'my.ini' file in MySQL's directory

   ![](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/MySQL/Addskip-grant-tables.png)

2. Reload the MySQL's service

   ```js
   net stop mysql
   net start mysql
   ```

3. Login MySQL without password and set the new password

   ```mysql
   mysql -u root -p
   // Don't need to enter the password
   ```

4. Change the password (In the higher version, the password filed is changed to authentication_string)

   ```mysql
   # lower version
   update mysql.user set password='123' where user='root';
   -- higher version
   update mysql.user set authentication_string='123' where user='root';
   /*password -> authentication_string*/
   ```

5. Refresh the database

   ```mysql
   flush privileges;
   ```

6. Delete the 'skip-grant-tables'

7. Reload the MySQL's service

### Server returns invalid timezone. Go to 'Advanced' tab and set 'serverTimezone' prope

[reference](https://blog.csdn.net/liuqiker/article/details/102455077)

![](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/MySQL/SetGlobalTimeZone.png)

# Navicat

## Install

[official website](http://www.navicat.com.cn/navicat-15-highlights/)

## Crack

![NavicatActive1](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/MySQL/NavicatActive1.png)

![NavicatActive2](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/MySQL/NavicatActive2.png)

## Issue

### rsa public key not find

copy the keygen to the installation directory and run as an administrator

### No All Pattern Found！File Already Patched?

1. Click the installation file to re install Navicat
2. First run the keygen, and then open Navicat

### Navicat program flutters back or no respond

Close the netease youdao dictionary
