When the `sudo` command is issued, the system will check if the user issuing the command has the appropriate rights, 
* as configured in `/etc/sudoers`. 
* sudo -l
* any rights entries with the `NOPASSWD` option can be seen without entering a password.
It is easy to misconfigure this. For example, 
* a user may be granted root-level permissions without requiring a password. 
* Or the permitted command line might be specified too loosely, allowing us to run a program in an unintended way, resulting is privilege escalation. 
	* For example, 
	* if the sudoers file is edited to grant a user the right to run a command such as `tcpdump` per the following entry in the sudoers file: `(ALL) NOPASSWD: /usr/sbin/tcpdump` an attacker could leverage this to take advantage of a the **postrotate-command** option.


Two best practices that should always be considered when provisioning `sudo` rights:


|1.|Always specify the absolute path to any binaries listed in the `sudoers` file entry. Otherwise, an attacker may be able to leverage PATH abuse (which we will see in the next section) to create a malicious binary that will be executed when the command runs (i.e., if the `sudoers` entry specifies `cat` instead of `/bin/cat` this could likely be abused).|
|2.|Grant `sudo` rights sparingly and based on the principle of least privilege. Does the user need full `sudo` rights? Can they still perform their job with one or two entries in the `sudoers` file? Limiting the privileged command that a user can run will greatly reduce the likelihood of successful privilege escalation.|