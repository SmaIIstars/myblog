---
title: Yagmail
tags:
  - yagmail
  - E-mail
  - Python
abbrlink: b3f977ec
date: 2021-01-24 00:00:00
---

# Yagmail

**It is Python's tripartite library for sending E-mail. For example, email validation, subscription notifications, etc.**

## Install

```pytho
pip install yagmail
```

## Usage

```python
# Register
import yagmail
yagmail.register('from_email', 'from_mail_password')

# Start a connection
yag = yagmail.SMTP('SMTP_services')
```

|    Service Name    |        Host        |
| :----------------: | :----------------: |
|         QQ         |    smtp.qq.com     |
| Tencent Enterprise | smtp.exmail.qq.com |
|        163         |    smtp.163.com    |
|        126         |    smtp.126.com    |
|        Sohu        |   smtp.sohu.com    |
|        Sina        |  smtp.sina.com.cn  |

```python
# Variables
to = 'to_email'
to2 = 'to2_email'
to3 = 'to3_email'
subject = 'This is obviously the subject'
body = 'This is obviously the body'
html = '<a href="https://pypi.python.org/pypi/sky/">Click me!</a>'
img = '/local/file/bunny.png'

# Sending E-mail
yag.send(to = to, subject = subject, contents = body)
yag.send(to = to, subject = subject, contents = [body, html, img])
yag.send(contents = [body, img])

# Sending E-mails to multiple objects
yag.send(to = to)
yag.send(to = [to, to2]) # List or tuples for emailadresses without aliases
yag.send(to = {to : 'Alias1'}) # Dictionary for emailaddress with aliases
yag.send(to = {to : 'Alias1', to2 : 'Alias2'}

# With files
attachments = [path, path2]
yag.send(to, subject, contents, attachments)
```

## Simple Sample

```python
# Register the From Email
from_email = {
    "user": 'From Email',
    "password": 'Password or Authorization Code',
    "host": 'SMTP service'
}


yag = yagmail.SMTP(
  user=from_email['user'],
  password=from_email['password'],
  host=from_email['host']
)

# Generate Email Content
# contents = [body, html, img]
subject = ['CDUT STA 验证码']
        contents = generate_contents(username, captcha)

def generate_contents(username, code):
    return ['''
<table width="600" cellspacing="0" border="0" align="center" style="border: rgba(0, 0, 0, 0.3) 1px solid">
  <tbody style="align-items: center">
    <tr style=" height: 64px; background-color: #415a94; color: #fff;">
      <td style="text-align: center; font-size: 21px;">CDUT STA</td>
    </tr>

    <tr>
      <td style=" display: table-cell; padding: 8% 0; color: #000; text-align: center; font-size: 21px; ">
        邮箱验证码
      </td>
    </tr>

    <tr>
      <td style="display: table-cell; padding: 0 6%; color: #333">
        尊敬的 {} , 您好！
      </td>
    </tr>

    <tr>
      <td style="display: table-cell; padding: 2% 6% 10% 6%; color: #333">
          您的验证码是: <span style="font-weight: 600; color: red">{}</span> ,请在 5 分钟内进行验证。如果该验证码不为您本人申请,请无视。
      </td>
    </tr>

    <tr>
      <td style="background: #f7f7f7; display: table-cell; padding: 2% 6%">
        <a href="https://www.baidu.com" style="color: #929292">返回</a>
      </td>
    </tr>
  </tbody>
</table>'''.format(username, code).replace('\n', '')]

# Send Email
# to = String || List || Object ('to1' || [to1, to2] || {to1: 'to1', to2: "to2"})
# contents = htmlTemplate || String
# yag.send(to, subject, contents = [body, html, img])
yag.send(to_email, subject, contents)
```

## Issues

### E-mail Template Limitation

1. Only the `<Table>` tag can be used.
2. Only the inline styles can be used.
3. Float positioning can not be used.
4. Style are best written in the corresponding tag, inheritance may be invalid.
5. Returning newline characters in the E-mail template may result in a lot of `<br>` tags

## References

[Official Website](https://github.com/kootenpv/yagmail)

[HTML Email Style](https://www.cnblogs.com/zhangwenjiajessy/p/6132201.html)

[Email Validation](https://www.cnblogs.com/jonyam/p/python-sand-email.html)

[STA Project](https://github.com/SmaIIstars/STABackend)
