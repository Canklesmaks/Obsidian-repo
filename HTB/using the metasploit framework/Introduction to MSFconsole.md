we can use the `-q` option, which does not display the banner.
```shell-session
canklesmaks@htb[/htb]$ msfconsole -q

msf6 > 
```

the `apt` package manager can currently handle the update of modules and features effortlessly.

#### Installing MSF
```shell-session
canklesmaks@htb[/htb]$ sudo apt update && sudo apt install metasploit-framework

<SNIP>

(Reading database ... 414458 files and directories currently installed.)
Preparing to unpack .../metasploit-framework_6.0.2-0parrot1_amd64.deb ...
Unpacking metasploit-framework (6.0.2-0parrot1) over (5.0.88-0kali1) ...
Setting up metasploit-framework (6.0.2-0parrot1) ...
Processing triggers for man-db (2.9.1-1) ...
Scanning application launchers
Removing duplicate launchers from Debian
Launchers are updated
```
## MSF Engagement Structure

The MSF engagement structure can be divided into five main categories.

- Enumeration
- Preparation
- Exploitation
- Privilege Escalation
- Post-Exploitation 
![[Pasted image 20240325120929.png]]