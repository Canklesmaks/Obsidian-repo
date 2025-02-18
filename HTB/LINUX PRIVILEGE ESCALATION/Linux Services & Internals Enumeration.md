- What services and applications are installed?
    
- What services are running?
    
- What sockets are in use?
    
- What users, admins, and groups exist on the system?
    
- Who is current logged in? What users recently logged in?
    
- What password policies, if any, are enforced on the host?
    
- Is the host joined to an Active Directory domain?
    
- What types of interesting information can we find in history, log, and backup files
    
- Which files have been modified recently and how often? Are there any interesting patterns in file modification that could indicate a cron job in use that we may be able to hijack?
    
- Current IP addressing information
    
- Anything interesting in the `/etc/hosts` file?
    
- Are there any interesting network connections to other systems in the internal network or even outside the network?
    
- What tools are installed on the system that we may be able to take advantage of? (Netcat, Perl, Python, Ruby, Nmap, tcpdump, gcc, etc.)
    
- Can we access the `bash_history` file for any users and can we uncover any thing interesting from their recorded command line history such as passwords?
    
- Are any Cron jobs running on the system that we may be able to hijack?
#### Network Interfaces
```shell-session
canklesmaks@htb[/htb]$ ip a
```
#### Hosts
```shell-session
canklesmaks@htb[/htb]$ cat /etc/hosts
```

#### User's Last Login
```shell-session
canklesmaks@htb[/htb]$ lastlog
```
#### Logged In Users
```shell-session
canklesmaks@htb[/htb]$ w
```
#### Command History
```shell-session
canklesmaks@htb[/htb]$ history
```
#### Finding History Files
```shell-session
canklesmaks@htb[/htb]$ find / -type f \( -name *_hist -o -name *_history \) -exec ls -l {} \; 2>/dev/null
```
#### Cron
```shell-session
canklesmaks@htb[/htb]$ ls -la /etc/cron.daily/
```
#### Proc
```shell-session
canklesmaks@htb[/htb]$ find /proc -name cmdline -exec cat {} \; 2>/dev/null | tr " " "\n"
```
#### Installed Packages (older linux)
```shell-session
canklesmaks@htb[/htb]$ apt list --installed | tr "/" " " | cut -d" " -f1,3 | sed 's/[0-9]://g' | tee -a installed_pkgs.list
```
#### Sudo Version
```shell-session
canklesmaks@htb[/htb]$ sudo -V
```

#### Binaries

```shell-session
canklesmaks@htb[/htb]$ ls -l /bin /usr/bin/ /usr/sbin/
```

[GTFObins](https://gtfobins.github.io) provides an excellent platform that includes a list of binaries that can potentially be exploited to escalate our privileges on the target system.

#### GTFObins onliner for investigation
```shell-session
canklesmaks@htb[/htb]$ for i in $(curl -s https://gtfobins.github.io/ | html2text | cut -d" " -f1 | sed '/^[[:space:]]*$/d');do if grep -q "$i" installed_pkgs.list;then echo "Check GTFO for: $i";fi;done
```
#### Trace System Calls
```shell-session
canklesmaks@htb[/htb]$ strace ping -c1 10.129.112.20
```
#### Configuration Files
```shell-session
canklesmaks@htb[/htb]$ find / -type f \( -name *.conf -o -name *.config \) -exec ls -l {} \; 2>/dev/null
```
#### Scripts
```shell-session
canklesmaks@htb[/htb]$ find / -type f -name "*.sh" 2>/dev/null | grep -v "src\|snap\|share"
```
#### Running Services by User

```shell-session
canklesmaks@htb[/htb]$ ps aux | grep root
```