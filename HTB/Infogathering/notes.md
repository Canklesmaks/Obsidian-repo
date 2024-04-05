## Active subdomain enumeration
* when using *NSLOOKUP* to exploit misconfigured DNS, provide the server FQDN with the IP.
* You need to inspect all the zones for possible vectors and information when enumerating.
![[Pasted image 20240229193328.png]]

## Virtual Hosts 
* It is important as well to inspect any vhosts you may discover when enumerating
![[Pasted image 20240301141202.png]]
* the flag needed was in the _ap vhost_ @ap.inlanefreight.htb"
* USE **CURL** OFTEN for this.
![[Pasted image 20240301141244.png]]
* @app.inlanefreight.htb contained information
![[Pasted image 20240301141823.png]]
* when enumerating its important to take note of and pay special attention to things that may contain high value leads or information.
* **DO NOT graze over other aspects though, be thorough and methodical.**
EXAMPLE:
![[Pasted image 20240301142307.png]]

## SKILLS ASSESMENT
1. "What is the Registrar IANA ID number for githubapp.com domain?"
 ![[Pasted image 20240301144757.png]]
 2. "What is the last mailserver returned when querying the MX records for githubapp.com?"
![[Pasted image 20240301145014.png]]
3.  "Perform active infrastructure identification against the host https://i.imgur.com. What server name is returned for the host?"
![[Pasted image 20240301145615.png]]
4. "Perform subdomain enumeration against the target githubapp.com. Which subdomain has the word 'triage' in the name?"
![[Pasted image 20240301150313.png]]
**active tools such as GOBUSTER may be needed to enumerate domains.** 

## Conclusion
	Use these tools as often as you can so you remember and learn even more. alot of what you had trouble with was knowing when to use which tool. 
	Practice makes perfect!