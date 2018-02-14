## Basic Scripting Lab

In this lab we are going to dive into basic scripting using the Bash shell.  By the end of this lab, we will create a `backup.sh` script that backs up files in our `~/configs` directory

### Task 1 - Basic Scripting

Bash scripting is a way of taking Linux shell/cli command and building upon them.  Bash scripts are run serially - in the sequence the commands are entered.

##### Step 1 - shebang and echo

Bash scripts start with a sequence `#!/bin/bash`.  The `#!` is commonly called [`shebang`](https://en.wikipedia.org/wiki/Shebang_(Unix))

This sequence provides details on which interpreter to use to execute the lines that follow. 

Create a file called `hello.sh` and copy the following contents:

```bash
#!/bin/bash

echo "HELLO!"
```

Once saved, we will need to provide `execution` permission to the script.  We can do this by running the command `chmod +x hello.sh` within the same directory.

Now, let's run the script:

```bash
[ntc@ntc ~]$ chmod +x hello.sh 
[ntc@ntc ~]$ ./hello.sh 
HELLO!
[ntc@ntc ~]$ 
```

> If we did not add execution permissions to the script, we would see this output:

```bash
[ntc@ntc ~]$ ./hello.sh
-bash: ./hello.sh: Permission denied
```

##### Step 2 - Variables

Let's build upon our script by adding variables.  Variables provide an easy way to account for varations in your script.  For example, let's change our `echo command` to look like this:

```bash
#!/bin/bash

HELLO="How's it going today?"

echo $HELLO
```

Run the script & view the output:

```bash
[ntc@ntc ~]$ ./hello.sh 
How's it going today?
[ntc@ntc ~]$ 
```

> Note: Notice that we do not have to change permissions every time we change the file

Lets change the script again:

```bash
#!/bin/bash

HELLO="You Changed ME!"

echo $HELLO
```

You can probably guess the output:

```bash
[ntc@ntc ~]$ ./hello.sh 
You Changed ME!
[ntc@ntc ~]$ 
```

Variables are assigned using a `VARIABLE=VALUE` syntax

They are called in using a `$VARIABLE` syntax, which translates to `VALUE`

Also, it is important to note that in bash, there are no spaces before or after the `=` in the variable assignment.

##### Step 3 - External Variables

Varibles can also be referenced from external sources.  For example, many built-in variables called [environment variables](https://www.centoshowtos.org/environment-variables/) are automatically imported into bash scripts.

Let's edit our script:

```bash
#!/bin/bash

GREETING="Hi "
OUTPUT=" it's great to meet you!"

echo $GREETING $USER $OUTPUT
```

Save the script, but, before running it, notice that `$USER` is not defined in the script

Run the script and view the output:

```bash
[ntc@ntc ~]$ ./hello.sh 
Hi ntc it's great to meet you!
[ntc@ntc ~]$ 
```
Where did the value of `$USER` come from?

In your terminal execute the following command:

```bash
[ntc@ntc ~]$ echo $USER
ntc
[ntc@ntc ~]$
```

Since `$USER` is an environment variable, bash will evaluate it  automatically.  This can be extremely useful when building generic scripts.

> CHALLENGE: Incorporate other environment variables in your script. View them by executing `env`

### Task 2 - Conditionals

Conditionals provide an easy way to change a scripts behavior based on a certain value, assignment, or input.

The basic syntax for conditionals is `if ... then`.  Example:

```
if you are happy then smile
```

A real example:

```bash
if [ 1 == 1 ]; then
  echo "1 is 1"
fi
```

In bash the `if .. fi` indicates the beginning and end of an `if, then` condntional.  


##### Step 1 - Basic conditional

Building on our previous script, we will add a conditional:

```bash
#!/bin/bash

GREETING="Hi "
OUTPUT=" it's great to meet you!"

if [ $mood == "happy" ]; then
    echo $GREETING $USER $OUTPUT
fi
```

```bash
[ntc@ntc ~]$ ./hello.sh 
[ntc@ntc ~]$ 
```

Notice the lack of output.  In order for this to work, we need to set an environmental variable `$mood`.  Run the following command from the terminal: `export mood=happy`

That will set a `$mood` variable for the duration of your terminal session.

Now, let's re-run our script:

```bash
[ntc@ntc ~]$ ./hello.sh 
Hi ntc it's great to meet you!
[ntc@ntc ~]$ 
```

##### Step 2 - Advanced Conditionals

The above wasn't very useful, lets add some more logic.  We are going to add an `else` to the script.

```bash
#!/bin/bash

GREETING="Hi "
OUTPUT=" it's great to meet you!"
REPLY=" I hope you feel better"

if [ "$mood" == "happy" ]; then
    echo $GREETING $USER $OUTPUT
else
    echo $USER $REPLY
fi 
```

> It is often very useful to 'talk' through the logic in a conditional

This adds another element to our script.  Let's change the mood variable's value and re-run it:

```bash
[ntc@ntc ~]$ export mood=unhappy
[ntc@ntc ~]$ ./hello.sh 
ntc I hope you feel better
[ntc@ntc ~]$ 
```

Let's change mood to its original value:

```bash
[ntc@ntc ~]$ export mood=happy
[ntc@ntc ~]$ ./hello.sh 
Hi ntc it's great to meet you!
[ntc@ntc ~]$
```

> CHALLENGE:  Add More `else` conditionals to account for different moods: sad, unhappy, and jolly.  Also, change the REPLY depending on the mood

### Task 3 - Backup Script

This script will be used for the advanced module.  It will combine all of the things we have discussed and expound on them

##### Step 1 - Backup Script

Navigate to the `~/configs`  directory and have a look around.  

Create a `backup.sh` script that contains the following:

```bash
#!/bin/bash

# Backup Script
# Copies files that end with .cfg to the backup directory

BACKUP_DIR="/home/${USER}/backups"
CONFIG_DIR="/home/${USER}/configs"

# Check to see if Backup directory exists
# If not, create it

if [ ! -d $BACKUP_DIR ]; then
        echo " >>> CREATING $BACKUP_DIR <<< "
        mkdir -p $BACKUP_DIR
        echo " >>> BACKUP DIRECTORY CREATED <<< "
    else
      echo " >>> DIRECTORY $BACKUP_DIR ALREADY EXISTS <<< "
    fi

# Backup files from config directory to backup directory
echo " >>> BACKING UP FILES <<< "

# Search $CONFIG_DIR for files that end with *.cfg then
# Copy those to the $BACKUP_DIR

find $CONFIG_DIR -name "*.cfg" -print0 | xargs -0 -I{} cp -p {} $BACKUP_DIR

echo " >>> FILES BACKED UP <<< "

echo " ####### ALL DONE ####### "
```