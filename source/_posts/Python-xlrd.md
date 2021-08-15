---
title: XLRD and XLWT in Python
tags: Python
abbrlink: 14aa609c
date: 2021-02-19 19:19:08
---

# 'xlrd' and 'xlwt' in Python

**'_xlrd_' and '_xlwt_' are two Python tripartite library for working with Excel files**.

## xlrd

Responsible for reading files.

- Open up the Excel files

  ```python
  workbook = xlrd.open_workbook(path)
  ```

- Get all sheets of file

  ```python
  workbook.sheet_names()
  ```

- Get the sheet and content

  ```python
  # Get sheet by index
  sheet2 = workbook.sheet_by_index(1)
  
  # Get sheet by name
  sheet2 = workbook.sheet_by_name('sheet2')
  
  # Get content
  sheet2.name
  sheet2.nrows
sheet2.ncols
  
  sheet2.row_values(index)
  sheet2.col_values(index)
  ```
  
  



## xlwt

Responsible for write files.

## Issue

- Excel xlsx file not supported

  Change the version of 'xlrd', it has been updated to 2.0.1 and only supports _.xls_ files, not _.xlsx_

  ```cmd
  pip install xlrd==1.2.0
  ```

- If file path has Chinese, 'open' operation needs to be transcoded.
  1. The prefix 'r' to path indicates the native string.
  2. filename = filename.decode('utf-8')

## References

[python里面的xlrd模块详解（一）](https://www.cnblogs.com/insane-Mr-Li/p/9092619.html)

[Python中xlrd和xlwt模块使用方法](https://www.cnblogs.com/linyfeng/p/7123423.html)

