Many services may be found, which have flaws that can be leveraged to escalate privileges. An example is the popular terminal multiplexer [Screen](https://linux.die.net/man/1/screen). Version 4.5.0 suffers from a privilege escalation vulnerability due to a lack of a permissions check when opening a log file.

#### Screen Version Identification

Vulnerable Services

```shell-session
canklesmaks@htb[/htb]$ screen -v

Screen version 4.05.00 (GNU) 10-Dec-16
```