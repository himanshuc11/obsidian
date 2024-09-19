Bash scripting is a way to run commands one after another. Similar to a programming language

**Shell** Program that allows user to interact with the OS services via commands

```bash
ls # List the contents of current directory
ls /path/to/folder # List the contents of folder
pwd # List the (present) current working directory
echo $SHELL # Displays the shell used /bin/bash or /bin/zsh
which x # Prints the location of executable x
```

**Script** Text file used to run commands
To create a script file and execute it
- Create a file `<scriptname>.sh` 
- Edit permissions to make it executable `chmod +x <scriptname>.sh`
- Execute the script using `./<scriptname>.sh`

Note: `<scriptname>.sh` first line should tell us about the bash interpreter
`#!/bin/bash` for using bash as our interpreter.

### Variables
Variables exist as long 
- as terminal session is in place, if defined at terminal level
- as long as script is running, if defined at script level

`<variableName>=value`
`echo "Printing variable contents $<variableName>"`

When using variable `$` help us avoid name conflicts.

`<variable>=$(bash command -with flags)` 
example: `files=$(ls)`

This method helps us capture the output of commands inside a variable.
Internally a sub-shell is created to execute the command and contents of it are populated inside the variable.

Running `env` will list all the environment variables.

Convention
- All caps variable names are system variable names. `$USER`
- Use lowercase variable names
- Lower case variable names are user defined

### Math

`expr val1 operation val2`
```
+  => Addition
-  => Subtraction
\* => Multiplication
\  => Division
```

### Exit Codes
Exit codes tell us if the previously run command was successful or not.
`$?` gives the exit code of previously run command

`0` exit code, means that the previously run command was `successful`
`non zero` exit code means previously run command was `failure`

Note: Think of `$?` as a global variable, that is updated after each command, which sets its value.

We can return custom exit code from our script using `exit <exit-code>`
No line is executed after this code, think of this as a return.

### Test Utility
Test utility evaluates and expression, and returns 0 (true) or 1 (false).  If no expression, returns 1.
```bash
[expression]
```

There are 3 groups of comparison's
- **File Compare** : `-x /path/to/file`. Common flags `-f for file compare and -d for directory compare`
- **String Compare**:  Based on binary value of the string. `s1 = s2, s1 != s2, s1 < s2, s1 > s2`
- **Number Compare** `n1 -flag n2`. Common flags `-eq, -ne, -gt, -lt, -le`

For more info `man test`

### Conditions
```bash
#!/bin/bash

# Vanilla If
if [ condition ]
then
	# Code
fi

# If Else
if [ condition ]
then
	# Code
else
	# Code
fi

```

### Loops

**While Loops**
```bash
#!/bin/bash

while [ condition ] # Test condition can be of any 3 types discussed above
do
	# Code
done
```

**For loops**
For loop, to loop over each item in the set

```bash
#!/bin/bash

for number in 1 2 3 4 5 # Generating the set manually
do
	# Code
done

for number in {start..stop} # Generate the range [start, stop]
do
	# Code
done

for file in ~/path/to/file # Loop over all files in the directory
do
	# Code
done
```

