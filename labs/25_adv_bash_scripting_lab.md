## Scripting Lab

In this lab we will build upon our scripting capabilities from the previous module.

### Task 1 - `backup.sh`

#### Step 1 - Backup Script

To begin, lets review our previous module's backup script:

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

This script does the following:
- check for a `backup` directory
- create a `~/backup` directory if it does not exist
- search the `~/configs` directory for files ending in `.cfg`
- copy those files to the `backup` directory

We will need to update this script to backup & remove files from 'yesterday' in the `~/configs` directory.

##### Step 2 - Edit files

Lets setup our configuration directory.  Navigate to the ~/.configs` directory, and execute the following two commands

```bash
[ntc@ntc configs]$ find . -regex ".*\(1\|3\|5\).cfg" -print0 | xargs -0 -I {} touch -d "yesterday" {}
[ntc@ntc configs]$ find . -regex ".*\(2\|4\).cfg" -print0 | xargs -0 -I {} touch -d "today" {}
```

> Based on our previous module, can you guess what these two commands are doing?

The two commands are editing the modified date for the files in the configs directory  

- Odd files `[1,3,5]` ending in `.cfg` are found via regex, and then that filename is `piped` to the `touch` command.  
- Even files `[2,4]` ending in `.cfg` are found via regex, and their filename is `piped` to the `touch` command.
- `touch` changes the even files' date to "today" & the odd files' date to "yesterday"

Check this by running `ls -lthr` in the `~/.configs` directory

This will help stage changes to our script

### Task 2 - Additional Backup

##### Step 1 - Edit `backup.sh`

Let's imagine that we are now being asked to copy our backups from our server, to a remote file store.  This is a common request, as long-term storage is seldom done on the device itself.  You may encounter this dealing with system logs, configuration files, and databases.  This is commonly called 'archiving'.  

Instead of copying files one-by-one, it is often a good practice to create a compressed copy of the files in question, and then archive them.  Let's configure our script to do this for us.

##### Step 2 - zip

[Zip](https://linux.die.net/man/1/zip) is a compression & file packaging utility that works w/Linux, Windows, Mac.  To install, run `yum install -y zip`

Let's edit our script to include archiving files via zip.

> Make sure to run the commands in Task 1, Step 2 first

```
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

echo " >>> ARCHIVING TODAYS FILES <<< "

find $BACKUP_DIR -type f -mtime +0 -print0 | xargs -0 -I {} zip -j configs.zip {}

echo " ####### ALL DONE ####### "
```

Now run `./backup.sh` 

```
[ntc@ntc ~]$ ./backup.sh 
 >>> DIRECTORY /home/ntc/backups ALREADY EXISTS <<< 
 >>> BACKING UP FILES <<< 
 >>> FILES BACKED UP <<< 
 >>> ARCHIVING TODAYS FILES <<< 
  adding: atl-rtr01.cfg (deflated 49%)
  adding: atl-rtr03.cfg (deflated 49%)
  adding: atl-rtr05.cfg (deflated 49%)
  adding: nyc-rtr01.cfg (deflated 48%)
  adding: nyc-rtr03.cfg (deflated 49%)
  adding: nyc-rtr05.cfg (deflated 49%)
 ####### ALL DONE ####### 
```   

Let's examine the command we added:

- `find $BACKUP_DIR -type f` - Find a files (not directories) in the `./backups` directory
- `mtime +0` - Find files with last modified date older than today
- `print0` - Print the names as they are found
- `xargs -0 -I {}` - Take input from the `find` command, ignore spaces & special characters and assign it an arbitrary value of `{}`
- `zip -j configs.zip {}` - Compress the files found from the `find` results and ignore (`-j`) the directory structure

After running, verify there is a `configs.zip` in the current directory