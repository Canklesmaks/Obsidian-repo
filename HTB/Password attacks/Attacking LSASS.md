In addition to getting copies of the SAM database to dump and crack hashes, we will also benefit from targeting LSASS. As discussed in the `Credential Storage` section of this module, LSASS is a critical service that plays a central role in credential management and the authentication processes in all Windows operating systems.
![[Pasted image 20240624125348.png]]

Upon initial logon, LSASS will:

- Cache credentials locally in memory
- Create [access tokens](https://docs.microsoft.com/en-us/windows/win32/secauthz/access-tokens)
- Enforce security policies
- Write to Windows [security log](https://docs.microsoft.com/en-us/windows/win32/eventlog/event-logging-security)

## Dumping LSASS Process Memory

Similar to the process of attacking the SAM database, with LSASS, it would be wise for us first to create a copy of the contents of LSASS process memory via the generation of a memory dump.

* conducting attacks offline gives us more flexibility in the speed of our attack and requires less time spent on the target system.
* There are countless methods we can use to create a memory dump. 
	* *Let's cover techniques that can be performed using tools already built-in to Windows.*
#### Task Manager Method
*  With access to an interactive graphical session with the target, we can use task manager to create a memory dump. This requires us to: `Open Task Manager` > `Select the Processes tab` > `Find & right click the Local Security Authority Process` > `Select Create dump file`
	* A file called `lsass.DMP` is created and saved in: 
	C:\Users\loggedonusersdirectory\AppData\Local\Temp
This is the file we will transfer to our attack host. We can use the file transfer method discussed in the [[Attacking SAM (security accounts manager)]] section of this module to transfer the dump file to our attack host.

#### Rundll32.exe & Comsvcs.dll Method

* We can use an *alternative* method to dump LSASS process memory through a command-line utility called [rundll32.exe](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/rundll32).
* This way is faster than the Task Manager method and more flexible because we may gain a shell session on a Windows host with only access to the command line.
	*  It is important to note that modern anti-virus tools recognize this method as malicious activity.
* Before issuing the command to create the dump file, we must determine what process ID (`PID`) is assigned to `lsass.exe`. This can be done from cmd or PowerShell:
#### Finding LSASS PID in cmd
* From cmd, we can issue the command `tasklist /svc` and find lsass.exe and its process ID in the PID field.
```cmd-session
C:\Windows\system32> tasklist /svc
```

#### Finding LSASS PID in PowerShell
* From PowerShell, we can issue the command `Get-Process lsass` and see the process ID in the `Id` field.
```powershell-session
PS C:\Windows\system32> Get-Process lsass
```
#### Creating lsass.dmp using PowerShell
*  With an **elevated** PowerShell session, we can issue the following command to create the dump file:
```powershell-session
PS C:\Windows\system32> rundll32 C:\windows\system32\comsvcs.dll, MiniDump 672 C:\lsass.dmp full
```
* With this command, we are running `rundll32.exe` to call an exported function of `comsvcs.dll` which also calls the MiniDumpWriteDump (`MiniDump`) function to dump the LSASS process memory to a specified directory (`C:\lsass.dmp`).
	* most modern AV tools recognize this as malicious and prevent the command from executing.
		* In these cases, we will need to consider ways to bypass or disable the AV tool we are facing.

## Using Pypykatz to Extract Credentials
* we can use a powerful tool called [pypykatz](https://github.com/skelsec/pypykatz) to attempt to extract credentials from the .dmp file once on our local machine.
* Pypykatz is an implementation of Mimikatz written entirely in Python. The fact that it is written in Python allows us to run it on Linux-based attack hosts
* At the time of this writing, Mimikatz only runs on Windows systems, so to use it, we would either need to use a Windows attack host or we would need to run Mimikatz directly on the target, which is not an ideal scenario.
* Recall that LSASS stores credentials that have active logon sessions on Windows systems.
	* When we dumped LSASS process memory into the file, we essentially took a "snapshot" of what was in memory at that point in time.
		* If there were any active logon sessions, the credentials used to establish them will be present. Let's run Pypykatz against the dump file and find out

#### Running Pypykatz
* The command initiates the use of `pypykatz` to parse the secrets hidden in the LSASS process memory dump.
* the command because LSASS is a subsystem of `local security authority`, then we specify the data source as a `minidump` file, proceeded by the path to the dump file (`/home/peter/Documents/lsass.dmp`) stored on our attack host.
#### MSV
* [MSV](https://docs.microsoft.com/en-us/windows/win32/secauthn/msv1-0-authentication-package) is an authentication package in Windows that LSA calls on to validate logon attempts against the SAM database.
* Pypykatz extracted the `SID`, `Username`, `Domain`, and even the `NT` & `SHA1` password hashes associated with the bob user account's logon session stored in LSASS process memory.
	* This will prove helpful in the final stage of our attack covered at the end of this section.
#### WDIGEST
* `WDIGEST` is an older authentication protocol enabled by default in `Windows XP` - `Windows 8` and `Windows Server 2003` - `Windows Server 2012`.
* LSASS caches credentials used by WDIGEST in clear-text. This means if we find ourselves targeting a Windows system with WDIGEST enabled, we will most likely see a password in clear-text. 
* Modern Windows operating systems have WDIGEST disabled by default. Additionally, it is essential to note that Microsoft released a security update for systems affected by this issue with WDIGEST. We can study the details of that security update [here](https://msrc-blog.microsoft.com/2014/06/05/an-overview-of-kb2871997/).

#### Kerberos
* [Kerberos](https://web.mit.edu/kerberos/#what_is) is a network authentication protocol used by Active Directory in Windows Domain environments. Domain user accounts are granted tickets upon authentication with Active Directory.
*  This ticket is used to allow the user to access shared resources on the network that they have been granted access to without needing to type their credentials each time.
	* LSASS`caches passwords`, `ekeys`, `tickets`, and `pins` associated with Kerberos. It is possible to extract these from LSASS process memory and use them to access other systems joined to the same domain.
#### DPAPI
* The Data Protection Application Programming Interface or [DPAPI](https://docs.microsoft.com/en-us/dotnet/standard/security/how-to-use-data-protection) is a set of APIs in Windows operating systems used to encrypt and decrypt DPAPI data blobs on a per-user basis for Windows OS features and various third-party applications.
* Here are just a few examples of applications that use DPAPI and what they use it for:

|`Internet Explorer`|Password form auto-completion data (username and password for saved sites).|
|`Google Chrome`|Password form auto-completion data (username and password for saved sites).|
|`Outlook`|Passwords for email accounts.|
|`Remote Desktop Connection`|Saved credentials for connections to remote machines.|
|`Credential Manager`|Saved credentials for accessing shared resources, joining Wireless networks, VPNs and more.|

* Mimikatz and Pypykatz can extract the DPAPI `masterkey` for the logged-on user whose data is present in LSASS process memory.
* This masterkey can then be used to decrypt the secrets associated with each of the applications using DPAPI and result in the capturing of credentials for various accounts.
	* PAPI attack techniques are covered in greater detail in the [Windows Privilege Escalation](https://academy.hackthebox.com/module/details/67) module.
#### Cracking the NT Hash with Hashcat
* Now we can use Hashcat to crack the NT Hash. In this example, we only found one NT hash associated with the Bob user, which means we won't need to create a list of hashes as we did in the `Attacking SAM` section of this module.
* After setting the mode in the command, we can paste the hash, specify a wordlist, and then crack the hash.