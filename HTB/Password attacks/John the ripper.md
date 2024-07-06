#### Dictionary Attacks

* Dictionary attacks involve using a pre-generated list of words and phrases (known as a dictionary) to attempt to crack a password. This list of words and phrases is often acquired from various sources, such as publicly available dictionaries, leaked passwords, or even purchased from specialized companies.
#### Brute Force Attacks

* Brute force attacks involve attempting every conceivable combination of characters that could form a password. This is an extremely slow process, and using this method is typically only advisable if there are no other alternatives.
#### Rainbow Table Attacks

* Rainbow table attacks involve using a pre-computed table of hashes and their corresponding plaintext passwords, which is a much faster method than a brute-force attack. However, this method is limited by the rainbow table size – the larger the table, the more passwords, and hashes it can store. Additionally, due to the nature of the attack, it is impossible to use rainbow tables to determine the plaintext of hashes not already included in the table.

## Cracking Modes

`Single Crack Mode` is one of the most common John modes used when attempting to crack passwords using a single password list. It is a brute-force attack, meaning all passwords on the list are tried, one by one, until the correct one is found.

```shell-session
canklesmaks@htb[/htb]$ john --format=<hash_type> <hash or hash_file>
```
#### Wordlist Mode

`Wordlist Mode` is used to crack passwords using multiple lists of words. It is a dictionary attack which means it will try all the words in the lists one by one until it finds the right one. It is generally used for cracking multiple password hashes using a wordlist or a combination of wordlists. It is more effective than Single Crack Mode because it utilizes more words but is still relatively basic. The basic syntax for the command is:


```shell-session
canklesmaks@htb[/htb]$ john --wordlist=<wordlist_file> --rules <hash_file>
```


`Incremental Mode` is an advanced John mode used to crack passwords using a character set. It is a hybrid attack, which means it will attempt to match the password by trying all possible combinations of characters from the character set. This mode is the most effective yet most time-consuming of all the John modes

```shell-session
canklesmaks@htb[/htb]$ john --incremental <hash_file>
```