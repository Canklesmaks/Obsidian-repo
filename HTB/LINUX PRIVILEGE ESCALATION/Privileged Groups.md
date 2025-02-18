## LXC / LXD
LXD is similar to Docker and is Ubuntu's container manager. 
* Upon installation, all users are added to the LXD group. 
* Membership of this group can be used to escalate privileges by creating an LXD container, making it privileged, and then accessing the host file system at `/mnt/root`.

## Docker
Placing a user in the docker group is essentially equivalent to root level access to the file system without requiring a password. 
* Members of the docker group can spawn new docker containers. One example would be running the command `docker run -v /root:/mnt -it ubuntu`. 
* This command create a new Docker instance with the /root directory on the host file system mounted as a volume. 
* Once the container is started we are able to browse to the mounted directory and retrieve or add SSH keys for the root user. 
	* This could be done for other directories such as `/etc` which could be used to retrieve the contents of the `/etc/shadow` file for offline password cracking or adding a privileged user.
## Disk
Users within the disk group have full access to any devices contained within `/dev`, 
* such as `/dev/sda1`, which is typically the main device used by the operating system. 
* An attacker with these privileges can use `debugfs` to access the entire file system with root level privileges. 
* As with the Docker group example, this could be leveraged to retrieve SSH keys, credentials or to add a user.
## ADM
Members of the adm group are able to read all logs stored in `/var/log`. This does not directly grant root access, but could be leveraged to gather sensitive data stored in log files or enumerate user actions and running cron jobs.
