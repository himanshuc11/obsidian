SSH (Secure Shell) is a protocol that allows secure data transfer between two machines.
By default SSH installed on Mac and Linux, Windows uses Putty.

`which ssh` command is used to see if ssh is installed on pc.

To communicate with a remote server
- The server must expose a port and accept connections
- ssh server component must be installed

By default port 22 is used for ssh.

Connection to remote server
`ssh <usernameOnRemoteServer>@ipAddress`
`ssh root@192.168.1.1`
Note, we can use the `ssh -v <usernameOnRemoteServer>@ipAddress` flag to get verbose description

When connecting to the server for the first time, the client ssh program will ask for confirmation that authenticity of the server cannot be established. The server gives you a host key (SHA fingerprint) for you to verify if you are connecting with the correct server.

Then it prompts you for a password and lets you connect to the remote server via SSH.

After the first connect, the server's fingerprint gets added to a list of `~/.ssh/known_hosts` so next time, client program doesn't ask you verify fingerprint and gets straight to password. It is saved in `known_hosts` to avoid asking for fingerprint verification again and again.

This is done to ensure that when IP of the server changes, the fingerprint will also change making the fingerprint in `known_hosts` invalid and hence we will not connect to a wrong remote server.

**SSH Alias**
Since `ssh <usernameOnRemoteServer>@ipAddress` is not memorable, we can create alias for each host server

Inside the `~/.ssh/config` file, (Can create manually if not present using `touch config`)
```
Host aliasname
	Hostname ipAddress
	Port 22
	User <usernameOnRemoteServer>

Host aliasname2
	Hostname ipAddress2
	Port 22
	User <usernameOnRemoteServer>
```
Now we can directly connect to the remote using `ssh aliasname`


**SSH KEYS**
To avoid entering password each time we connect to the remote server, we can use keys.
We can create a key using `ssh-keygen`

By default uses RSA, but ed25519 algorithm is more secure, to generate key of that format
`ssh-keygen -t ed25519`

This will run a cmd with the following prompts
- path where the key will be stored. (Note that if the key name exists, this will overwrite, hence make sure path is unique)
- Passphrase

Passphrase is a layer of protection on the generated keys. 
If we try to use this key, the client machine will ask you for the passphrase before it uses it.
If you give the wrong passphrase, you cannot use the key. 
Therefore even if the key was leaked, without passphrase it is useless as no one can use the key without it.

After you generate the keys, you will get two keys a private key ie a key with no extension and a public key with the `.pub` extension.
- id_x
- id_x.pub
The x will be the algorithm used to generate the key.

The private key must be stored securely, while the public key can be distributed.

**How to configure server to use key based auth mechanism**
- Connect to the server
- `nano ~/.ssh/authorized_keys`
- Paste the contents of `id_x.pub` inside `authorized_keys` file of the remote server

Now when we try to connect to the server, we don't get the password prompt, only the passphrase, as the server carries auth based on your `id_x` private key.

Alternative to the above process is to use `ssh` command to do the same
`ssh-copy-id -i /path/to/public/key alias`
This will install the public key on the remote server

**SSH key management**
When managing multiple SSH keys, we can use comments to differentiate them.
While generating the key `ssh-keygen -t ed25519 -C "companyName/clientName"`
This comment is just appended at the end of the public key file to keep a alias to key

**SSH Server**
The server component of ssh is `sshd`
`which sshd` shows the path where it is installed

The config of the server ssh is in `sshd_config`
```
Port 22
PermitRootLogin no
PasswordAuthentication no
```

Make `PermitRootLoginNo` **after you create another user** who can login. Since attackers will try to login as root, and we disable it, it ensures security.

Make `PasswordAuthentication` **after you can login with private ssh keys**, so attackers cannot brute force passwords

Note, make sure to restart the ssh service using the command
`systemctl restart sshd`

Check the status of the server through
`systemctl status sshd`

Existing connections do not terminate when you restart ssh.
Hence always keep a login into the remote and test if you can connect with client before closing the remote connection


**SSH Issues**
1. Incorrect permissions. 
	- Public key should be readable by all (user, group, public)
	- Private key should be readable and writeable just by user
	- The `.ssh` folder on remote readable and writeable just by user
	- `authorized_keys` are only readable and writeable by user
2. `cd /var/log && tail -f auth.log` to see the debug logs

[Reference](https://www.youtube.com/watch?v=YS5Zh7KExvE)
