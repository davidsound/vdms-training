## Basic Scripting Lab

In this lab we are going to dive into basic scripting using the Bash shell.  By the end of this lab, we will create a `backup.sh` script that backs up files in our `~/configs` directory

**This lab will take place on your `centos` host**

### Task 1 - Basic Scripting

Bash scripting is a way of taking Linux shell/cli command and building upon them.  Bash scripts are run serially - in the sequence the commands are entered.

##### Step 1 - shebang and echo

Bash scripts start with a sequence `#!/bin/bash`.  The `#!` is commonly called [`shebang`](https://en.wikipedia.org/wiki/Shebang_(Unix))

This sequence provides details on which interpreter to use to execute the lines that follow. 

Create a file called `backup.sh` and copy the following contents:

```bash
#!/bin/bash

echo "HELLO WORLD!"
```

Once saved, provide `execution` permission to the script. Do this by running the command `chmod +x backup.sh` within the same directory.

Now run the script:

```bash
[ntc@ntc ~]$ chmod +x backup.sh 
[ntc@ntc ~]$ ./backup.sh
HELLO WORLD!
[ntc@ntc ~]$ 
```

> If the script was not provided execution permissions you would see the following error.

```bash
[ntc@ntc ~]$ ./backup.sh
-bash: ./backup.sh: Permission denied
```

##### Step 2 - Variables

Build upon the previous script by adding variables.  Variables provide an easy way to account for varations in your script.  For example, change the `echo command` to look like this:

```bash
#!/bin/bash

SCRIPT_DESC="This script will show the value of an environmental variable."

echo $SCRIPT_DESC
```

Run the script and view the output:

```bash
[ntc@ntc ~]$ ./backup.sh 
This script will show the value of an environmental variable.
[ntc@ntc ~]$ 
```

> Note: There is no need to update file permissions after each change.

Change the script again:

```bash
#!/bin/bash

SCRIPT_DESC="Find value of environmental variable below."

echo $SCRIPT_DESC
```

This will change the output given when the script is run.

```bash
[ntc@ntc ~]$ ./backup.sh  
Find value of environmental variable below.
[ntc@ntc ~]$ 
```

Variables are assigned using a `VARIABLE=VALUE` syntax

> It is important to note that in bash there are no spaces before or after the `=` in the variable assignment.

They are called using a `$VARIABLE` syntax, which translates to `VALUE`
 

##### Step 3 - External Variables

Varibles can also be referenced from external sources.  For example, many built-in variables called [environment variables](https://www.centoshowtos.org/environment-variables/) are automatically imported into bash scripts.

Edit the script to call the external environmental variable `$USER`.

```bash
#!/bin/bash

SCRIPT_DESC="Find value of environmental variable below."

echo $SCRIPT_DESC
echo "User:" $USER
```

Run the script and view the output:

```bash
[ntc@ntc ~]$ ./backup.sh  
Find value of environmental variable below.
User: ntc
[ntc@ntc ~]$  
```
Where did the value of `$USER` come from?

In your terminal execute the following command:

```bash
[ntc@ntc ~]$ echo $USER
ntc
[ntc@ntc ~]$
```

Since `$USER` is an environment variable, bash will evaluate it automatically.  This can be  useful when building scripts.

> CHALLENGE: Incorporate other environment variables in your script. View them by executing `env`.










### Task 2 - Conditionals

Conditionals provide an easy way to change a scripts behavior based on a certain value, assignment, or input.

The basic syntax for conditionals is `if ... then`.

A real example:

```bash
if [ 1 == 1 ]; then
  echo "1 is 1"
fi
```

In bash the `if .. fi` indicates the beginning and end of an `if, then` condntional.  


##### Step 1 - Basic conditional

Building on the previous script, we will add a conditional:

```bash
#!/bin/bash

echo "Backup Script"

echo "Does the configs directory exist?"

if [ -d "/home/${USER}/configs" ]; then
  echo "Yes.  It does exist."
fi
```

This `if/then` conditional checks to see if the `./configs` directory exists in the current directory.  If this case that directory exists in the `ntc` users home directory.  Since this directory does exist, the script returns `Yes.  It does exist.`.

```bash
[ntc@ntc ~]$ ./backup.sh 
Backup Script
Does the configs directory exist?
Yes.  It does exist.
[ntc@ntc ~]$ 
```

##### Step 2 - Advanced Conditionals

The script also needs logic in place to report if the `./configs` directory did not exist.

```bash
#!/bin/bash

echo "Backup Script"

echo "Does the configs directory exist?"

if [ -d "/home/${USER}/configs" ]; then
  echo "Yes. It does exist."
else
  echo "No. It does not exist."
fi
```

This adds an `else` statement.  This says that `if` the `./configs` directory does not exist return `No. It does not exist.`

If you run the script again the ouput will be the same.

```bash
[ntc@ntc ~]$ ./backup.sh   
Backup Script
Does the configs directory exist?
Yes. It does exist.
[ntc@ntc ~]$
```

##### Step 3

Add another conditional to check and see if the `backups` directory exists in the current directory (which is the `ntc` user home directory).

```bash
#!/bin/bash

echo "Backup Script"

echo "Does the configs directory exist?"

if [ -d "/home/${USER}/configs" ]; then
  echo "Yes. It does exist."
else
  echo "No. It does not exist."
fi

echo "Does the backups directory exist?"

if [ -d "/home/${USER}/backups" ]; then
  echo "Yes. It does exist."
else
  echo "No. It does not exist."
fi
```

Since the `backups` directory does not exist, the script will tell us that it does not exist.

```bash
[ntc@ntc ~]$ ./backup.sh
Backup Script
Does the configs directory exist?
Yes. It does exist.
Does the backups directory exist?
No. It does not exist.
```
