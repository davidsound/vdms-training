## Scripting Lab

In this lab we will build upon the `backup.sh` script created in the previous module.

### Task 1 - `backup.sh`

#### Step 1 - Backup Script

To begin, lets review our previous module's backup script:

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

This script checks if the needed directories exist and returna basic `Yes or No` answer.

Make the following changes to the script.

* Restructure the script to make the `backups` and `configs` directories a variable in the file.
* Check to see if the `backups` directory exists.  If it does **not** exist, create the directory.
* Copy any files ending in `.cfg` in the `configs` directory and copy it to the `backups` directory.

The full script is below.

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

##### Step 2

Now run the new script, observe the output, and verify the files ahve been copied to the new `backups` directory.

```bash
[ntc@ntc ~]$ ./backup.sh 
 >>> CREATING /home/ntc/backups <<< 
 >>> BACKUP DIRECTORY CREATED <<< 
 >>> BACKING UP FILES <<< 
 >>> FILES BACKED UP <<< 
 ####### ALL DONE ####### 
[ntc@ntc ~]$ ls
backup_dir  backups  backup.sh  configs  Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos  vlan.json  vlan.yml  web  whitespace.data
[ntc@ntc ~]$ cd backups/
[ntc@ntc backups]$ ls -ltr
total 76
-rw-rw-r--. 1 ntc ntc 4181 Feb 11 19:26 nyc-rtr02.cfg
-rw-rw-r--. 1 ntc ntc 4181 Feb 11 19:26 nyc-rtr04.cfg
-rw-rw-r--. 1 ntc ntc 4181 Feb 11 19:26 atl-rtr02.cfg
-rw-rw-r--. 1 ntc ntc 4181 Feb 11 19:26 atl-rtr04.cfg
-rw-rw-r--. 1 ntc ntc 4181 Feb 17 19:25 nyc-rtr03.cfg
-rw-rw-r--. 1 ntc ntc 4181 Feb 17 19:25 nyc-rtr05.cfg
-rw-rw-r--. 1 ntc ntc 4181 Feb 17 19:25 atl-rtr01.cfg
-rw-rw-r--. 1 ntc ntc 4181 Feb 17 19:25 atl-rtr03.cfg
-rw-rw-r--. 1 ntc ntc 4181 Feb 17 19:25 atl-rtr05.cfg
-rw-rw-r--. 1 ntc ntc 4059 Feb 17 19:25 nyc-rtr01.cfg
```

**Notes about script changes.**

* The line `[ ! -d $BACKUP_DIR ]` looks different than the conditional we used previously.  By adding the `!` we have changed the conditional to read `if the backup directory does not exist`.  Therefore, if the directory already exists it will go straight to the `else` statement.
* The line `find $CONFIG_DIR -name "*.cfg" -print0 | xargs -0 -I{} cp -p {} $BACKUP_DIR` adds some new elements to the `find` command that was previously covered.  In this command the script is searching the `configs` directory for all files that end in `.cfg`.  Then, for each file it finds, it passes it to the `cp` or `copy` command.  The `cp` copies the file to the `backups` directory.  The `-print0` is like `/dev/null`.  It takes the `stdout` from the `find` command and suppresses.



##### Step 3


Edit the script to include archiving files via zip.

> Make sure to run the commands in Task 1, Step 2 first

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

find $CONFIG_DIR -name "*.cfg" -print0 | xargs -0 -I {} cp -p {} $BACKUP_DIR

echo " >>> FILES BACKED UP <<< "

echo " >>> ARCHIVING FILES OLDER THAN 1 WEEK <<< "

find $BACKUP_DIR -type f -mtime +1w -print0 | xargs -0 -I {} zip -j configs.zip {}

echo " ####### ALL DONE ####### "
```

Now run `./backup.sh` 

```bash
[ntc@ntc ~]$ ./backup.sh  
 >>> DIRECTORY /home/ntc/backups ALREADY EXISTS <<< 
 >>> BACKING UP FILES <<< 
 >>> FILES BACKED UP <<< 
 >>> ARCHIVING FILES OLDER THAN 1 WEEK <<< 
  adding: nyc-rtr02.cfg (deflated 49%)
  adding: nyc-rtr04.cfg (deflated 49%)
  adding: atl-rtr02.cfg (deflated 49%)
  adding: atl-rtr04.cfg (deflated 49%)
 ####### ALL DONE ####### 
[ntc@ntc ~]$
```   

Let's examine the command we added:

- `find $BACKUP_DIR -type f` - Find a files (not directories) in the `./backups` directory
- `mtime +0` - Find files with last modified date older than today
- `print0` - Print the names as they are found
- `xargs -0 -I {}` - Take input from the `find` command, ignore spaces & special characters and assign it an arbitrary value of `{}`
- `zip -j configs.zip {}` - Compress the files found from the `find` results and ignore (`-j`) the directory structure

After running, verify there is a `configs.zip` in the current directory

```bash
[ntc@ntc ~]$ ls -ltr
total 28
-rw-rw-r--. 1 ntc ntc  598 Feb  8 11:18 vlan.json
-rw-rw-r--. 1 ntc ntc 2342 Feb  8 11:20 vlan.yml
drwxr-xr-x. 2 ntc ntc    6 Feb  9 10:03 Desktop
drwxr-xr-x. 2 ntc ntc    6 Feb  9 10:03 Templates
drwxr-xr-x. 2 ntc ntc    6 Feb  9 10:03 Public
drwxr-xr-x. 2 ntc ntc    6 Feb  9 10:03 Downloads
drwxr-xr-x. 2 ntc ntc    6 Feb  9 10:03 Documents
drwxr-xr-x. 2 ntc ntc    6 Feb  9 10:03 Music
drwxr-xr-x. 2 ntc ntc    6 Feb  9 10:03 Videos
drwxr-xr-x. 2 ntc ntc    6 Feb  9 10:03 Pictures
-rw-r--r--. 1 ntc ntc  308 Feb 11 20:37 whitespace.data
drwxrwxr-x. 2 ntc ntc  216 Feb 12 09:26 configs
drwxrwxr-x. 4 ntc ntc   37 Feb 18 01:04 backup_dir
drwxrwxr-x. 2 ntc ntc   41 Feb 18 15:55 web
drwxrwxr-x. 2 ntc ntc  216 Feb 18 19:40 backups
-rwxrwxr-x. 1 ntc ntc  885 Feb 18 19:49 backup.sh
-rw-rw-r--. 1 ntc ntc 9202 Feb 18 19:49 configs.zip
[ntc@ntc ~]$ 
```
