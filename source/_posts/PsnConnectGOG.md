---
title: GOG连接PSN超时解决方法
date: 2020-06-17 17:24:46
tags: 
urlname: psn-gog-connection
category: 其他
description: Playstation connection timed out问题解决，领取PC版巫师3
---
**关闭两步验证，获取 "sso cookie"**

turn off 2-step verification first
log in to PSN (store.playstation.com) through your browser
after the login open this link: https://ca.account.sony.com/api/v1/ssocookie
Copy content of "npsso"

**修改代码，本地验证** 
open windows explorer and enter the following into the address bar at the top and press enter

```text
%LocalAppData%\GOG.com\Galaxy\plugins\installed\
```
open the psn_{random number} folder and edit the plugin.py file with your favourite text editor
go to line 64 and change it from:
```python
stored_npsso = stored_credentials.get("npsso") if stored_credentials else None
```
to:
```python
stored_npsso = "Copied value of npsso"
```
("copied value of npsso" should be a 64 character long text consisting of numbers, lower case letters and upper case letters)   (the quotation marks " " are important!)
In addition to totally REPLACING the line 64, delete lines 65 and 66
the line 64 has to start after 8 blank SPACES (do not use tabs)

**重启gog再连接**
restart GOG Galaxy
try connecting with PSN again 