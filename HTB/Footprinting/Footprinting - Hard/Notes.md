
	Initial scan completed. 
	Right off the bat I'm noticing it is a linux host.
	Pop3 Imap SSH
## First steps
*  You need to see if you can interact with these imap and pop3 services.
	* there are different programs you can use, Use **Curl** too.
## Initial information
* Curl didn't yield as I have no login.
* using the s_client in openssl I was able to connect to Imap and pop3
![[Pasted image 20240304154205.png]]
![[Pasted image 20240304154146.png]]
## Next steps
* Use these open ssl commands to interact with the services and potentiallly list info.
	* POP3 may be of most interest (mailboxes). Be thorough before you seek forums

# Session 2

	Looks like interacting with IMAP/POP3 over openssl is a not go, however after scanning udp ports, it looks like there is an SNMP server that can be fuddled with. I need to footprint that SNMP server and see what can be done to it.
	**Keep track of your commands run here on out.**

# Session 3
	Before using SNMP walk or something to footprint snmp, I want to see if I can use openVAS to get any information on known vulnerabilities 
	fuck openVAS
	
*I beleive this to be the community string for the snmp server.*
	10.129.202.20 [backup] Linux NIXHARD 5.4.0-90-generic #101-Ubuntu SMP Fri Oct 15 20:00:55 UTC 2021 x86_64
*ran and returned*
	┌─[us-academy-2]─[10.10.14.181]─[htb-ac-973140@htb-rihyjmjwey]─[~]
└──╼ [★]$ braa backup@10.129.202.20:.1.3.6.*
10.129.202.20:21ms:.0:Linux NIXHARD 5.4.0-90-generic #101-Ubuntu SMP Fri Oct 15 20:00:55 UTC 2021 x86_64
10.129.202.20:20ms:.0:.10
10.129.202.20:20ms:.0:895896
10.129.202.20:21ms:.0:Admin <tech@inlanefreight.htb>
10.129.202.20:20ms:.0:NIXHARD
10.129.202.20:21ms:.0:Inlanefreight
10.129.202.20:20ms:.0:72
10.129.202.20:20ms:.0:39
10.129.202.20:20ms:.1:.1
10.129.202.20:21ms:.2:.1
10.129.202.20:21ms:.3:.1
10.129.202.20:20ms:.4:.1
10.129.202.20:20ms:.5:.1
10.129.202.20:21ms:.6:.49
10.129.202.20:21ms:.7:.4
10.129.202.20:21ms:.8:.50
10.129.202.20:20ms:.9:.3
10.129.202.20:20ms:.10:.92
10.129.202.20:20ms:.1:The SNMP Management Architecture MIB.
10.129.202.20:21ms:.2:The MIB for Message Processing and Dispatching.
10.129.202.20:20ms:.3:The management information definitions for the SNMP User-based Security Model.
10.129.202.20:20ms:.4:The MIB module for SNMPv2 entities
10.129.202.20:21ms:.5:View-based Access Control Model for SNMP.
10.129.202.20:20ms:.6:The MIB module for managing TCP implementations
10.129.202.20:20ms:.7:The MIB module for managing IP and ICMP implementations
10.129.202.20:20ms:.8:The MIB module for managing UDP implementations
10.129.202.20:21ms:.9:The MIB modules for managing SNMP Notification, plus filtering.
10.129.202.20:20ms:.10:The MIB module for logging SNMP Notifications.
10.129.202.20:20ms:.1:39
10.129.202.20:20ms:.2:39
10.129.202.20:20ms:.3:39
10.129.202.20:21ms:.4:39
10.129.202.20:20ms:.5:39
10.129.202.20:20ms:.6:39
10.129.202.20:20ms:.7:39
10.129.202.20:20ms:.8:39
10.129.202.20:20ms:.9:39
10.129.202.20:20ms:.10:39
10.129.202.20:20ms:.0:896826
10.129.202.20:20ms:.0:�
10.129.202.20:20ms:.0:393216
10.129.202.20:23ms:.0:BOOT_IMAGE=/vmlinuz-5.4.0-90-generic root=/dev/mapper/ubuntu--vg-ubuntu--lv ro ipv6.disable=1 maybe-ubiquity

10.129.202.20:21ms:.0:0
10.129.202.20:21ms:.0:158
10.129.202.20:21ms:.0:0
10.129.202.20:20ms:.0:1
10.129.202.20:20ms:.80:/opt/tom-recovery.sh
**10.129.202.20:21ms:.80:tom NMds732Js2761**
10.129.202.20: Message cannot be decoded!
10.129.202.20: Message cannot be decoded!
10.129.202.20: Message cannot be decoded!

	USING "1 LOGIN tom NMds732Js2761" to check imap
using other imap commands i found his ssh private key, I need to use it to log in via ssh
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----

checked bash history and logged into the sql server. 

user found.
![[Pasted image 20240313151534.png]]

![[Pasted image 20240313151607.png]]

SUCCESS!