A `Payload` in Metasploit refers to a module that aids the exploit module in (typically) returning a shell to the attacker. The payloads are sent together with the exploit itself to bypass standard functioning procedures of the vulnerable service (`exploits job`) and then run on the target OS to typically return a reverse connection to the attacker and establish a foothold (`payload's job`).]

three different types of payload modules in the Metasploit Framework:
* singles
* stagers
* stages
#### Singles
A `Single` payload contains the exploit and the entire shellcode for the selected task.

some exploits will not support the resulting size of these payloads as they can get quite large.

`Singles` are self-contained payloads. They are the sole object sent and executed on the target system, getting us a result immediately after running. A Single payload can be as simple as adding a user to the target system or booting up a process.

#### Stagers
`Stager` payloads work with Stage payloads to perform a specific task.

`Stagers` are typically used to set up a network connection between the attacker and victim and are designed to be small and reliable. Metasploit will use the best one and fall back to a less-preferred one when necessary.

#### Stages
`Stages` are payload components that are downloaded by stager's modules.
The various payload Stages provide advanced features with no size limits, such as Meterpreter, VNC Injection, and others. Payload stages automatically use middle stagers:

- A single `recv()` fails with large payloads
- The Stager receives the middle stager
- The middle Stager then performs a full download
- Also better for RWX
## Staged Payloads

A staged payload is, simply put, an `exploitation process` that is modularized and functionally separated to help segregate the different functions it accomplishes into different code blocks, each completing its objective individually but working on chaining the attack together.

The scope of this payload, as with any others, besides granting shell access to the target system, is to be as compact and inconspicuous as possible to aid with the Antivirus (`AV`) / Intrusion Prevention System (`IPS`) evasion as much as possible.

`Stage0` of a staged payload represents the initial shellcode sent over the network to the target machine's vulnerable service, which has the sole purpose of initializing a connection back to the attacker machine.
* As a Metasploit user, we will meet these under the common names `reverse_tcp`, `reverse_https`, and `bind_tcp`.
* Reverse connections are less likely to trigger prevention systems like the one initializing the connection is the victim host, which most of the time resides in what is known as a `security trust zone`.

Stage0 code also aims to read a larger, subsequent payload into memory once it arrives
* the attacker machine will most likely send an even bigger payload stage which should grant them shell access.
This larger payload would be the `Stage1` payload. We will go into more detail in the later sections.
#### Meterpreter Payload
The `Meterpreter` payload is a specific type of multi-faceted payload that uses `DLL injection` to ensure the connection to the victim host is stable, hard to detect by simple checks, and persistent across reboots or system changes.

## Searching for Payloads
 Meterpreter payloads offer us a significant amount of flexibility. Their base functionality is already vast and influential. We can automate and quickly deliver combined with plugins such as [GentilKiwi's Mimikatz Plugin](https://github.com/gentilkiwi/mimikatz) parts of the pentest while keeping an organized, time-effective assessment. To see all of the available payloads, use the `show payloads` command in `msfconsole`.
```shell-session
msf6 > show payloads
```
* there are a lot of available payloads to choose from. Not only that, but we can create our payloads using `msfvenom

We can also use `grep` in `msfconsole` to filter out specific terms. This would speed up the search and, therefore, our selection.

```shell-session
msf6 exploit(windows/smb/ms17_010_eternalblue) > grep meterpreter show payloads
```

we can add another `grep` command after the first one and search for `reverse_tcp`.

```shell-session
msf6 exploit(windows/smb/ms17_010_eternalblue) > grep meterpreter grep reverse_tcp show payloads

   15  payload/windows/x64/meterpreter/reverse_tcp                          normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse TCP Stager
   16  payload/windows/x64/meterpreter/reverse_tcp_rc4                      normal  No     Windows Meterpreter (Reflective Injection x64), Reverse TCP Stager (RC4 Stage Encryption, Metasm)
   17  payload/windows/x64/meterpreter/reverse_tcp_uuid                     normal  No     Windows Meterpreter (Reflective Injection x64), Reverse TCP Stager with UUID Support (Windows x64)
   ```

To set the payload for the currently selected module, we use `set payload <no.>` only after selecting an Exploit module to begin with.

```shell-session
msf6 exploit(windows/smb/ms17_010_eternalblue) > set payload 15
```

After selecting a payload, we will have more options available to us.

focusing on `LHOST` and `LPORT` (our attacker IP and the desired port for reverse connection initialization). Of course, if the attack fails, **we can always use a different port and relaunch the attack.
