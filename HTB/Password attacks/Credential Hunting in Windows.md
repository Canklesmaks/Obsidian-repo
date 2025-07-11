once we have access to a target Windows machine through the GUI or CLI, we can significantly benefit from incorporating credential hunting into our approach.

* `Credential Hunting` is the process of performing detailed searches across the file system and through various applications to discover credentials.
	* To understand this concept, let's place ourselves in a scenario. We have gained access to an IT admin's Windows 10 workstation through RDP.
## Search Centric

Many of the tools available to us in Windows have search functionality. In this day and age, there are search-centric features built into most applications and operating systems, so we can use this to our advantage on an engagement

* A user may have documented their passwords somewhere on the system.
* There may even be default credentials that could be found in various files.
	* It would be wise to base our search for credentials on what we know about how the target system is being used. In this case, we know we have access to an IT admin's workstation.

'What might an IT admin be doing on a day-to-day basis & which of those tasks may require credentials?'
* We can use this question & consideration to refine our search to reduce the need for random guessing as much as possible.
#### Key Terms to Search

Whether we end up with access to the GUI or CLI, we know we will have some tools to use for searching but of equal importance is what exactly we are searching for. Here are some helpful key terms we can use that can help us discover some credentials:

|               |              |             |
| ------------- | ------------ | ----------- |
| Passwords     | Passphrases  | Keys        |
| Username      | User account | Creds       |
| Users         | Passkeys     | Passphrases |
| configuration | dbcredential | dbpassword  |
| pwd           | Login        | Credentials |
Let's use some of these key terms to search on the IT admin's workstation.

## Search Tools

With access to the GUI, it is worth attempting to use `Windows Search` to find files on the target using some of the keywords mentioned above.

![[Pasted image 20240626141457.png]]

We can also take advantage of third-party tools like [Lazagne](https://github.com/AlessandroZ/LaZagne) to quickly discover credentials that web browsers or other installed applications may insecurely store.

It would be beneficial to keep a [standalone copy](https://github.com/AlessandroZ/LaZagne/releases/) of Lazagne on our attack host so we can quickly transfer it over to the target.
Once Lazagne.exe is on the target, we can open command prompt or PowerShell, navigate to the directory the file was uploaded to, and execute the following command:

#### Running Lazagne All
```cmd-session
C:\Users\bob\Desktop> start lazagne.exe all
```
This will execute Lazagne and run `all` included modules. We can include the option `-vv` to study what it is doing in the background. Once we hit enter, it will open another prompt and display the results:

```cmd-session
|====================================================================|
|                                                                    |
|                        The LaZagne Project                         |
|                                                                    |
|                          ! BANG BANG !                             |
|                                                                    |
|====================================================================|


########## User: bob ##########

------------------- Winscp passwords -----------------

[+] Password found !!!
URL: 10.129.202.51
Login: admin
Password: SteveisReallyCool123
Port: 22
```
* If we used the `-vv` option, we would see attempts to gather passwords from all Lazagne's supported software.
#### Using findstr

We can also use [findstr](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/findstr) to search from patterns across many types of files. Keeping in mind common key terms, we can use variations of this command to discover credentials on a Windows target:

```cmd-session
C:\> findstr /SIM /C:"password" *.txt *.ini *.cfg *.config *.xml *.git *.ps1 *.yml
```
## Additional Considerations

There are thousands of tools & key terms we could use to hunt for credentials on Windows operating systems. 
*  Know that which ones we choose to use will be primarily based on the function of the computer.
* Always be mindful of how the system is being used, and this will help us know where to look.


Here are some other places we should keep in mind when credential hunting:
- Passwords in Group Policy in the SYSVOL share
- Passwords in scripts in the SYSVOL share
- Password in scripts on IT shares
- Passwords in web.config files on dev machines and IT shares
- unattend.xml
- Passwords in the AD user or computer description fields
- KeePass databases --> pull hash, crack and get loads of access.
- Found on user systems and shares
- Files such as pass.txt, passwords.docx, passwords.xlsx found on user systems, shares, [Sharepoin](https://www.microsoft.com/en-us/microsoft-365/sharepoint/collaboration)