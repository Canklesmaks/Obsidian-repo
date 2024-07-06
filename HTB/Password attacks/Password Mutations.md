In the previous sections, we guessed very simple passwords, but it becomes much more difficult to adapt this to systems that apply password policies that force the creation of more complex passwords.

* Unfortunately, the tendency for users to create weak passwords also occurs despite the existence of password policies.
* Passwords are often created closely related to the service used.
* many employees often select passwords that can have the company's name in the passwords.
* A person's preferences and interests also play a significant role.
	*  These can be pets, friends, sports, hobbies, and many other elements of life

users use the following additions for their password to fit the most common password policies:

| **Description**                        | **Password Syntax** |
| -------------------------------------- | ------------------- |
| First letter is uppercase.             | `Password`          |
| Adding numbers.                        | `Password123`       |
| Adding year.                           | `Password2022`      |
| Adding month.                          | `Password02`        |
| Last character is an exclamation mark. | `Password2022!`     |
| Adding special characters.             | `P@ssw0rd2022!`     |
## Hashcat & JtR
* We can use a very powerful tool called [Hashcat](https://hashcat.net/hashcat/) to combine lists of potential names and labels with specific mutation rules to create custom wordlists.
*  [documentation](https://hashcat.net/wiki/doku.php?id=rule_based_attack) of Hashcat
* the ones listed below are enough for us to understand how Hashcat mutates words.
|**Function**|**Description**|
|---|---|
|`:`|Do nothing.|
|`l`|Lowercase all letters.|
|`u`|Uppercase all letters.|
|`c`|Capitalize the first letter and lowercase others.|
|`sXY`|Replace all instances of X with Y.|
|`$!`|Add the exclamation character at the end.|

Hashcat will apply the rules of `custom.rule` for each word in `password.list` and store the mutated version in our `mut_password.list` accordingly. Thus, one word will result in fifteen mutated words in this case.
#### Generating Rule-based Wordlist

```shell-session
canklesmaks@htb[/htb]$ hashcat --force password.list -r custom.rule --stdout | sort -u > mut_password.list
canklesmaks@htb[/htb]$ cat mut_password.list

password
Password
passw0rd
Passw0rd
p@ssword
P@ssword
P@ssw0rd
password!
Password!
passw0rd!
p@ssword!
Passw0rd!
P@ssword!
p@ssw0rd!
P@ssw0rd!
``````

* `Hashcat` and `John` come with pre-built rule lists that we can use for our password generating and cracking purposes.
* One of the most used rules is `best64.rule`

## Hashcat Existing Rules

```shell-session
/usr/share/hashcat/rules/
```
* We can now use another tool called [CeWL](https://github.com/digininja/CeWL) to scan potential words from the company's website and save them in a separate list
* We can then combine this list with the desired rules and create a customized password list that has a higher probability of guessing a correct password.
* We specify some parameters, like the depth to spider (`-d`), the minimum length of the word (`-m`), the storage of the found words in lowercase (`--lowercase`), as well as the file where we want to store the results (`-w`).

```shell-session
canklesmaks@htb[/htb]$ cewl https://www.inlanefreight.com -d 4 -m 6 --lowercase -w inlane.wordlist
canklesmaks@htb[/htb]$ wc -l inlane.wordlist

326
```
