* We will often use this kind of shell as we come across vulnerable systems because it is likely that an admin will overlook outbound connections, giving us a better chance of going undetected.
```shell-session
canklesmaks@htb[/htb]$ sudo nc -lvnp 443
Listening on 0.0.0.0 443
```
We may want to use common ports like this because when we initiate the connection to our listener, we want to ensure it does not get blocked going outbound through the OS firewall and at the network level.