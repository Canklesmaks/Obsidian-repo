IN LINUX passwords are also stored encrypted in a file.

This file is called the `shadow` file and is located in `/etc/shadow` and is part of the Linux user management system. In addition, these passwords are commonly stored in the form of `hashes`. An example can look like this:

The other two files are `/etc/passwd` and `/etc/group`
#### Shadow File
```shell-session
root@htb:~# cat /etc/shadow

...SNIP...
htb-student:$y$j9T$3QSBB6CbHEu...SNIP...f8Ms:18955:0:99999:7:::
```

The `/etc/shadow` file has a unique format in which the entries are entered and saved when new users are created.

||||||||||
|---|---|---|---|---|---|---|---|---|
|htb-student:|$y$j9T$3QSBB6CbHEu...SNIP...f8Ms:|18955:|0:|99999:|7:|:|:|:|
|`<username>`:|`<encrypted password>`:|`<day of last change>`:|`<min age>`:|`<max age>`:|`<warning period>`:|`<inactivity period>`:|`<expiration date>`:|`<reserved field>`|

## Windows Authentication Process
* The [Windows client authentication process](https://docs.microsoft.com/en-us/windows-server/security/windows-authentication/credentials-processes-in-windows-authentication) can oftentimes be more complicated than with Linux systems and consists of many different modules that perform the entire logon, retrieval, and verification processes. In addition, there are many different and complex authentication procedures on the Windows system, such as Kerberos authentication.
* The [Local Security Authority](https://learn.microsoft.com/en-us/windows-server/security/credentials-protection-and-management/configuring-additional-lsa-protection) (`LSA`) is a protected subsystem that authenticates users and logs them into the local computer. In addition, the LSA maintains information about all aspects of local security on a computer. It also provides various services for translating between names and security IDs (`SIDs`).
 ![[Pasted image 20240416133527.png]]

Local interactive logon is performed by the interaction between the logon process ([WinLogon](https://www.microsoftpressstore.com/articles/article.aspx?p=2228450&seqNum=8)), the logon user interface process (`LogonUI`), the `credential providers`, `LSASS`, one or more `authentication packages`, and `SAM` or `Active Directory`.

Credential Manager is a feature built-in to all Windows operating systems that allows users to save the credentials they use to access various network resources and websites. Saved credentials are stored based on user profiles in each user's `Credential Locker`. Credentials are encrypted and stored at the following location:

  Credential Storage

```powershell-session
PS C:\Users\[Username]\AppData\Local\Microsoft\[Vault/Credentials]\
```