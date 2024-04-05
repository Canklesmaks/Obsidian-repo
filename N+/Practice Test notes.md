FULLY UNDERSTAND WHAT YOU ARE READING. READ ALLOUD TO YOURSELF EVERY QUESTION.

# Networking fundamentals
APP:          layer 7 is the end user layer with HTTP FTP IRC AND DNS 

PRES:        layer 6 is  syntax related  and encryption happens here. SSL SSH IMAP FTP MPEG JPEG

SESH:        layer 5 is API's and port NUMBERS AKA "sockets"

TRNSPRT:  layer 4 trasport is just tcp/udp protocol 

NWORK:   layer 3 is ALWAYS packets, routers , ipsec igmp icmp                                                                           layer 3 switch can forwards based on destination IP addresses.

DL:            layer 2 is like dumb switches, bridges and point to point  MAC ADR VLANSEGMENTation           layer 2 switch is always forwards based on mac (ARP)

PHYS:       layer 1 is like hubs and cables  and wireless HUBS ARE LAYER 1 BASICALLY A REPEATER
* WAN technologies 
	* DSL and dial-up services can be received over POTS (plain old telephone system)
	* OC-3 is a type of fiber connection
	* WiMAX is a type of microwave connection
	* Starlink is a type of satellite connection. 
	* IPs like: 169.254.0.1 are APIPA (Automatic Private IP Address) , not a class ABC.
	* ANYCAST is ONLY for ipv6
	* 1->7 = encapsulation
	* 7->1 = De-encapsulation
	* STRAIGHT THROUGH is for PC -> hub/switch (OPPOSITE DEVICES)
	* CROSSOVER CABLE is for connecting PC -> PC  (LIKE-DEVICES)
	* When two switches DON'T support MDIX , CROSSOVER
	* ROLLOVER is for CONSOLE port
	* DNS TXT records( they CAN have machine readable code)are a key component of several different email authentication methods (SPF, DKIM, and DMARC)
* HYPERVISORS
	* Read carefully when hypervisors are mentioned. they will often work in tandem with virtualized hardware. vswitch, vrouter, it is the same concept only invisible.
* PORTS #
	* POP3 = 110
	* LDAPS = 636
	* (DNS) = 53
	* DHCP = 67/68
	* (HTTP) = 80
	* SMB = 445
	* SMTP = 25
	* SMTP TLS = 587
	* (IMAP) = 143
	* POP3 = 25
	* IMAP SSL = 993
	* POP3 SSL = 995
	* microsoft sql = 1433
	* mysql = 3306
	* LDAP = 389
* PROTOCOLS
	* MGRE allows one node to communicate with many other nodes to create TUNNELS over another network.
* TELECOM 
	*  T1 LINES are usually terminated at the demarcation point. (MDF)
	* DOCSIS MODEMS are for CABLE/COAX
	
* TERMINATION
	* ![[Pasted image 20230709190649.png]]
* CABLE DISTRIBUTION 
	* Patch panels and cable trays may be used to form the backbone of your cable distribution plant, but PLENUMS and RAISED FLOORS will allow  you to distribute the cabling across the site, out of sight.
# Network Implementations
* WIRING
	* connecting legacy devices to one another requires crossover cables because of their LACK of MDIX (medium dependent interface crossover) support 

	* WIRING DOMAINS
		* COLLISION domains are EACH link and ALL devices within a HUB
		* BROADCAST domains are SEPERATED BY ROUTERS (LANs)

* WIRELESS
	* ROUTER is used to allow segments to communicate.
		* Route poisoning is a method to prevent a router from sending packets through a route that has become invalid within computer networks
	
	* only WPA2 uses AES. the older WEP uses RC4 encryption
	
	 * frequency mismatch can occur between devices using different 802.11 standards.

	* RFID is for asset tags
	* NFC is for communication between two electronic devices over a distance of 4 cm or less.

* ANTENNA*
	* High gain antennas put out increase signal strengths and can reach further distances
	* Low gain antennas spread the power out across a wider volume in space, but the signal reaching the receivers is weaker and harder to process.

* SEGMENTATION
	* VLANS must be tagged (802.1q) to ensure that traffic in vlans gets to the right VLAN.
	* Implementing VLANS will REDUCE BROADCAST TRAFFIC.
	* for traffic to enter and leave a VLAN it must go through a router or layer 3 switch

* FIREWALLS / SECURITY DEVICES 
	* STATEFULL FIREWALLS can track source IP address, destination IP address, port numbers, and the current session status'
	* STATELESS FIREWALLS filter packets based on static rules without considering the packet's relationship to other packets. they are also lightweight and have low processing overhead.*
	* UTM (Unified Threat Management) devices include antivirus and other protections will protect from more things than just a network based firewall
	* IDS will evaluate an entire packet until all of the IDS alert rules have been checked and the packet is allowed to continue its journey.
		* IDS CANNOT BLOCK TRAFFIC
	* USING AN ENCRYPTED ALTERNATIVE WILL NOT PREVENT TRAFFIC FROM TRAVELING OVER A PORT.  <- This is a job for firewalls.


# Network Operations
* BUSINESS OPS CONCEPTS
	* REDUNDANCY is the core of business continuity

	* EMERGENCY
		* UPS (uninterruptible power supply) should provide short term power for long enough that a long term solution to outage can be provided.

* FRAMING, ENCAPSULATION, PACKETS
	* RUNT = Ethernet frame LESS than 64 bytes
	* GIANT = Ethernet frame GREATER than 1518 bytes
	* CRC errors indicates a data corruption during transmission.
		* caused by various factors, including electrical interference, signal attenuation, faulty cabling or connectors, & incorrect network device configurations.
	* ENCAPSULATION errors occur when there is a problem with the encapsulation process in networking protocols
		* can occur due to misconfigurations or compatibility issues between networking devices, mismatched protocols or versions between sender and receiver, improper handling of packet fragmentation or reassembly, or issues with network device firmware or software.
*  CONTRACTUALS
	* Statement of Work (SOW) is a document that outlines all the work that is to be performed, as well as the agreed-upon deliverables and timelines.
	* (SLA) is a written agreement that qualitatively and quantitatively specifies the service committed by a vendor to a customer.*
# Network Security
* ATTACKS
	* On Path / MItM attacks can be where a perpetrator positions himself in a conversation between a user and an application, either to eavesdrop OR impersonate one of the parties. 
	* DoS and DDoS attack occur when bandwidth/resources are flooded with requests.
	* ARP spoofing occurs when a malicious actor sends falsified ARP (Address Resolution Protocol) messages over a local area network. This results in the linking of an attacker's MAC address with the IP address of a legitimate computer or server on the network
	* VLAN hopping uses  a method of "Double tagging"
	* DEAUTHENTICATION ATTACK is type of denial-of-service attack that targets communication between a user and a Wi-Fi wireless access point by sending a deauthentication frame to the victim's machine
		* This causes the wireless client to disconnect from the wireless network and then reconnect.
		* During that reconnection, an attacker can conduct a packet capture of the authentication handshake and use that to attempt to brute force the network's pre-shared key.
*  ENCRYPTION
	* SSL VPN is a type of virtual private network that uses the Secure Sockets Layer protocol in a standard web browser to provide secure, remote-access VPN capability.
* WIRELESS
	* RADIUS can be used when requiring users to authenticate to a DOMAIN before gaining access to a network REMOTELY
* SERVERS
	* Public servers, such as the FTP server, should be installed in a SCREEDNED SUBNET so that additional security mitigations like a web application firewall or application-aware firewall can be used to protect them.
* SITE HARDENING 
	* MAC address filters are useful and are often used to prevent others from connecting to the WIRED network.
	* NAC can identify which machines are known and trusted assets and provide them with access to the secure internal network. NAC could also determine unknown machines and provide them with direct internet access ONLY by placing them onto a guest network or VLAN.
	* APPLICATION aware FIREWALLS can detect and block even TLS connection on various ports.
	* PNAC (802.1x) PAP(port auth protocol) can be use with EAP methods.
	* You can add whole URLs to the browsers group policy block list.
* PATCHING
	* manufacturer will often release a VULNERABILITY PATCH. A vulnerability patch is a piece of software that fixes security issues.
	* FIRMWARE updates will upgrade your device with ADVANCED OPERATIONAL STRUCTURES AND FEATURES without needing a hardware upgrade
* SOCIAL ENGINEERING
	* PIGGYBACKING involves consent. 
	* TAILGATING is done without consent.
* ETC
	* IF an OPEN SOURCE remote access tool is needed, VNC (virtual network computing) is available for linux and MACos. RDP is NOT open source.
	* PATCHING is a great way to combat threats and protect systems
		* by definition, A ZERO DAY THREAT is a flaw in the software, hardware, or firmware that is unknown to the party or parties responsible for patching or otherwise fixing the flaw.
	* A NON-PERSISTENT AGENT is used to access the device during a one-time check-in at login. 
		* it is BEST used to perform a one-time temporary posture assessment in a NAC environment?
		* it is software the client runs (usually from a browser) as they are connecting so the agent can perform the checks, but the software does not permanently stay with the client after they disconnect. This is beneficial in BYOD (Bring Your Own Device) policies

# Network Troubleshooting

* TOOLS
	* NMAP, is a cross-platform, open-source tool used to scan IP addresses and ports on a target network, and to detect running services, applications, or operating systems on that network's clients, servers, and devices.
	* TRACEROUTE command is used on Linux, Unix, and OS X devices to show details about the path that a packet takes from a host to a target and displays information about each hop in the path. While using 
		* PING will tell you if the remote website is reachable or not, it WON'T tell you WHERE the connection is broken.
	* SHOW ROUTE command is used on a Cisco networking device to display the current state of the routing table for a given network device
		* To determine if AN INTERFACE is connected using OSPF or EIGRP, you would need to use the "show route" command to display the current status.
* INTERFACES 
	* on a SWITCH or ROUTER, the FULL-duplex (fdx) setting should be used to increase the throughput of the interface.
	
	* to display the STATISTICS for a given switchport on a CISCO switch
		* ^SHOW INTERFACE^

	* OBSERVING that the switchports' physical interfaces are continually going up and down can indicate a DUPLICATE MAC ADDRESS 
		* One indication of this occurring is when a switch continually changes the port assignments for that address as it updates its content-addressable memory (CAM) table to reflect the physical address and switchport bindings.
			* This will cause the switchports to continually flap by going up and down as the assignments are updated within the CAM table.





* Captive portals usually rely on 802.1x (PNAC), and 802.1x TYPICALLY uses RADIUS for authentication.

* IP phones often do NOT have powersupplys

* remember CIDR notations on a test can be misleading. find the range of the given CIDR, and determine what the subnets ACTUALLY are.

* DB-9 is a connector often used for serial connection
	* RS-232 is the standard for serial communications.
* CABLING STANDARDS*
	* remember the memory aid, IF naming convention contains Base-S as part of its name then it uses a multimode fiber cable.
* CSU/DSU (Channel Service Unit/Data Service Unit) is a hardware device about the size of an external modem that converts digital data frames from the communications technology used on a local area network (LAN) into frames appropriate to a wide-area network (WAN) and vice versa.
	* A CSU/DSU is used to TERMINATE a T1 connection at the customer's site

![[Pasted image 20230718190418.png]]
* This indicates a hard disk bottleneck











# EXTRA 

*

* split horizon  :  Never send routing information back in the direction from which it was received.

* When a router receives a notification about an offline route or node, the router will initiate a HOLD DOWN TIMER **allowing the offline router to recover and not update its routing table until the time expires**. 

* budget of an optical link is a calculation or analysis of the power budget or signal loss.

* convergence mechanisms are triggered when things like changes in the network topology, such as link failures, new network devices, or network updates happen

* CAT 6 = 10g over short
* all ethernet standards will have a T for "twisted pair
* only long range fiber will be single-mode"

* RAS = remote access server = remote desktop gateway


* IEEE
	* 802.3ad = LACP
	* 802.3af = POE
	* 802.1D = STP
	* 802.1X = PNAC
