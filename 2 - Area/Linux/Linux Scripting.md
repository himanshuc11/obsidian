### Paths
Linux file system is a hierarchical structure, with the **root** directory always at the top (`/`).

There are two types of paths in Linux
- Absolute path: They begin with a `/` and specify the location relative to a **root** directory.
- Relative path: The don't begin with a `/`. They specify location relative to the current directory `pwd`.

Common abbreviations
- `~` refers to the home directory
- `.` refers to the current directory
- `..` refers to the parent directory

`pwd` command prints path to present working directory
`ls` list all the contents of `pwd`
`ls -l` list all contents of `pwd` in long format

`cd` is used to change directory.

### Permissions
Each file has a set of permissions associated to it.
To view details use `ls -l`
`flagBit ___ ___ ___`

The first bit is a flag bit. Possible values
- `d` -> Indicates the item is a directory.
- `-` -> Indicates the item is a file.
- `l` -> Indicates the item is a link.

The next set of 3  characters represent user owner permissions
- `r` -> The user has read permission, read contents of file or folder
- `w` -> The user has write permission, write contents to file or folder
- `x` -> Execute the file (if it is a file) or allows user to `cd` into the directory

The next set of 3 characters represent the group owner permissions
Here also the values are `r, w and x`, meaning same as above.

The last set of 3 characters represent the world permissions, permissions for other users
Here also the values are `r, w and x`, meaning same as above.

Changing permission
In Linux only 2 people usually change permissions
- root user
- owner of the file

Basic security is to give users full access to home directory and no other permissions

`chmod +r/w/x <filename>` : Set this bit for all the user/group and world
`chmod u/g/o-r/w/x <filename>`: Unset `r/w/x` for `user/group/other`
`chmod u/g/o+r/w/x <filename>`: Set bit for particular type of user 

`r, w, x` can be translated into numbers as well
`r` -> 4
`w` -> 2
`x` -> 1

`chmod d1d2d3 <filename>`
Here each digit represents the permissions for each group of users
Basically, convert into binary and see which bits are set.

`7` -> `rwx`
`5` -> `r-x`

There is a `-R` flag to recursively `chmod` of the folder.

`chown` is used to change ownership
`sudo chown -R user:group <folder>`

### Files
In Linux everything is a file. Keyboard is a read only file, Monitor is a write only file etc.
Linux is an extension-less system, in which the `.extension` doesn't really matter for the `<file>.extension`. Linux looks inside the file to determine it's type

- Linux is case sensitive
- Cannot have spaces in name. `cd 'n1 n2 n3'` or `cd n1\ n2\ n3` to escape spaces
- Some files are hidden from the user. If a file starts with `.` it is a hidden file. `ls -a` to list the hidden files.

### Common file and folder commands
`mkdir <folder>` make a folder
`mkdir -p <parent>/<child>` make the hierarchy as it goes
`mkdir -v <folder>` verbose logs

`rmdir <folder>` removes the folder, provided it is already empty
`rmdir -p <parent>/<child>` remove the hierarchy
`rmdir -v <folder>` verbose logs

`touch <filname>` create a new file

`cp -r <source> <destination>` to recursively copy the file
`mv <source> <destination>`
`rm <file>` remove a file
`rm -r <folder>` remove a non empty file

### Wildcards
Wildcards help us define a pattern for defining set of paths (files and directories are just paths).

- `*` 0 or more characters
- `?` represent a single character
- `[]` represent a set of characters
- `[startChar-stopChar]` is used to generate a range of characters

Note: bash shell translates the wildcards into actual files, not the command itself. 
Wildcards are part of bash, not the command itself.

### man page cheat code
`man <command-to-look-up>`
To search by keyword `man -k <search-term>`

Inside the man pages, search for the term using `/<term>`


`command [options]`

`[]` square bracket tells us that options are optional. Command may be run with or without them.



Reference:
https://ryanstutorials.net/linuxtutorial/navigation.php 

Online compiler
https://www.onworks.net/playonline/index.php

Self Hosting: https://www.youtube.com/watch?v=Q1Y_g0wMwww&list=PLLnpHn493BHHAxTeLNUZEDLYc8uUwqGXa 


