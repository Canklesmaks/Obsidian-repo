## Enumeration
* Enumeration is the key to privilege escalation.
* always know
	* `OS Version
	* `Kernel Version`
	* Running Services
	* Installed Packages and Versions
	* Logged in Users
	* User Home Directories
	* #### SSH Directory Contents
	* #### Bash History
	* Cron Jobs
	* Unmounted File Systems and Additional Drives
	* SETUID and SETGID Permissions
	* Writeable Directories
	* `Writeable Files

#### Find Writable Directories

  Introduction to Linux Privilege Escalation

```shell-session
canklesmaks@htb[/htb]$ find / -path /proc -prune -o -type d -perm -o+w 2>/dev/null

/dmz-backups
/tmp
/tmp/VMwareDnD
/tmp/.XIM-unix
/tmp/.Test-unix
/tmp/.X11-unix
/tmp/systemd-private-8a2c51fcbad240d09578916b47b0bb17-systemd-timesyncd.service-TIecv0/tmp
/tmp/.font-unix
/tmp/.ICE-unix
/proc
/dev/mqueue
/dev/shm
/var/tmp
/var/tmp/systemd-private-8a2c51fcbad240d09578916b47b0bb17-systemd-timesyncd.service-hm6Qdl/tmp
/var/crash
/run/lock
```

#### Find Writable Files

  Introduction to Linux Privilege Escalation

```shell-session
canklesmaks@htb[/htb]$ find / -path /proc -prune -o -type f -perm -o+w 2>/dev/null

/etc/cron.daily/backup
/dmz-backups/backup.sh
/proc
/sys/fs/cgroup/memory/init.scope/cgroup.event_control

<SNIP>

/home/backupsvc/backup.sh

<SNIP>
```
