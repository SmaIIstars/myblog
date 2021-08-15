---
title: 'Four body format for POST (form-data, x-www-form-urlencoded, raw, binary)'
tags: POST
abbrlink: 9cc151b5
date: 2021-01-25 16:30:21
---

# Four body format for POST



## format-data

*multipart/form-data*, it can upload multiple key-values and files. 

![multipart/form-data](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/POSTBody-form-data.png)

```javascript
// axios simple sample
let formData = new FormData();
formData.append("username", "username");
formData.append("password", "password");

axios({
  method: "post",
  url: "xxx",
  data: formData,
  headers: {
    "Content-Type": "multipart/form-data",
  },
})
```



## x-www-form-urlencoded

*application/x-www-from-urlencoded*, it converts data to key-values and cannot upload files.

![application/x-www-from-urlencoded](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/POSTBody-x-www-form-urlencoded.png)



## raw

It can upload text in any format. We always upload JSON data in this way.

|      content-type      |    Note    |
| :--------------------: | :--------: |
|       text/plain       |    Text    |
| application/javascript | JavaScript |
|    application/json    |    JSON    |
|       text/html        |    HTML    |
|    application/xml     |    XML     |

![raw](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/POSTBody-raw.png)



## binary

*application/octet-stream*, only binary data can be upload, usually used to upload files. Because it is not key-value format, only one data can be passed at once.

![binary](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/POSTBody-binary.png)