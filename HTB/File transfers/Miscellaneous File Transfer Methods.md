## Netcat

[Netcat](https://sectools.org/tool/netcat/) (often abbreviated to `nc`) is a computer networking utility for reading from and writing to network connections using TCP or UDP, which means that we can use it for file transfer operations.
**Note:** **Ncat** is used in HackTheBox's PwnBox as nc, ncat, and netcat.

## File Transfer with Netcat and Ncat
The target or attacking machine can be used to initiate the connection, which is helpful if a firewall prevents access to the target. Let's create an example and transfer a tool to our target.

In this example, we'll transfer [SharpKatz.exe](https://github.com/Flangvik/SharpCollection/raw/master/NetFramework_4.7_x64/SharpKatz.exe) from our Pwnbox onto the compromised machine. We'll do it using two methods. Let's work through the first one.

We'll first start Netcat (`nc`) on the compromised machine, listening with option `-l`, selecting the port to listen with the option `-p 8000`, and redirect the [stdout](https://en.wikipedia.org/wiki/Standard_streams#Standard_input_(stdin)) using a single greater-than `>` followed by the filename, `SharpKatz.exe`.

#### NetCat - Compromised Machine - Listening on Port 8000

  Miscellaneous File Transfer Methods

```shell-session
victim@target:~$ # Example using Original Netcat
victim@target:~$ nc -l -p 8000 > SharpKatz.exe
```
If the compromised machine is using Ncat, we'll need to specify `--recv-only` to close the connection once the file transfer is finished.

#### Ncat - Compromised Machine - Listening on Port 8000

  Miscellaneous File Transfer Methods

```shell-session
victim@target:~$ # Example using Ncat
victim@target:~$ ncat -l -p 8000 --recv-only > SharpKatz.exe
```
## PowerShell Session File Transfer

We already talk about doing file transfers with PowerShell, but there may be scenarios where HTTP, HTTPS, or SMB are unavailable. If that's the case, we can use [PowerShell Remoting](https://docs.microsoft.com/en-us/powershell/scripting/learn/remoting/running-remote-commands?view=powershell-7.2), aka WinRM, to perform file transfer operations.

[PowerShell Remoting](https://docs.microsoft.com/en-us/powershell/scripting/learn/remoting/running-remote-commands?view=powershell-7.2) allows us to execute scripts or commands on a remote computer using PowerShell sessions. Administrators commonly use PowerShell Remoting to manage remote computers in a network, and we can also use it for file transfer operations. By default, enabling PowerShell remoting creates both an HTTP and an HTTPS listener. The listeners run on default ports TCP/5985 for HTTP and TCP/5986 for HTTPS.

To create a PowerShell Remoting session on a remote computer, we will need administrative access, be a member of the `Remote Management Users` group, or have explicit permissions for PowerShell Remoting in the session configuration. Let's create an example and transfer a file from `DC01` to `DATABASE01` and vice versa.

We have a session as `Administrator` in `DC01`, the user has administrative rights on `DATABASE01`, and PowerShell Remoting is enabled. Let's use Test-NetConnection to confirm we can connect to WinRM.
#### From DC01 - Confirm WinRM port TCP 5985 is Open on DATABASE01.
```powershell-session
PS C:\htb> whoami

htb\administrator

PS C:\htb> hostname

DC01
```

```powershell-session
PS C:\htb> Test-NetConnection -ComputerName DATABASE01 -Port 5985

ComputerName     : DATABASE01
RemoteAddress    : 192.168.1.101
RemotePort       : 5985
InterfaceAlias   : Ethernet0
SourceAddress    : 192.168.1.100
TcpTestSucceeded : True
```

Because this session already has privileges over `DATABASE01`, we don't need to specify credentials. In the example below, a session is created to the remote computer named `DATABASE01` and stores the results in the variable named `$Session`.

#### Create a PowerShell Remoting Session to DATABASE01

  Miscellaneous File Transfer Methods

```powershell-session
PS C:\htb> $Session = New-PSSession -ComputerName DATABASE01
```

We can use the `Copy-Item` cmdlet to copy a file from our local machine `DC01` to the `DATABASE01` session we have `$Session` or vice versa.

#### Copy samplefile.txt from our Localhost to the DATABASE01 Session

  Miscellaneous File Transfer Methods

```powershell-session
PS C:\htb> Copy-Item -Path C:\samplefile.txt -ToSession $Session -Destination C:\Users\Administrator\Desktop\
```

#### Copy DATABASE.txt from DATABASE01 Session to our Localhost

  Miscellaneous File Transfer Methods

```powershell-session
PS C:\htb> Copy-Item -Path "C:\Users\Administrator\Desktop\DATABASE.txt" -Destination C:\ -FromSession $Session
```
## RDP

RDP (Remote Desktop Protocol) is commonly used in Windows networks for remote access. We can transfer files using RDP by copying and pasting. We can right-click and copy a file from the Windows machine we connect to and paste it into the RDP session.

If we are connected from Linux, we can use `xfreerdp` or `rdesktop`. At the time of writing, `xfreerdp` and `rdesktop` allow copy from our target machine to the RDP session, but there may be scenarios where this may not work as expected.
#### Mounting a Linux Folder Using rdesktop


```shell-session
canklesmaks@htb[/htb]$ rdesktop 10.10.10.132 -d HTB -u administrator -p 'Password0@' -r disk:linux='/home/user/rdesktop/files'
```

#### Mounting a Linux Folder Using xfreerdp


```shell-session
canklesmaks@htb[/htb]$ xfreerdp /v:10.10.10.132 /d:HTB /u:administrator /p:'Password0@' /drive:linux,/home/plaintext/htb/academy/filetransfer
```
