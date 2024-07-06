## CUPP
 Many tools can create a custom password wordlist based on certain information.
* Cupp asks us questions about the target in a form, and creates a wordlist.

 Since we saw the password policy when we logged in, we know that the password must meet the following conditions:

1. 8 characters or longer
2. contains special characters
3. contains numbers

So, we can remove any passwords that do not meet these conditions from our wordlist. Some tools would convert password policies to `Hashcat` or `John` rules, but `hydra` does not support rules for filtering passwords. So, we will simply use the following commands to do that for us:
```bash
sed -ri '/^.{,7}$/d' william.txt            # remove shorter than 8
sed -ri '/[!-/:-@\[-`\{-~]+/!d' william.txt # remove no special chars
sed -ri '/[0-9]+/!d' william.txt            # remove no numbers
```
We see that these commands shortened the wordlist from 43k passwords to around 13k passwords, around 70% shorter.
## Mangling

It is still possible to create many permutations of each word in that list. We never know how our target thinks when creating their password, and so our safest option is to add as many alterations and permutations as possible, *noting that this will, of course, take much more time to brute force.

Many great tools do word mangling and case permutation quickly and easily, like [rsmangler](https://github.com/digininja/RSMangler) or [The Mentalist](https://github.com/sc0tfree/mentalist.git).

## Custom Username Wordlist

We should also consider creating a personalized username wordlist based on the person's available details. 


For example, the person's username could be `b.gates` or `gates` or `bill`, and many other potential variations. There are several methods to create the list of potential usernames, the most basic of which is simply writing it manually. 

One such tool we can use is [Username Anarchy](https://github.com/urbanadventurer/username-anarchy), which we can clone from GitHub, as follows
```shell-session
canklesmaks@htb[/htb]$ git clone https://github.com/urbanadventurer/username-anarchy.git

Cloning into 'username-anarchy'...
remote: Enumerating objects: 386, done.
remote: Total 386 (delta 0), reused 0 (delta 0), pack-reused 386
Receiving objects: 100% (386/386), 16.76 MiB | 5.38 MiB/s, done.
Resolving deltas: 100% (127/127), done.
```