
A [Brute Force](https://en.wikipedia.org/wiki/Brute-force_attack) attack is a method of attempting to guess passwords or keys by automated probing. An example of a brute-force attack is password cracking. Passwords are usually not stored in clear text on the systems but as hash values. see [[Attacking SAM (security accounts manager)]]

Here is a small list of files that can contain hashed passwords:

|**`Windows`**|**`Linux`**|
|---|---|
|unattend.xml|shadow|
|sysprep.inf|shadow.bak|
|SAM|password|
* *the password **cannot** be calculated backward from the hash value, the brute force method determines the hash values **belonging** to the randomly selected passwords until a hash value matches the stored hash value.**
	* also called offline brute-forcing

* On most websites, there is always a login area for administrators, authors, and users somewhere.
	* usernames are often recognizable
	* complex passwords are rarely used because they are difficult to remember

There are many tools and methods to utilize for login brute-forcing, like:
- `Ncrack`
- `wfuzz`
- `medusa`
- `patator`
- `hydra`

The following topics will be discussed:

- Brute forcing basic HTTP auth
- Brute force for default passwords
- Brute forcing login forms
- Brute force usernames
- Creating personalized username and password wordlists based on our target
- Brute forcing service logins, such as FTP and SSH
