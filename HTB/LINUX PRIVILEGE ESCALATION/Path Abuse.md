w[PATH](http://www.linfo.org/path_env_var.html) is an environment variable that specifies the set of directories where an executable can be located.\
We can check the contents of the PATH variable by typing `env | grep PATH` or `echo $PATH`****

the `conncheck` script created in `/usr/local/sbin` will still run when in the `/tmp` directory because it was created in a directory specified in the PATH.

Adding `.` to a user's PATH adds their current working directory to the list.

if we can modify a user's path, we could replace a common binary such as `ls` with a malicious script such as a reverse shell.

