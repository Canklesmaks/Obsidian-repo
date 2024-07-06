When configuring networks, we sometimes work with vast infrastructures (depending on the company's size) that can have many hundreds of interfaces. Often one network device, such as a router, printer, or a firewall, is overlooked, and the `default credentials` are used, or the same `password is reused`.
## Credential Stuffing
* There are various databases that keep a running list of known default credentials. One of them is the [DefaultCreds-Cheat-Sheet](https://github.com/ihebski/DefaultCreds-cheat-sheet).
* Attacking those services with the default or obtained credentials is called [Credential Stuffing](https://owasp.org/www-community/attacks/Credential_stuffing).
	* This is a simplified variant of brute-forcing because only composite usernames and the associated passwords are used.

* After searching the internet for the default credentials, we can create a new list that separates these composite credentials with a colon (`username:password`). \
	* In addition, we can select the passwords and mutate them by our `rules` to increase the probability of hits.