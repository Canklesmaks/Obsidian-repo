with the admin panels, we can manage servers, their services, and configurations. Many admin panels have also implemented features or elements such as the [b374k shell](https://github.com/b374k/b374k) that might allow us to execute OS commands directly.

To cause as little network traffic as possible, it is recommended to try the top 10 most popular administrators' credentials

If none of these credentials grant us access, we could next resort to another widespread attack method called **password spraying.
* this attack method is based on reusing already found, guessed, or decrypted passwords across multiple accounts.
* Since we have been redirected to this admin panel, the same user may have access here.

## Brute Forcing Forms

`Hydra` provides many different types of requests we can use to brute force different services. If we use `hydra -h`, we should be able to list supported services

```shell-session
canklesmaks@htb[/htb]$ hydra -h | grep "Supported services" | tr ":" "\n" | tr " " "\n" | column -e

Supported			        ldap3[-{cram|digest}md5][s]	rsh
services			        memcached					rtsp
				            mongodb						s7-300
adam6500			        mssql						sip
asterisk			        mysql						smb
cisco				        nntp						smtp[s]
cisco-enable		        oracle-listener				smtp-enum
cvs				            oracle-sid					snmp
firebird			        pcanywhere					socks5
ftp[s]				        pcnfs						ssh
http[s]-{head|get|post}		pop3[s]						sshkey
http[s]-{get|post}-form		postgres					svn
http-proxy		        	radmin2						teamspeak
http-proxy-urlenum		    rdp				  		    telnet[s]
icq				            redis						vmauthd
imap[s]		        		rexec						vnc
irc				            rlogin						xmpp
ldap2[s]		        	rpcap
```
In this situation there are only two types of `http` modules interesting for us:

1. `http[s]-{head|get|post}`
2. `http[s]-post-form`

The 1st module serves for basic HTTP authentication, while the 2nd module is used for login forms, like `.php` or `.aspx` and others.

Since the file extension is "`.php`" we should try the `http[s]-post-form` module. To decide which module we need, we have to determine whether the web application uses `GET` or a `POST` form. We can test it by trying to log in and pay attention to the URL. If we recognize that any of our input was pasted into the `URL`, the web application uses a `GET` form. Otherwise, it uses a `POST` form.