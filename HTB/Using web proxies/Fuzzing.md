## Burp
* we used the wild simoleon (lol) to fuzz using the common.txt wordlist.
* we added a match condition to filter 200 OK response.
* we also made an ignore condition if the regex matched a common string.
![[Pasted image 20240513114056.png]]

# ZAP Fuzzer

* by visiting a site's /skills directory, we were, welcomed with a generic "welcome back user"
* we took a cookie from this request in order fuzz it for different md5 hashed usernames using the "top-usernames-shortlist.txt" wordlist to get our flag
* our flag was in the response with a longer content than the others. 
*![[Pasted image 20240513132659.png]]