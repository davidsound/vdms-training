## Lab 2 - Navigating the *nix file system

This lab will help you build familiarity with the *nix file system and master the key commands to become proficient.

> **NOTE: At any point, typing the command `clear` or pressing the key combination "CTRL-l" will clear the screen.**
> **NOTE: Using the `UP` and `DOWN` arrow allows you to cycle through the previous commands typed.**



### Task 1 - Directory listing

##### Step 1

Log into your jump host over SSH. You will land at the following prompt:

```
[ntc@ntc ~]$
```

This is your `home` directory. 

##### Step 2 

Type in the command `pwd`. This command prints the "present working directory" to the screen:

```
[ntc@ntc ~]$ pwd
/home/ntc
[ntc@ntc ~]$

```



##### Step 3

`ls` is the command used to "list" directories and files in *nix. Go ahead and issue the ls command.


```
[ntc@ntc ~]$ ls
configs  VirtualBoxVMs
[ntc@ntc ~]$ 
```


##### Step 4

The output generated does not tell us whether the listed output are files or directories. Most *nix commands support `flags` that alter the way the main command works. Flags are indicated by using a `-` followed by a letter or combination of letters. Alternatively flags can also be indicated using `--` followed by a word. Go ahead and issue the `ls -l` command. This will provide you with a "long" or detailed listing of the directory.

```
[ntc@ntc ~]$ ls -l
total 0
drwxrwxr-x. 3 ntc ntc 51 Jan 22 19:56 configs
drwxrwxr-x. 3 ntc ntc 45 Jan 23 17:02 VirtualBoxVMs
[ntc@ntc ~]$ 

```


In the above output, the first field indicates the type of the file listed.



##### Step 5

In *nix, the `man` command is used to invoke the "manual" on any given command. Try this with the `ls` command:

```
[ntc@ntc ~]$ man ls

```

Do all flags have word equivalents ?

>Note: To exit the manual, type `q`

**BONUS: Invoke the man page for the `pwd` command**



##### Step 6

Look at the explanations for the following flags `l`, `a`, `t`, `r` using `man`. Use these flag combinations to do a directory listing.

```
[ntc@ntc ~]$ ls -l
total 0
drwxrwxr-x. 3 ntc ntc 51 Jan 22 19:56 configs
drwxrwxr-x. 3 ntc ntc 45 Jan 23 17:02 VirtualBoxVMs
[ntc@ntc ~]$ man ls
[ntc@ntc ~]$ man ls
[ntc@ntc ~]$ ls -lt
total 0
drwxrwxr-x. 3 ntc ntc 45 Jan 23 17:02 VirtualBoxVMs
drwxrwxr-x. 3 ntc ntc 51 Jan 22 19:56 configs
[ntc@ntc ~]$ 
[ntc@ntc ~]$ ls -ltr
total 0
drwxrwxr-x. 3 ntc ntc 51 Jan 22 19:56 configs
drwxrwxr-x. 3 ntc ntc 45 Jan 23 17:02 VirtualBoxVMs
[ntc@ntc ~]$ ls -a
.  ..  .bash_history  .bash_logout  .bash_profile  .bashrc  .config  configs  .gitconfig  .lesshst  .vagrant.d  VirtualBoxVMs
[ntc@ntc ~]$ 
[ntc@ntc ~]$ ls -la
total 24
drwx------. 6 ntc  ntc   186 Jan 24 17:21 .
drwxr-xr-x. 4 root root   32 Dec 21 16:37 ..
-rw-------. 1 ntc  ntc  2094 Jan 24 13:33 .bash_history
-rw-r--r--. 1 ntc  ntc    18 Dec  6  2016 .bash_logout
-rw-r--r--. 1 ntc  ntc   193 Dec  6  2016 .bash_profile
-rw-r--r--. 1 ntc  ntc   231 Dec  6  2016 .bashrc
drwx------. 3 ntc  ntc    24 Jan 23 16:59 .config
drwxrwxr-x. 3 ntc  ntc    51 Jan 22 19:56 configs
-rw-rw-r--. 1 ntc  ntc    40 Jan 22 19:48 .gitconfig
-rw-------. 1 ntc  ntc    41 Jan 22 19:56 .lesshst
drwxrwxr-x. 7 ntc  ntc   119 Jan 23 17:00 .vagrant.d
drwxrwxr-x. 3 ntc  ntc    45 Jan 23 17:02 VirtualBoxVMs
[ntc@ntc ~]$ 
[ntc@ntc ~]$ ls -latr
total 24
-rw-r--r--. 1 ntc  ntc   231 Dec  6  2016 .bashrc
-rw-r--r--. 1 ntc  ntc   193 Dec  6  2016 .bash_profile
-rw-r--r--. 1 ntc  ntc    18 Dec  6  2016 .bash_logout
drwxr-xr-x. 4 root root   32 Dec 21 16:37 ..
-rw-rw-r--. 1 ntc  ntc    40 Jan 22 19:48 .gitconfig
drwxrwxr-x. 3 ntc  ntc    51 Jan 22 19:56 configs
-rw-------. 1 ntc  ntc    41 Jan 22 19:56 .lesshst
drwx------. 3 ntc  ntc    24 Jan 23 16:59 .config
drwxrwxr-x. 7 ntc  ntc   119 Jan 23 17:00 .vagrant.d
drwxrwxr-x. 3 ntc  ntc    45 Jan 23 17:02 VirtualBoxVMs
-rw-------. 1 ntc  ntc  2094 Jan 24 13:33 .bash_history
drwx------. 6 ntc  ntc   186 Jan 24 17:21 .
[ntc@ntc ~]$ 

```

> **Note: The flag combinations `ltr` and `latr` are often used since they output the long list of all files within the directory, listing the oldest file/dir at the top and the newest one at the bottom.**








##### Step 7

Use the `ls` command to list the contents of a specific directory. You can still use the same flags to execute this:

```
[ntc@ntc ~]$ ls -ltr /etc/default/
total 12
-rw-r--r--. 1 root root  119 Nov  4  2016 useradd
-rw-r--r--. 1 root root 1756 Nov 30 18:21 nss
-rw-r--r--. 1 root root  372 Jan 23 16:45 grub
[ntc@ntc ~]$ 
```



### Task 2 - Directory Navigation

##### Step 1 

Use the `mkdir` command to create a directory called `dc_sites` within you home directory.

```
[ntc@ntc ~]$ mkdir dc_sites

```

```
[ntc@ntc ~]$ ls -ltr
total 0
drwxrwxr-x. 3 ntc ntc 51 Jan 22 19:56 configs
drwxrwxr-x. 3 ntc ntc 45 Jan 23 17:02 VirtualBoxVMs
drwxrwxr-x. 2 ntc ntc  6 Jan 24 17:53 dc_sites
[ntc@ntc ~]$ 

```



##### Step 2

Similar to `ls` the `mkdir` command supports many flags. A commonly used way to create a nested directory structure is by using the `mkdir -p` flag. Use the `man` page of mkdir to understand this option. Then go ahead and create the following directory structure:

```
dc_sites
└── US
    └── ATL
        └── alpha-dc

```



Solution:

```
[ntc@ntc ~]$ mkdir -p dc_sites/US/ATL/alpha_dc

```


##### Step 3

The command used to navigate to a particular directory is `cd` - "change directory". Use this command to navigate to the `dc_sites` directory. Use the `pwd` command to validate.


```
[ntc@ntc ~]$ cd dc_sites
[ntc@ntc dc_sites]$ pwd
/home/ntc/dc_sites
[ntc@ntc dc_sites]$ 

```

> Note: Pay attention to how the prompt has changed, reflecting your current working directory.



##### Step 4

*nix uses "relative paths". `.` denotes the current working directory. and `..` denotes the parent directory. Navigate back to the ntc user home directory using relative path:


```
[ntc@ntc dc_sites]$ cd ../
[ntc@ntc ~]$ pwd
/home/ntc
[ntc@ntc ~]$ 
```

> Note: You can do this for any number of parent levels, all the way to the "root" or `/` 



##### Step 5

`cd` to the `alpha_dc` subdirectory under `dc_sites`

```
[ntc@ntc ~]$ cd dc_sites/US/ATL/alpha_dc/
[ntc@ntc alpha_dc]$ pwd
/home/ntc/dc_sites/US/ATL/alpha_dc
[ntc@ntc alpha_dc]$ 

```

> Note: the `TAB` key can be used to autocomplete commands in the "bash shell".


##### Step 6

Use relative paths to navigate back to `/home`

```
[ntc@ntc alpha_dc]$ cd ../../../../../../home
[ntc@ntc home]$ pwd
/home

```

> Note: The home directory has special significance. It is where the "home" directories of each user on the system resides.


> Execute a `ls -l` command here to see what users are configured on your jump hosts.


##### Step 7

In the bash shell, using the `cd` command with a `-` will place you in the previous directory you were in.

```
[ntc@ntc home]$ cd -
/home/ntc/dc_sites/US/ATL/alpha_dc
[ntc@ntc alpha_dc]$ pwd
/home/ntc/dc_sites/US/ATL/alpha_dc
[ntc@ntc alpha_dc]$ 

```


##### Step 8

Simply typing the `cd` command, without specifying any path, will result in landing the user in her home directory. Go ahead and try this out.


```
[ntc@ntc alpha_dc]$ pwd
/home/ntc/dc_sites/US/ATL/alpha_dc
[ntc@ntc alpha_dc]$ 

```

```

[ntc@ntc alpha_dc]$ cd
[ntc@ntc ~]$ 

```

```

[ntc@ntc ~]$ pwd
/home/ntc
[ntc@ntc ~]$ 
```

##### Step 9

A thrid-party utility called `tree` is pre-installed on your jumphosts. This is a handy tool that helps visualize the directory structure from the command line. Use `tree` to visualize the directory structure of the `dc_sites` directory.

```
[ntc@ntc ~]$ tree dc_sites
dc_sites
└── US
    └── ATL
        └── alpha_dc

3 directories, 0 files
[ntc@ntc ~]$ 

```


##### Step 10

Create a new directory path for the EMEA datacenter site within `dc_sites`. Create the directory structure that looks as follows:

```
dc_sites/
├── EMEA
│   └── LON
│       └── beta_dc
└── US
    └── ATL
        └── alpha_dc

```



##### Step 11

Use the `rmdir` command to remove the `beta_dc` directory

```
[ntc@ntc dc_sites]$rmdir EMEA/LON/beta_dc
[ntc@ntc dc_sites]$tree
.
├── EMEA
│   └── LON
└── US
    └── ATL
        └── alpha_dc

5 directories, 0 files

```


##### Step 12

What happens when you try to delete the `EMEA` directory?

```
[ntc@ntc dc_sites]$rmdir EMEA
rmdir: failed to remove ‘EMEA’: Directory not empty
[ntc@ntc dc_sites]$

```

By default only empty directories can be removed. 


##### Step 13

To remove a directory structure recursively, use the `rmdir -p` command. Use this command to remove the `EMEA` and `LON` directories

```
[ntc@ntc dc_sites]$rmdir -p EMEA/LON/
[ntc@ntc dc_sites]$tree
.
└── US
    └── ATL
        └── alpha_dc

3 directories, 0 files
[ntc@ntc dc_sites]$

```

