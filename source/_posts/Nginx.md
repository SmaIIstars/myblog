---
title: Nginx
tags: Nginx
abbrlink: 65b69107
date: 2020-12-17 10:07:54
---

# Nginx

## Install

[Download Nginx](http://nginx.org/en/download.html)

## Start Nginx

1. Double-click to start nginx.exe, and the command box flashes past
2. Enter the command nginx.exe or start nginx

## Reload Nginx

```nginx
nginx -s reload
```

## Stop Nginx

```nginx
# Out of service
nginx -s quit

# Forced shutdown of service
nginx -s stop
```

## Domain name binding

```nginx
server {
  server_name www.itblood.com; #Domain name
}
```

## Forwarding service

```nginx
location / {
  proxy_pass  http://127.0.0.1:8080; # Forwarding address
  proxy_set_header Host $proxy_host;
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
```

## Setup Nginx starts from startup

Convert the Nginx to the system service

1. [Download the 'Windows Service Wrapper'](https://github.com/kohsuke/winsw)

2. Place the program in the directory of Nginx and rename 'nginx-service.exe'.

3. Create configuration file 'nginx-service.xml'

   ```xml
   <service>
   	<id>nginx</id>
   	<name>nginx</name>
   	<description>nginx</description>
   	<logpath>D:\xxx\nginx-x.x.x</logpath>
   	<logmode>roll</logmode>
   	<depend></depend>
   	<executable>D:\xxx\nginx-x.x.x\nginx.exe</executable>
   	<stopexecutable>D:\xxx\nginx-x.x.x\nginx.exe -s stop</stopexecutable>
   </service>
   ```

4. Execute 'nginx-service.exe install' in the CMD in the curent directory

   ![Structure Tree](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/Nginx/StructureTree.png)

5. The Service item is set to start

   ![](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/Nginx/ServiceManagement.png)
