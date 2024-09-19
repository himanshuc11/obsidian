Users in context of a Linux server refer to who can access the server, and what are the files the user is able to access

`root` user is a user account that exists on all Linux distributions. It has un-restricted access to the system files and folders. It's assumed on Linux system that running as root, you know what you are doing, hence there may not be confirmation prompts as well.

Note: It is recommended to use root, only when needed. Instead of using root outright, we can run individual commands with elevated privileges using `sudo` instead of being root all the time.
The purpose of sudo is to enable user to do things that only root user can do.

`sudo <command>`
`Enter your password: `
The password you entered is cached, and not asked for again till a long time.
Ubuntu Server automatically gives the first user account created during installation access to `sudo`.
Additional users created may or may not have access to `sudo`, based on what is granted to them.

**List Users**
Each user generally has their own home directory, so to list the users on a system
`ls -l /home`

Etsy password
There is a file called `/etc/passwd` 
Each line in this file corresponds to a user account on the system.
`username:x:uid:gid:displayName:firstName,lastName:/home/<username>:/bin/bash`

`x` in second column tells password is not stored here, and is actually stored in `/etc/shadow`

userId assigned to the first user is 1000. 
Human usable accounts have userIds >= 1000.
The next userId is autoIncremented
userIds < 1000 are system accounts

**Creating users**

`useradd`  by default adds user accounts.
The default `useradd` is present in `/etc/default/useradd`.
This varies based on distribution used.

Include all the options, so that the command can be portable

`sudo useradd -m <username>`
`-m` flag used to create a home directory for the user
`ls -l /home` will show the user directory.

`sudo useradd -d /home/<username> -m <username>`
`-d /path/to/user/home` is used to specify where to create the user home directory.
`/home/username` is the convention of home directory.

**Creating password**
`passwd` command is used to change the password that user is logged in with, based on the current password.

`sudo passwd <username>`
`sudo` let's you skip entering `<username>'s`  password, with `sudo` we can directly change password of all users



`adduser` is a better version of `useradd` and uses cmd to ask you for all the options.
`adduser` is not available on all distro's of Linux

`sudo adduser <username>`
It adds user, group, home directory for the user



**Delete users**
`sudo userdel <username>`
This command doesn't delete the home directory if present.

`sudo userdel -r <username>`.
Removes user-account and home directory.

**Special files**
`/etc/shadow`
`/etc/passwd`
`/etc/skel`

`/etc/shadow` stores the hashed passwords
`<username>:hashedPassword:numberOfDaysTillPasswordChange` 

