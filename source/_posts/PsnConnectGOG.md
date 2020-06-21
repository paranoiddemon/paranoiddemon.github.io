---
title: GOG连接PSN"Playstation connection timed out"问题解决
date: 2020-06-17 17:24:46
tags: 
category: 其他
description: 领取PC版巫师3
---
https://www.gog.com/forum/general_beta_gog_galaxy_2.0/playstation_connection_timed_out

- turn off 2-step verification first

- log in to PSN (store.playstation.com) through your browser

- after the login open this link: https://ca.account.sony.com/api/v1/ssocookie
Copy content of "npsso"

- open windows explorer and enter the following into the address bar at the top and press enter
%LocalAppData%\GOG.com\Galaxy\plugins\installed\

- open the psn_{random number} folder and edit the plugin.py file with your favourite text editor

- go to line 64 and change it from:
stored_npsso = stored_credentials.get("npsso") if stored_credentials else None
to:
stored_npsso = "Copied value of npsso"
("copied value of npsso" should be a 64 character long text consisting of numbers, lower case letters and upper case letters)
(the quotation marks " " are important!)

- In addition to totally REPLACING the line 64, delete lines 65 and 66
- Python is indent-sensitive, it means the script interpreter uses the vertical alignment to know if blocks of code are within certain statements/conditionals/loops or others. To make sure that the script is correctly interpreted you need to make sure that the line 64 you have just replaced starts with the "s" of stored_npsso right under the "c" of async which is in line 63. Actually it, the line 64 has to start after 8 blank SPACES (do not use tabs).

- restart GOG Galaxy
- try connecting with PSN again 