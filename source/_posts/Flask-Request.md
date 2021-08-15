---
title: Flask-Request
tags: Flask
abbrlink: 66635f9d
date: 2021-01-20 14:26:44
---

# Request

**Data interaction object in Falsk framework**



## Attribute

|   Attribute    |                         Instruction                          |                            Note                            |
| :------------: | :----------------------------------------------------------: | :--------------------------------------------------------: |
|      form      | MultiDict (one-key multi-value dictionary) resolved from 'POST' or 'PUT' requests |   noly x-www-form-urlencoded, form-data can be obtained    |
|      args      |            MultiDict resolved from URL query part            |                                                            |
|   **values**   |                         form & args                          |                                                            |
|    cookies     |                                                              |                                                            |
|  **headers**   |                     Headers information                      |                                                            |
|      data      |                                                              |                                                            |
|     files      |                   Files ImmutableMultiDict                   |                                                            |
|   **method**   |                       type of request                        |                                                            |
|      url       |                    base_url + query part                     |   http://127.0.0.1:5000/data/register?token=token&id=id    |
|    base_url    |                       url_root + path                        |            http://127.0.0.1:5000/data/register             |
|    url_root    |                                                              |                   http://127.0.0.1:5000/                   |
|      path      |                                                              |                       /data/register                       |
|   blueprint    |                      Name of blueprint                       |                                                            |
|    **json**    |               isJson ? json_data(dict) : None                |                                                            |
| **get_data()** |                      get bytes of data                       | get_data(as_text=True) to str, then use json.loads to Json |



## References

[Flask-Request Reference](https://blog.csdn.net/u011146423/article/details/88191225)