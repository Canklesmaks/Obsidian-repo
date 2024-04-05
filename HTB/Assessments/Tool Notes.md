
`Nessus`, `Nexpose`, and `Qualys` are well-known vulnerability scanning platforms that also provide free community editions. There are also open-source alternatives such as `OpenVAS`.

## Nessus Overview
[Nessus Essentials](https://community.tenable.com/s/article/Nessus-Essentials) by Tenable is the free version of the official Nessus Vulnerability Scanner. Individuals can access Nessus Essentials to get started understanding Tenable's vulnerability scanner. *The caveat is that it can only be used for up to 16 hosts.* The features in the free version are limited but are perfect for someone looking to get started with Nessus. The free scanner will attempt to identify vulnerabilities in an environment.
![[Pasted image 20240307161059.png]]

![[Pasted image 20240307161130.png]]
## Nessus Plugins etc
Nessus works with plugins written in the [Nessus Attack Scripting Language (NASL)](https://en.wikipedia.org/wiki/Nessus_Attack_Scripting_Language) and can target new vulnerabilities and CVEs. These plugins contain information such as the vulnerability name, impact, remediation, and a way to test for the presence of a particular issue.

* Nessus also supports credentialed scanning and provides a lot of flexibility by supporting LM/NTLM hashes, Kerberos authentication, and password authentication.
  * Nessus also supports authentication for a variety of databases types including Oracle, PostgreSQL, DB2, MySQL, SQL Server, MongoDB, and Sybase:
  * In addition to that, Nessus can perform plaintext authentication to services such as FTP, HTTP, IMAP, IPMI, Telnet, and more:
  
 The Executive Summary report provides a listing of hosts, a total number of vulnerabilities discovered per host, and a `Show Details` option to see the severity, CVSS score, plugin number, and name of each discovered issue. The plugin number contains a link to the full plugin writeup from the Tenable plugin database.
 
  * The PDF option provides the scan results in a format that is easier to share
  * The CSV report option allows us to select which columns we would like to export.
  * This is particularly useful if importing the scan results into another tool such as *Splunk*if a document needs to be shared with many internal stakeholders responsible for remediation of the various assets scanned or to perform analytics on the scan data.
  
  **Note:** These scan reports should only be shared as either an appendix or supplementary data to a custom penetration test/vulnerability assessment report. They should not be given to a client as the final deliverable for any assessment type.
  * It is best to always make sure the vulnerabilities are grouped together for a clear understanding of each issue and the assets affected.
  * We can also write our own scripts to automate many Nessus features.
## Exporting Nessus Scans

Nessus gives the option to export scans into two formats `Nessus (scan.nessus)` or `Nessus DB (scan.db)`. The `.nessus` file is an `.xml` file and includes a copy of the scan settings and plugin outputs. The `.db` file contains the `.nessus` file and the scan's KB, plugin Audit Trail, and any scan attachments.

* Scripts such as the [nessus-report-downloader](https://raw.githubusercontent.com/eelsivart/nessus-report-downloader/master/nessus6-report-downloader.rb) can be used to quickly download scan results in all available formats from the CLI using the Nessus REST API:
* we can also write our own.
## Nessus scanning issues

	It is always best to communicate with your client (or internal stakeholders if running a scan against your own network) on whether any sensitive/legacy hosts should be excluded from the scan or if any high priority/high availability hosts should be scanned separately, outside of regular business hours, or with different scan configurations to avoid potential issues.

* There are also times when a scan may return unexpected results and need to be fine-tuned.
## Mitigating Issues
  * Some firewalls will cause us to receive scan results showing either all ports open or no ports open.
	  * If this happens, a quick fix is often to configure an Advanced Scan and disable the `Ping the remote host` option. This will stop the scan from using ICMP to verify that the host is "live" and instead proceed with the scan.

* Nessus does not have an option for an "exclusion list" of hosts within a CIDR range like we can do with tools like Nmap.
* unless specifically requested, we should never perform [Denial of Service checks](https://www.tenable.com/plugins/nessus/families/Denial%20of%20Service).
	* We can ensure that these types of plugins are not used by always enabling the ["safe checks"](https://www.tenable.com/blog/understanding-the-nessus-safe-checks-option) option when performing scans to avoid any network plugins that can have a negative impact on a target, such as crashing a network daemon.

## Network Impact

It is also essential to keep in mind the potential impact of vulnerability scanning on a network, especially on low bandwidth or congested links. This can be measured using [vnstat](https://humdi.net/vnstat/)




## OpenVAS Overview
[OpenVAS](https://www.openvas.org/) by Greenbone Networks is a publicly available open-source vulnerability scanner. OpenVAS can perform network scans, including authenticated and unauthenticated testing
  

The Executive Summary report provides a listing of hosts, a total number of vulnerabilities discovered per host, and a `Show Details` option to see the severity, CVSS score, plugin number, and name of each discovered issue. The plugin number contains a link to the full plugin writeup from the Tenable plugin database.

## install 

```shell-session
canklesmaks@htb[/htb]$ sudo apt-get update && apt-get -y full-upgrade
canklesmaks@htb[/htb]$ sudo apt-get install gvm && openvas
```
```shell-session
canklesmaks@htb[/htb]$ gvm-setup
```

This will begin the setup process and take up to 30 minutes.^^^

```shell-session
canklesmaks@htb[/htb]$ gvm-start
```
after gvm-start you should get a pointer to the web interface.\

## network scan type synopsis
	these are templates, but are reccomended.
- `Base`: This scan configuration is meant to enumerate information about the host's status and operating system information. This scan configuration does not check for vulnerabilities.
    
- `Discovery`: This scan configuration is meant to enumerate information about the system. The configuration identifies the host's services, hardware, accessible ports, and software being used on the system. This scan configuration also does not check for vulnerabilities.
    
- `Host Discovery`: This scan configuration solely tests whether the host is alive and determines what devices are `active` on the network. This scan configuration does not check for vulnerabilities as well. _OpenVAS leverages ping to identify if the host is alive._
    
- `System Discovery`: This scan enumerates the target host further than the 'Discovery Scan' and attempts to identify the operating system and hardware associated with the host.
    
- `Full and fast`: This configuration is recommended by OpenVAS as the safest option and leverages intelligence to use the best NVT checks for the host(s) based on the accessible ports.
## Exporting Formats

There are various export formats for reporting purposes, including XML, CSV, PDF, ITG, and TXT. If you choose to export your report out as an XML, you can leverage various XML parsers to view the data in an easier to read format.

## exporting results 
```shell-session
canklesmaks@htb[/htb]$ python3 -m openvasreporting -i report-2bf466b5-627d-4659-bea6-1758b43235b1.xml -f xlsx
```
openvasreporting will generate excel spreadsheets such as 
![[Pasted image 20240313105656.png]]
