`Targets` are unique operating system identifiers taken from the versions of those specific operating systems which adapt the selected exploit module to run on that particular version of the operating system.

The `show targets` command issued within an exploit module view will display all available vulnerable targets for that specific exploit.

```shell-session
msf6 > show targets
```

Leaving the selection to `Automatic` will let msfconsole know that it needs to perform service detection on the given target before launching a successful attack.

```shell-session
msf6 exploit(windows/browser/ie_execcommand_uaf) > options

Module options (exploit/windows/browser/ie_execcommand_uaf):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   OBFUSCATE  false            no        Enable JavaScript obfuscation
   SRVHOST    0.0.0.0          yes       The local host to listen on. This must be an address on the local machine or 0.0.0.0
   SRVPORT    8080             yes       The local port to listen on.
   SSL        false            no        Negotiate SSL for incoming connections
   SSLCert                     no        Path to a custom SSL certificate (default is randomly generated)
   URIPATH                     no        The URI to use for this exploit (default is random)


Exploit target:

   Id  Name
   --  ----
   0   Automatic
```
If we, however, know what versions are running on our target, we can use the `set target <index no.>` command to pick a target from the list.

```shell-session
msf6 exploit(windows/browser/ie_execcommand_uaf) > set target 6

target => 6
```
## Target Types

There is a large variety of target types. Every target can vary from another by service pack, OS version, and even language version. It all depends on the return address and other parameters in the target or within the exploit module.

