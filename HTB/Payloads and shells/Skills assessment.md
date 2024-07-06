## H1 

* Ended up being I needed to craft a java payload manually using msfvenom and the generic "Exploit Multi/handler" payload handler
* also had to use the internal network address for the foothold machine 172.16.1.5 duh. I was using the outside facing address. 
* Dir command is useful
* use the remainder of the modules to help

## H2
* We needed to import the module, the path was under data/
* use f12 and other things to find more specific info 
	* its imperitave we take a closer look at the options of the modules and payloads.
		* we needed to specify the vhost and we got stuck
		
## H3
 * SMB exploit ms17 psexec ended up being the vector, try some things you know first.