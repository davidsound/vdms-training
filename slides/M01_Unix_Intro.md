layout: true

.footer-picture[![Network to Code Logo](slides/media/Footer2.PNG)]
.footnote-left[(C) 2015 Network to Code, LLC. All Rights Reserved. ]
.footnote-con[CONFIDENTIAL]

---

class: center, middle, title
.footer-picture[<img src="slides/media/Footer1.PNG" alt="Blue Logo" style="alight:middle;width:350px;height:60px;">]

# Introduction to *nix


---

# Module Overview

- The *nix architecture
- Getting around
- Command line editors - nano
- Command line editors - vim
- Using Cyberduck
- Sublime text

---


class: ubuntu

# The *nix Architecture

.left-column[
<img src="slides/media/unix/OS_arch.png" alt="Pep8" style="align: left:;width:600px;height:500px;">
]


.right-column[
- OS is a resource manager
- Each layer provides an abstraction
- Confining scope and protection
]

---

# A brief history of Linux



    Hello everybody out there using minix –

    I’m doing a (free) operating system (just a hobby, won’t be big and
    professional like gnu) for 386(486) AT clones. This has been brewing
    since april, and is starting to get ready. I’d like any feedback on
    things people like/dislike in minix, as my OS resembles it somewhat
    (same physical layout of the file-system (due to practical reasons)
    among other things).

    I’ve currently ported bash(1.08) and gcc(1.40), and things seem to work.
    This implies that I’ll get something practical within a few months, and
    I’d like to know what features most people would want. Any suggestions
    are welcome, but I won’t promise I’ll implement them 🙂

    Linus (torvalds@kruuna.helsinki.fi)

    PS. Yes – it’s free of any minix code, and it has a multi-threaded fs.
    It is NOT protable (uses 386 task switching etc), and it probably never
    will support anything other than AT-harddisks, as that’s all I have :-(.
    

---

# A brief history of Linux(Contd.)


- Built in 1991 as a personal project
- Aims to be POSIX compliant
- Uses best of SysV and BSD 



---


# Distributions

Distributions target usage type/end user:

- **Servers** : RedHat Enterprise, Oracle Linux, Ubuntu Server, CentOS, Debian
- **Desktop**: Mint, ArchLinux, Fedora, Ubuntu Desktop 
- **Containers**: Alpine, CoreOS, RancherOS
- **Security**: Kali, BackTrack, TAILS, Qubes


---

class: ubuntu

# Which Shell?


There are a number of different shells for Unix. People can become very attached to the shell they prefer. Popular shells include

```
sh - the bourne shell
bash - the bourne again shell
csh - the c shell
ksh - the Korn shell (strangely, not named for the band)
zsh - the z shell

```


In this course, we will be working on the `bash` shell

---

class: middle, segue

# Getting around in the Linux OS
### Introduction to Linux for Network Engineers

---
class: ubuntu

# Directory Listing


Because memory and CPU power were at a premium in the past, UNICS (eventually shortened to UNIX) used short commands to minimize the space needed to store them and the time needed to decode them - hence the tradition of short UNIX commands we use today, e.g. ls, cp, rm, mv etc.


- List contents of the directory:

```
[ntc@ntc ~]$ ls
configs  VirtualBoxVMs
[ntc@ntc ~]$ 

```


- The Present Working Directory `pwd`

```
[ntc@ntc ~]$ pwd
/home/ntc
[ntc@ntc ~]$
```

---


class: ubuntu

# Using flags

- Many *nix commands support the use of `flags` that extend behavior


```
# Long  Listing

[ntc@ntc ~]$ ls -l
total 0
drwxrwxr-x. 3 ntc ntc 51 Jan 22 19:56 configs
drwxrwxr-x. 3 ntc ntc 45 Jan 23 17:02 VirtualBoxVMs
[ntc@ntc ~]$ 
```

```
# Long listing including hidden files, latest files last.

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

---

class: ubuntu 

# man pages 

- In-built help is provided by invoking the `man` or manual command

```
LS(1)                                                            User Commands                                                           LS(1)

NAME
       ls - list directory contents

SYNOPSIS
       ls [OPTION]... [FILE]...

DESCRIPTION
       List  information  about  the FILEs (the current directory by default).  Sort entries alphabetically if none of -cftuvSUX nor --sort is
       specified.

.
.
.
.
.
<output truncated for readability>
```
---

class: ubuntu
# The Linux directory layout

*/bin*: Binaries for system operations

*/sbin*: Binaries that requires "Superuser" privileges

*/lib*: System libraries

*/dev*: Hardware devices

*/opt*: Optional, 3rd Party applications

*/home*: User home directories


---

class: ubuntu
# The Linux directory layout(Contd.)


*/tmp*: Temporary files - Wiped at every reboot

*/etc*: System Wide configurations

*/boot*: Kernel

*/proc*: Runtime process information

*/usr*: User binaries - most apps get installed here

*/var*: Variable files (Files that change during system operation)





---

class: ubuntu 

# Directory Creation/Deletion


- `mkdir` to create a directory. Use the `-p` option to create subdirectories
- `rmdir` or `rm -fr` will remove directories



```
[ntc@ntc ~]$ mkdir dc_sites

[ntc@ntc ~]$ ls -ltr
total 0
drwxrwxr-x. 3 ntc ntc 51 Jan 22 19:56 configs
drwxrwxr-x. 3 ntc ntc 45 Jan 23 17:02 VirtualBoxVMs
drwxrwxr-x. 2 ntc ntc  6 Jan 24 17:53 dc_sites
[ntc@ntc ~]$ 


```


```
[ntc@ntc ~]$ mkdir -p dc_sites/EMEA/LON/beta_dc

```

---

class: ubuntu
# Removing directories

- Use the `rmdir` command to remove directories
- By default only empty directories can be removed
- To delete a directory structure including all directories in a path use the `-p` flag



```
[ntc@ntc dc_sites]$rmdir EMEA/LON/beta_dc
```
*Removes the `beta_dc` folder*


```
[ntc@ntc dc_sites]$rmdir -p EMEA/LON/
```
*Removes the EMEA and LON folders*

---

class: ubuntu 

# Relative Paths

- `.` in *nix denotes the current working directory
- `..` denotes the parent directory
- `/` is used to indicate the path relative to `.`


```
[ntc@ntc level-3]$ cd ../../../../../../home
[ntc@ntc home]$ pwd
/home
```
---

# Before the first lab

- The `bash` shell retains command history. This can be accessed by pressing the `UP` and `DOWN` arrow buttons
- Typing `clear` will clear the screen. 

---

# Lab Time

- Lab 1 - Accessing the lab
- Lab 2 - File System Navigation

**Accessing Lab Guide**
Use the `LAB_GUIDE.md` file in the root of the GitHub repository.

---

class: middle, segue

# Viewing file contents, redirection and command chaining
### Introduction to Linux for Network Engineers

---


class: ubuntu

# The `cat` command

- Stands for "concatenate" files and print to standard output
- Used typically to display file contents or stream them to another command


```
[ntc@ntc ~]$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
.
.
.
.
.
<output truncated for readability>

```


---

class: ubuntu

# `more` and `less`


- Commands to allow the user to paginate the contents of a lengthy file
- The `less` command allows the user to view the output forwards and backwards

```
[ntc@ntc ~]$ less /etc/services

```

```

[ntc@ntc ~]$ more /etc/services 
```

---

class: ubuntu
 
# The `file` command

- Used to identify the type of file (binary, text etc)

```
[ntc@ntc ~]$ file dc_sites
dc_sites: directory
[ntc@ntc ~]$ 

```

```

[ntc@ntc ~]$ file /etc/services 
/etc/services: C source, ASCII text
[ntc@ntc ~]$ 


```

---




class: ubuntu

# Redirecting output


- `>`, `>>` & `<`  are used to redirect **standard output**
- `2>` is used to redirect **standard error**

```
[ntc@ntc ~]$ echo "OSPF STANDARDS" > ospf_config_guide.txt
[ntc@ntc ~]$ 

```
>Note: Observe that the output is not sent to the screen. It is instead, redirected to the file called ospf_config_guide.txt

```

[ntc@ntc ~]$ cat ospf_config_guide.txt 
OSPF STANDARDS
[ntc@ntc ~]$ 


```


---
class: ubuntu

# Redirecting output(Contd.)

.left-column[
Redirect output and errors into separate files

```

[ntc@ntc ~]$ cat test_redirection/directory_listing.txt > output 2>error
[ntc@ntc ~]$ 
[ntc@ntc ~]$ cat error
[ntc@ntc ~]$ 
[ntc@ntc ~]$ cat output
total 4
drwxrwxr-x. 3 ntc ntc 51 Jan 22 19:56 configs
drwxrwxr-x. 3 ntc ntc 45 Jan 23 17:02 VirtualBoxVMs
drwxrwxr-x. 3 ntc ntc 50 Jan 25 15:44 dc_sites
-rw-rw-r--. 1 ntc ntc 16 Jan 25 15:48 greetings.txt
[ntc@ntc ~]$ 
[ntc@ntc ~]$ 
```

]

.right-column[
Redirect output and errors into the same file:

```
[ntc@ntc ~]$ cat test_redirection test_redirection/directory_listing.txt >output  2>&1 
[ntc@ntc ~]$ 

[ntc@ntc ~]$ cat output 
cat: tux: Is a directory
total 4
drwxrwxr-x. 3 ntc ntc 51 Jan 22 19:56 configs
drwxrwxr-x. 3 ntc ntc 45 Jan 23 17:02 VirtualBoxVMs
drwxrwxr-x. 3 ntc ntc 50 Jan 25 15:44 dc_sites
-rw-rw-r--. 1 ntc ntc 16 Jan 25 15:48 greetings.txt
[ntc@ntc ~]$ 


```
]

---

class: ubuntu

# Using "pipes" to chain commands

- "Pipe" denoted by the `|` symbol is used to send the output of one command as an input to another

```
[ntc@ntc ~]$ ls -l /usr/bin/ | more
total 72192
```


```
[ntc@ntc ~]$ ls -l dc_sites/ | wc -l > file_count.txt
[ntc@ntc ~]$ 
```

---

class: ubuntu

# Copying, moving and deleting files

- `cp` command is used to copy a file. 
  * Using this command with the `-R` flag allows recursive copy - copying a directory structure.
- `mv` command is used to rename a directory or file. 
- `rm` command is used to remove a file.
  * Used along with the `r` flag removes recursively.
  * Used along with the `f` flag removes forcibly.

```
[ntc@ntc ~]$ cp nyc-rtr01.cfg nyc-rtr01_backup.cfg
[ntc@ntc ~]$
```

```
[ntc@ntc ~]$ cp -r dc_sites dc_sites-backup
[ntc@ntc ~]$
```

```
[ntc@ntc ~]$ mv nyc-rtr01_backup.cfg nyc-rtr01.cfg

```

```
[ntc@ntc ~]$ rm -r dc_sites-backup
```

---

# Lab Time

- Lab 3 - Viewing files, redirection and chaining
- Lab 4 - Copying, moving and deleting 

---


# Editing text


<img src="slides/media/real_programmers.png" alt="Pep8" style="align: left:;width:600px;height:500px;">


---
class: ubuntu

# Editing text(Contd.)

- The `cat` command can be used with redirection to create or update files.
- Close out files using the `CTRL-d` key combination.

```
[ntc@ntc ~]$ cat > change_notes.txt
Interfaces to add to NYC-RTR01:
==============================
1. Loopback 1  
2. Tunnel 101
3. Portchannel 10
[ntc@ntc ~]$
```

```
[ntc@ntc ~]$ cat >> change_notes.txt
4. Loopback 2
[ntc@ntc ~]$
```

---
class: ubuntu

# Nano


- Simple command line text editor 
- Not installed by default
- No options for advanced editing

```
[ntc@ntc ~]$ nano change_notes.txt
```


---
# Nano(Contd.)

<img src="slides/media/nano/nano1.png" alt="Pep8" style="align:middle:;width:800px;height:500px;">

---

class: ubuntu

# Nano(Contd.)


Important shortcuts:

```
CTRL-n : Next line
CTRL-a : Beginning of the line
CTRL-g : Invoke help
ALT-r  : Search and replace
CTRL-o : Save
CTRL-x : Exit
```

---


# Lab Time

- Lab 5 - Using `cat` and `nano` to edit files


---
class: ubuntu

# `vim` or the "VI Improved" Editor

- `vim` is the upgraded version of the default `vi` editor
- Very powerful and feature rich
- Extensible behavior using popular/well-known plugins
- Context based (Edit mode, Visual mode, Normal mode etc)


Invoked at the command prompt as follows:

```
[ntc@ntc ~]$ vim configs/nyc-rtr01.cfg
```

---

# VIM(Contd.)

<img src="slides/media/vim/vim1.png" alt="Pep8" style="align: middle:;width:800px;height:500px;">

---

class: ubuntu
# Navigation

- By default opens in "normal mode"
- `h`, `j`, `k` and `l` keys are used to move the cursor left, up, down and right 
- Arrow keys will also work but discouraged.


In the normal mode:

```
w : moves the cursor forward one word at a time
b:  moves the cursor backward one word at a time
$:  moves the cursor to the end of the line
^:  moves the cursor to the first character of the line
SHIFT-g: moves the cursor to the bottom of the page
```


---

class: ubuntu

# Exiting vim

1. Type the `ESC` key. This will tell the editor that the context is being changed. 

2. Now type the `:` key. Now your cursor will be blinking at the bottom of the file.

3.  Type the `q` or `q!`(force) keys to exit the file. 


_`vim` has been subjected to ( an unfairly) number of jokes about leaving users stranded after opening a file._



![](slides/media/vim/exit_vim.png)








---
class: ubuntu

# Enabling line numbers

1. Type the `ESC` key. This will tell the editor that the context is being changed. 

2. Now type the `:` key. Now your cursor will be blinking at the bottom of the file.

3. Type `se nu`; short for "set number" to enable line numbers.


**To go to a particular line you can simply type the line number after steps 1 and 2**

---
# Enabling line numbers

<img src="slides/media/vim/vim2.png" alt="Pep8" style="align: middle:;width:800px;height:500px;">



---
class: ubuntu

# Editing text

- From normal mode, use the `i` key to enter the `INSERT` mode. In this mode you can edit text by simply typing the keys.

- To delete a character:
    1. Exit the `INSERT` mode by pressing the `ESC` key. 
    2. Navigate to the character 
    3. Use the `x` key to delete it
    
- To undo an edit, from the `normal` mode, type the `u` key. Similarly, to `redo` an edit, use the `CTRL-r` key combination

- To replace an entire word, navigate to the start of the word (or position from which you would like to make the change) and then type `cw` for "Change Word". This will switch the editor automatically to the `INSERT` mode


**From normal mode, type `ESC-:` and then `w` to save or "write" a file. If creating a new file use `w` followed by the new file's name.**

---

# INSERT mode
<img src="slides/media/vim/vim3.png" alt="Pep8" style="align: middle:;width:800px;height:500px;">

---

# Finding and replacing text

- From normal mode, use the `/` key to invoke a search followed by the text to find
- To look for repeated occurrences, type the `n` key for "next match"

To find and replace use the following steps:
1. From normal mode, type `ESC-:` followed by:
2.  `%s/` *[Word to replace]* `/` *[replacement word]* `/g`

> The `/g` implies "make the replacements globally for all matches in the file"

> To make interactive replacements use `/gc` instead. This option will prompt for confirmation for each replacement

---
class: ubuntu

# Opening multiple files in split screens

- Use `sp` for horizontal split or `vsp` for vertical

<img src="slides/media/vim/vim4.png" alt="Pep8" style="align: right:;width:800px;height:350px;">


---
class: ubuntu

# Basic operations with split screens


- Use `CTRL-ww` to jump between the split

- Using the `set scrollbind` setting on each window allows scrolling both windows simultaneously



---

# Copying, cutting and pasting

- To copy a line from normal mode, navigate to the line and type `yy` or "yank"
- To copy multiple lines, go to the starting line and then prepend `yy` with the number of lines to yank. For instance, `3yy` will yank or copy 3 lines.
- To cut, use the same methodology as copy. Except use the `dd` key combination rather than `yy`
- In either case, to paste the line copied, navigate to the desired location and then use the `p` key.

---

# Lab Time

- Lab 6 - Using vim for basic editing.

---


class: center, middle, segue

# Configuring vim and managing plugins 

---

class: ubuntu

# The `.vimrc` file

- Is executed soon as vim is started
- Stores user customizations and  placed in the `home` directory
- **Not automatically generated**
- The `.` implies "hidden" file
- The `rc` stands for "run commands"


> Portable and typically version controlled


Some typical customizations:

```
**Enable line numbers**: se nu
**Highlight line being edited**: set cursorline
**Set ruler**: set ruler
```

---

# .vimrc customizations

<img src="slides/media/vim/vim5.png" alt="Pep8" style="align: middle:;width:800px;height:500px;">


---
class: ubuntu

# Using plugins and plugin managers (vim-plug)

- A Vim plugin is a set of Vimscript files that are laid out in a certain directory structure. Before plugin managers became popular, Vim plugins were usually distributed as tarballs. Users would manually download the file and extract it in a single directory called `~/.vim`, and Vim would load the files under the directory during startup.

- Installing the `vim-plug` plugin manager is as simple as downloading it:


```
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```


---

class: ubuntu
# Curl - short introduction

Curl is a command line tool to transfer data to or from a server. Supports multiple protocols:

- HTTP/S
- FTP/S
- SCP
- SFTP
- TFTP etc.

Designed to run without user interaction

```
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

```
-f : Quitely ignore failures
-L : Follow links
-o : Output to a file

```

---

## Installing plugins with vim-plug - Step 1

The list of plugins to be installed are added to `.vimrc` within the `plug#begin()` and `plug#end()` calls

<img src="slides/media/vim/vim6.png" alt="Pep8" style="align: middle:;width:800px;height:450px;">

---

## Installing plugins with vim-plug - Step 2

Type `ESC-:source .vimrc` to invoke the `.vimrc` file. (This is equivalent to stopping vim and restarting it)

<img src="slides/media/vim/vim7.png" alt="Pep8" style="align: middle:;width:800px;height:450px;">


---
## Installing plugins with vim-plug - Step 3

From normal mode `InstallPlug` is invoked to install the declared plugins.

<img src="slides/media/vim/vim8.png" alt="Pep8" style="align: middle:;width:800px;height:450px;">


---


# Lab Time

- Lab 7 - Customizing vim and using vim plugins


---

class: center, middle, segue

# Visual mode editing, whitespaces


---

# TABs vs SPACEs

[Silicon_valley](https://www.youtube.com/watch?v=SsoOG6ZeyUI)


- Extremely important for "indentation sensitive" tools like `YAML` and `Python`
- Hard to tell visually whether whitespace is a `TAB` or a `SPACE`


---
class: ubuntu

# TABs vs SPACEs (Contd.)

In normal mode, `set list` . This is a boolean operator that turns-on or off whitespace identifiers. (Default off)


```
`:set list` displays whitespace, while `:set nolist` displays normally

```

- Optionally use `set listchars` to specify how to display TABs


- By default TABs are identified as `^I` characters.

---


# Visualizing TABS




.left-column[

Note line #4 and #16-20

<img src="slides/media/vim/vim9_LHS.png" alt="Pep8" style="align: middle:;width:500px;height:400px;">

]


.right-column[

Note line #4 and #16-20

<img src="slides/media/vim/vim9_RHS.png" alt="Pep8" style="align: middle:;width:500px;height:400px;">

]

---

# Visual mode editing

- Used to visually select a block or region of text within vim
- Operations can then be restricted to this block
- Invoked by typing :
  1. `v` to enter visual mode
  2. `CTRL-v` to enter visual "block" mode - for rectangular selections
  3. `SHIFT-v` to enter visual "line" mode - for entire line selection 


Typical use cases:
- Commenting/un-commenting out a block of code
- Indenting a region or block of text


---

# Commenting out a block of code

.left-column[

Use CTRL-v to start selection 

<img src="slides/media/vim/vim10_LHS.png" alt="Pep8" style="align: middle:;width:500px;height:400px;">

]


.right-column[

Use SHIFT-i to insert block 

<img src="slides/media/vim/vim10_RHS.png" alt="Pep8" style="align: middle:;width:500px;height:400px;">

]


---

# Lab Time

- Lab 8 - Working with whitespaces and using the vim-plug plugin manager

---


class: center, middle, segue

# CyberDuck and Sublime Text 

---

# CyberDuck

- Linux supports many GUI based editors. However oftentimes, especially when working with remote servers, the "Window Manager" is not installed. 

- CyberDuck is an Open Source, free software that can be used to manage remote files while still using a GUI editor locally.

- It relies on SFTP along with other protocols to edit files remotely



---

# CyberDuck(Contd.)

The 3 main functions provided by the tool are :

1. Download from remote host
2. Upload to remote host (upload on changes)
3. Delete on remote host

These options are made available through a command line tool called `duck`

---

class: ubuntu

# Download

class: ubuntu

```
[ntc@ntc ~]$ duck --verbose --download sftp://ntc@centos/home/ntc/configs/ ~/
```

![](slides/media/cyberduck/cyberduck1.png)



---
class: ubuntu

# Upload

```
[ntc@ntc ~]$ duck --verbose --existing compare --upload sftp://ntc@centos/home/ntc/configs/ ~/configs/
```

-  The `existing compare` option, instructs duck that if any of the files in our local configs directory exist in the remote configs directory they should be compared for changes. 
-  If the files have been changed locally, then upload the changed files. If no changes have been made, skip.



---
class: ubuntu

# Delete

```
[ntc@ntc ~]$ duck --verbose --delete sftp://ntc@centos/home/ntc/configs/test.file
```

This will delete the file on the remote host.


---
# Sublime Text

- Sublime Text is a very popular multi-platform editor. It comes with a lot of default features (like colorization, alignment, indentation etc.) that requires manual installation of plugins with an editor like `vim`
- Users are comfortable using the mouse to navigate and remembering all the keystrokes of a CLI based editor like `vim` might be a steep learning curve


*Sublime Text is not free*


---

class: ubuntu

# Some useful shortcuts

```
Shift+Ctrl+T : Open the last file being edited
Ctrl+G: Go To line number
Ctrl+S: Save the file
Ctrl-X:  Delete the entire line
```

---

# Lab Time

- Lab 9 - Using Cyberduck
- Lab 10 - Using SublimeText


---


class: center, middle, segue

# User Administration 

---

class: ubuntu

# Adding a user


```
[ntc@ntc ~]$ sudo useradd -m ntc-netops-user1
[ntc@ntc ~]$
```

*The -m flag will ensure that the home directory is created for the user under `/home`, using the user name as the directory name*

```
[ntc@ntc ~]$ sudo passwd ntc-netops-user1
Changing password for user ntc-netops-user1.
New password: 
BAD PASSWORD: The password fails the dictionary check - it is too simplistic/systematic
Retype new password: 
passwd: all authentication tokens updated successfully.
[ntc@ntc ~]$
```

---

# The `sudo` command

- Elevates privileges of a normal user to allow her to "do" "superuser" operations
- Use wisely!!

<img src="slides/media/sandwich.png" alt="Pep8" style="align: left:;width:600px;height:500px;">


---
class: ubuntu
# The `/etc/passwd` file

```
[ntc@ntc ~]$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
.
.
.
.

<output truncated for readability>
.
.
.
.
colord:x:995:990:User for colord:/var/lib/colord:/sbin/nologin
gdm:x:42:42::/var/lib/gdm:/sbin/nologin
testuser:x:1002:1002::/home/testuser:/bin/bash
ntc-netops-user1:x:1003:1003::/home/ntc-netops-user1:/bin/bash
[ntc@ntc ~]$
```

---

class: ubuntu

# Groups

- By default when a new user is added, a group by the same name is created and the user is added to that group

```
# Adding a new group

[ntc@ntc ~]$ sudo groupadd ntc-netops
[sudo] password for ntc: 
[ntc@ntc ~]$
```

The groups command can be used to check the current groups a user is member off:

```
[ntc@ntc ~]$ groups ntc-netops-user1
ntc-netops-user1 : ntc-netops-user1
[ntc@ntc ~]$
```

---
class: ubuntu

# Groups(Contd.)

```
[ntc@ntc ~]$ cat /etc/group
root:x:0:
bin:x:1:
daemon:x:2:
sys:x:3:
adm:x:4:
tty:x:5:
disk:x:6:
lp:x:7:
mem:x:8:
kmem:x:9:

```
---
class: ubuntu

# The usermod command

The `usermod` command can be used to update group memberships of an existing user:

```
[ntc@ntc ~]$ sudo usermod -a -G ntc-netops ntc-netops-user1 
[ntc@ntc ~]$
```
---
class: ubuntu

# Adding a privileged user

- Done through editing the `/etc/sudoers` file 
- Best practice - update `/etc/sudoers.d` directory
- Use `visudo`
---
class: ubuntu

# File permissions

Every file in Unix has the following attributes:

- Owner permissions − The owner's permissions determine what actions the owner of the file can perform on the file.

- Group permissions − The group's permissions determine what actions a user, who is a member of the group that a file belongs to, can perform on the file.

- Other (world) permissions − The permissions for others indicate what action all other users can perform on the file.

The actions typically are Read(r), Write(w) and eXecute(x).


`chmod` to change these permissions

```
[ntc-netops-user1@ntc ~]$ chmod o-r /opt/ntc-netops/Ops_workflow.md 
[ntc-netops-user1@ntc ~]$ 
[ntc-netops-user1@ntc ~]$ ls -l /opt/ntc-netops/
total 4
-rw-rw----. 1 ntc-netops-user1 ntc-netops 252 Feb  6 07:44 Ops_workflow.md
[ntc-netops-user1@ntc ~]$
```
---
class: ubuntu

# Change owner or group

```
[ntc-netops-user1@ntc ~]$ sudo chown -R  ntc-netops-user1:ntc-netops /opt/ntc-netops/
[sudo] password for ntc-netops-user1: 
[ntc-netops-user1@ntc ~]$ ls -l /opt
total 0
drwxr-xr-x. 2 ntc-netops-user1 ntc-netops  6 Feb  6 05:25 ntc-netops
```
---

# Lab Time

- Lab 11 - User administration


---
class: ubuntu
# Alias

Shortcuts to common commands

```
alias e='vim'
```

To list current aliases

```
[ntc@ntc ~]$ alias
alias e='vim'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l.='ls -d .* --color=auto'
alias la='ls -ltra'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias vi='vim'
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'
[ntc@ntc ~]$
```


---
class: ubuntu

# Modifying the PATH

The `$PATH` variable determines the search path for executables

Use the `which` command to find the current path of a given binary:

```
[ntc@ntc ~]$ which python
/bin/python
[ntc@ntc ~]$
```

Update the path using `export`

```
[ntc@ntc ~]$ mkdir mybin
[ntc@ntc ~]$ 
[ntc@ntc ~]$ export PATH=$PATH:$HOME/mybin/
[ntc@ntc ~]$ echo $PATH
/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/ntc/.local/bin:/home/ntc/bin:./mybin/:/home/ntc/mybin/
[ntc@ntc ~]$ 

```

---

# .bashrc and .bash_profile

- `.bash_profile` is invoked at login
- `.bashrc` is invoked every time the shell is created

Add aliases into `.bashrc` for ensuring they are present always
Add environment variables into `.bash_profile` for each login



---

# Lab Time

- Lab 12 - Aliases and environment variables

---

class: ubuntu
# SSH - Generating keys

.left-column[
Use the `ssh-keygen` to generate key pairs

```
[ntc@ntc ~]$ ssh-keygen -t rsa -b 4096
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ntc/.ssh/id_rsa):
Created directory '/home/ntc/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/ntc/.ssh/id_rsa.
Your public key has been saved in /home/ntc/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:VKxJehWj6PjCDdZBeAlddX5205ivgoKIHBS41yKbZ9k ntc@ntc
The key's randomart image is:
+---[RSA 4096]----+
| .. .+.o.o=..    |
|.  ...+..oo+   o.|
| ... .ooo+  . =.o|
|o.o .+.o+    o o.|
| =.++ o.S       .|
|o.+=E= .   .   . |
| oo + + . . . .  |
|     .   .   .   |
|                 |
+----[SHA256]-----+
```
]

.right-column[

Private and public keys

```
[ntc@ntc ~]$ tree ~/.ssh
/home/ntc/.ssh
├── id_rsa
└── id_rsa.pub
```

]

---

class: ubuntu
# Known hosts

Known public keys :
- Automatically gathered by initial key exchange OR
- Manually added

```
[ntc@ntc .ssh]$ tree
.
├── id_rsa
├── id_rsa.pub
└── known_hosts
```

---

class: ubuntu
# SSH login without password

Adding the public key to `~/.ssh/authorized_hosts` either manually or using the `ssh-copy-id` command:

```
[ntc@ntc .ssh]$ ssh-copy-id ntc@proxy.ntc.com
/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/ntc/.ssh/id_rsa.pub"
/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
ntc@proxy.ntc.com's password:

Number of key(s) added: 1

```

---

class: ubuntu

# The SSH `config` file

Used to store SSH specific configuration:

```
Host bastion.ntc.com
  Hostname proxy.ntc.com
  User ntc
  ForwardAgent yes
```

Here we created an alias for proxy.ntc.com as bastion.ntc.com 

Unlike other aliases, this only impacts SSH. It also enables us to adjust ssh parameters on a per-host basis

---

class: ubuntu
# SSH Proxying

SSH supports a `ProxyCommand` option to bounce through an intermediate host

```
ssh -o ProxyCommand="ssh -W %h:%p bastion.ntc.com" veos1


```

Add this to the `~/.ssh/config` file
```
Host veostest
  Hostname eos-spine1
  User ntc
  ForwardAgent yes
  ProxyCommand ssh -W %h:%p bastion.ntc.com veos1
```


```
[ntc@ntc .ssh]$ ssh veostest
Password: 
Last login: Wed Feb  7 17:16:24 2018 from 10.0.0.3
eos-spine1#

```


---

class: ubuntu
# Troubleshooting SSH

Example of an error: 

.left-column[
```
[ntc@ntc ~]$ ssh ntc@asa1
Unable to negotiate with 192.168.33.96 port 22: no matching key exchange method found. Their offer: diffie-hellman-group1-sha1
```

```
[ntc@ntc ~]$ ssh ntc@asa1 -v
OpenSSH_7.4p1, OpenSSL 1.0.2k-fips  26 Jan 2017
debug1: Reading configuration data /home/ntc/.ssh/config
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 59: Applying options for *
debug1: Connecting to asa1 [192.168.33.96] port 22.
debug1: Connection established.
### cut for brevity ###
debug1: Authenticating to asa1:22 as 'ntc'
debug1: SSH2_MSG_KEXINIT sent
debug1: SSH2_MSG_KEXINIT received
debug1: kex: algorithm: (no match)
```

]

.right-column[
Increase the level of verbosity for even more debug 

```
debug2: KEX algorithms: diffie-hellman-group-exchange-sha256,curve25519-sha256@libssh.org,ext-info-c # client acceptance
debug2: KEX algorithms: diffie-hellman-group1-sha1 # target offer
```
]


---

class: ubuntu

# Fix for the issue:

```
Host asa1
  User ntc
  ForwardAgent yes
  ProxyCommand ssh -W %h:%p bastion.ntc.com asa1
  KexAlgorithms +diffie-hellman-group1-sha1
```

*ForwardAgent*: This option allows authentication keys stored on our local machine to be forwarded onto the system you are connecting to. This can allow you to hop from host-to-host using your home keys.

---

# Lab Time

- Lab 13 - SSH Configuration

---

class: center, middle, segue

# System Administration



---



class: center, middle, segue

# Package Management

---


class: ubuntu

# Package management


`yum` is used to install packages for CentOS (`apt` for Ubuntu)

`/etc/yum.conf` is used to configure yum settings

```
# Configuring yum access through a proxy

proxy=http://proxy.ntc.com:8080
```

---

class: ubuntu

# YUM commands

Query the public repositories and give a list of updated software
```
[ntc@ntc ~]$ sudo yum list updates
```

Find whether updates exists for software already installed on the system

```
[ntc@ntc ~]$ yum check-update command

```
Install all updates

```
[ntc@ntc ~]$ sudo yum update

```
---

class: ubuntu
# Yum commands(Contd.)

See all currently installed packages

```
[ntc@ntc ~]$yum list installed
```

Check if a particular package is installed

```
[ntc@ntc ~]$sudo yum list installed python
```

Search for a package within the public repository,

```
[ntc@ntc ~]$sudo yum search fortune
```

Installing a package

```
[ntc@ntc ~]$sudo yum install fortune-mod.x86_64
```

---

class: ubuntu
# More YUM commands...

.left-column[
`yum info` will give details about a package

```
[ntc@ntc ~]$sudo yum info htop
Loaded plugins: fastestmirror, priorities
Loading mirror speeds from cached hostfile
* base: mirror.cc.columbia.edu
* epel: mirror.cogentco.com
* extras: ftp.linux.ncsu.edu
* updates: repos-va.psychz.net
Available Packages
Name        : htop
Arch        : x86_64
Version     : 2.0.2
Release     : 1.el7
Size        : 98 k
Repo        : epel/x86_64
Summary     : Interactive process viewer
URL         : http://hisham.hm/htop/
License     : GPL+
Description : htop is an interactive text-mode process viewer for Linux, similar to
: top(1).

[ntc@ntc ~]$
```

]

.right-column[

```
[ntc@ntc ~]$sudo yum whatprovides tree
Loaded plugins: fastestmirror, priorities
Loading mirror speeds from cached hostfile
* base: mirrors.gigenet.com
* epel: mirror.cogentco.com
* extras: ftp.linux.ncsu.edu
* updates: mirror.cloud-bricks.net

```


]


---

class: ubuntu
# Uninstalling packages


Use `yum remove` to uninstall packages

```
[ntc@ntc ~]$sudo yum remove fortune-mod.x86_64
Loaded plugins: fastestmirror, priorities
Resolving Dependencies
--> Running transaction check
---> Package fortune-mod.x86_64 0:1.99.1-17.el7 will be erased
--> Finished Dependency Resolution
```

---

class: ubuntu

# System Identification


Release information is in `/etc/redhat-release`

```
[ntc@ntc ~]$ cat /etc/redhat-release 
CentOS Linux release 7.4.1708 (Core)
```

For more details:

.small-code[
```
[ntc@ntc ~]$ cat /etc/*elease
CentOS Linux release 7.4.1708 (Core) 
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"

CentOS Linux release 7.4.1708 (Core) 
CentOS Linux release 7.4.1708 (Core)

```
]

---

class: ubuntu
# System Information(Contd.)

Using `hostnamectl`

.small-code[
```
[ntc@ntc ~]$ hostnamectl
   Static hostname: ntc
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 1e2b46dbc0c04b05b592c837c366bb76
           Boot ID: 907bb0e54c8c47a1a4d6a7dda082156e
    Virtualization: xen
  Operating System: CentOS Linux 7 (Core)
       CPE OS Name: cpe:/o:centos:centos:7
            Kernel: Linux 3.10.0-693.17.1.el7.x86_64
      Architecture: x86-64
[ntc@ntc ~]$ 


```
]

`hostnamectl` can also be used to update the hostname of the server

.small-code[
```
[ntc@ntc ~]$ hostnamectl --help                                                                                                 master
hostnamectl [OPTIONS...] COMMAND ...

Query or change system hostname.

  -h --help              Show this help
     --version           Show package version
     --no-ask-password   Do not prompt for password
  -H --host=[USER@]HOST  Operate on remote host
  -M --machine=CONTAINER Operate on local container
     --transient         Only set transient hostname
     --static            Only set static hostname
     --pretty            Only set pretty hostname

Commands:
  status                 Show current hostname settings
  set-hostname NAME      Set system hostname
  set-icon-name NAME     Set icon name for host
  set-chassis NAME       Set chassis type for host
  set-deployment NAME    Set deployment environment for host
  set-location NAME      Set location for host


```
]

---

class: ubuntu
# Kernel Information

Use `uname -r` for identifying the current kernel

```
[ntc@ntc ~]$ uname -r
3.10.0-693.17.1.el7.x86_64
[ntc@ntc ~]$ 

```
Details can be obtained from `/proc/version`
```

[ntc@ntc ~]$ cat /proc/version 
Linux version 3.10.0-693.17.1.el7.x86_64 (builder@kbuilder.dev.centos.org) (gcc version 4.8.5 20150623 (Red Hat 4.8.5-16) (GCC) ) #1 SMP Thu Jan 25 20:13:58 UTC 2018
[ntc@ntc ~]$ 

```

---

class: ubuntu
# Gathering CPU and Memory information

.left-column[
CPU information

```
[ntc@ntc ~]$ cat /proc/cpuinfo
processor    : 0
vendor_id    : GenuineIntel
cpu family    : 6
model        : 158
model name    : Intel(R) Core(TM) i7-7700HQ CPU @ 2.80GHz
stepping    : 9
cpu MHz        : 2808.002
cache size    : 6144 KB
physical id    : 0
siblings    : 2
core id        : 0
cpu cores    : 2
apicid        : 0
initial apicid    : 0
fpu        : yes
fpu_exception    : yes
cpuid level    : 22
```
]

.right-column[
Memory Information

```
[ntc@ntc ~]$ cat /proc/meminfo 
MemTotal:        3881696 kB
MemFree:         3489816 kB
MemAvailable:    3526364 kB
Buffers:            2108 kB
Cached:           231576 kB
SwapCached:            0 kB
Active:           207288 kB
Inactive:          84088 kB
Active(anon):      57892 kB
Inactive(anon):     8324 kB
Active(file):     149396 kB
Inactive(file):    75764 kB
```
]

---

class: ubuntu
# Health check using `vmstat` and `free`

`vmstat` reports information about processes, memory, paging, block IO, traps, disks and cpu activity.

```
[ntc@ntc ~]$ vmstat
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 1  0      0 3489668   2108 283628    0    0     2     1    9    8  0  0 100  0  0

```

`free`  displays  the total amount of free and used physical and swap memory in the system, as well as the buffers and caches used by the kernel. The information is gathered by parsing /proc/meminfo. 

```
[ntc@ntc ~]$ free -h
              total        used        free      shared  buff/cache   available
Mem:           3.7G        103M        3.3G        8.3M        279M        3.4G
Swap:          1.5G          0B        1.5G
```

---
# Lab Time

- Lab 14 - Package Management
- Lab 15 - Health Monitoring


---

class: ubuntu
# Process monitoring

`top` and the more human-readable `htop` are excellent tools to troubleshoot processes using hardware resources on the system

```
[ntc@ntc ~]$ top

top - 05:59:51 up 14:37,  1 user,  load average: 0.00, 0.01, 0.05
Tasks:  96 total,   2 running,  94 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.2 sy,  0.0 ni, 99.8 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  3881696 total,  3488304 free,   107008 used,   286384 buff/cache
KiB Swap:  1572860 total,  1572860 free,        0 used.  3525532 avail Mem 

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                               
    1 root      20   0  128168   6840   4080 S   0.3  0.2   0:04.98 systemd                               
10688 ntc       20   0   57524   2172   1508 R   0.3  0.1   0:00.06 top                                   
    2 root      20   0       0      0      0 S   0.0  0.0   0:00.00 kthreadd                              
    3 root      20   0       0      0      0 S   0.0  0.0   0:00.29 ksoftirqd/0                           
    5 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 kworker/0:0H                          
    6 root      20   0       0      0      0 S   0.0  0.0   0:00.00 kworker/u4:0                          
    7 root      rt   0       0      0      0 S   0.0  0.0   0:00.04 migration/0                           
    8 root      20  
```

Depending on your distribution, `htop` might not come installed as part of the core operating system and will need to be installed.


---

class: ubuntu
# Process Management


Use the `&` to run a command in the background

```
[ntc@ntc ~]$ top &
[1] 2361
[ntc@ntc ~]$ 

```

- The output `2361` is the process ID or `PID`
- Typically used for long running processes that do not need intervention
- Use `fg` to immediately bring a task to foreground

---

class: ubuntu
# The process ID


.left-column[
Use `ps -ef` to list current running processes

.small-code[

```
[ntc@ntc ~]$ ps -ef | more
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 06:42 ?        00:00:16 /usr/lib/systemd/systemd --switched-root --system --deserialize 21
root         2     0  0 06:42 ?        00:00:00 [kthreadd]
root         3     2  0 06:42 ?        00:00:00 [ksoftirqd/0]
root         5     2  0 06:42 ?        00:00:00 [kworker/0:0H]
root         9     2  0 06:42 ?        00:00:01 [rcu_sched]
root        10     2  0 06:42 ?        00:00:00 [watchdog/0]
root        11     2  0 06:42 ?        00:00:00 [watchdog/1]

.
.
<output truncated for readability>
```
]
]


.right-column[
Extensive command with many options
.small-code[

```
[ntc@ntc ~]$ ps --help simple

Usage:
 ps [options]

Basic options:
 -A, -e               all processes
 -a                   all with tty, except session leaders
  a                   all with tty, including other users
 -d                   all except session leaders
 -N, --deselect       negate selection
  r                   only running processes
  T                   all processes on this terminal
  x                   processes without controlling ttys

For more details see ps(1).
[ntc@ntc ~]$ 

```
]

]

---


class: ubuntu

# Terminating a process

- Kill by process id:


```
[ntc@ntc ~]$ kill 20827 

```

    Kill `-9` is used to `force kill` processes 

- Kill by process name:

```
[ntc@ntc ~]$ sudo pkill httpd
```

- "Hangup" to read a config file


```
[ntc@ntc ~]$ sudo kill -HUP httpd
```

*Process ID will not change*





---


class: ubuntu
# Logging

System log files depend on distribution

**RedHat variants**: `/var/log/messages`
**Debian variants**: `/var/log/syslog`


```
[ntc@ntc ~]$ sudo tail -f /var/log/messages
Feb  8 17:01:01 localhost systemd: Started Session 32 of user root.
Feb  8 17:01:01 localhost systemd: Starting Session 32 of user root.
Feb  8 17:01:01 localhost systemd: Removed slice User Slice of root.
Feb  8 17:01:01 localhost systemd: Stopping User Slice of root.
Feb  8 18:01:01 localhost systemd: Created slice User Slice of root.
Feb  8 18:01:01 localhost systemd: Starting User Slice of root.
Feb  8 18:01:01 localhost 

```

*Need to invoke `sudo` to view logs

---

class: ubuntu
# Generating logs


Use `logger` to write log messages to file


```
[ntc@ntc ~]$ logger -h

Usage:
 logger [options] [message]

```

Generating a "maillog" event
```
[ntc@ntc ~]$ sudo logger -p mail.err "Test Log"
[sudo] password for ntc: 

```

Observed output
```

[ntc@ntc ~]$ sudo tail -1 /var/log/maillog
Feb 13 08:20:11 ntc ntc: Test Log
[ntc@ntc ~]$ 

```


---
# Lab Time

- Lab 16 - Process Management
- Lan 17 - Logging

---

class: ubuntu
# Disk usage

The `df` command is used to display information concerning the file systems available to the host.

```
[ntc@ntc ~]$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2       100G  3.5G   97G   4% /
devtmpfs        7.8G     0  7.8G   0% /dev
tmpfs           7.8G     0  7.8G   0% /dev/shm
tmpfs           7.8G  8.8M  7.8G   1% /run
tmpfs           7.8G     0  7.8G   0% /sys/fs/cgroup
/dev/sda1       5.0G  226M  4.8G   5% /boot
/dev/sda5        90G  148M   90G   1% /home
tmpfs           1.6G   12K  1.6G   1% /run/user/42
tmpfs           1.6G     0  1.6G   0% /run/user/1001

```

To check usage of a single mount point:

```
[ntc@ntc ~]$ df -h /home
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda5        90G  148M   90G   1% /home
[ntc@ntc ~]$ 

```

---

class: ubuntu
# Disk usage(Contd.)

The `du` command reports the sizes of directory trees including all files within that tree.

.small-code[
```
[ntc@ntc ~]$ sudo du -h /var/log
[sudo] password for ntc: 
0	/var/log/samba/old
0	/var/log/samba
0	/var/log/ppp
0	/var/log/glusterfs
9.8M	/var/log/audit
0	/var/log/chrony
0	/var/log/ntpstats
0	/var/log/pluto/peer
0	/var/log/pluto
0	/var/log/libvirt/qemu
0	/var/log/libvirt
0	/var/log/sssd
0	/var/log/speech-dispatcher
4.0K	/var/log/cups
88K	/var/log/gdm
16K	/var/log/tuned
1.1M	/var/log/sa
0	/var/log/qemu-ga
2.9M	/var/log/anaconda
0	/var/log/rhsm
21M	/var/log
[ntc@ntc ~]$ 

```
]

---

class: ubuntu
# Disk usage summary

To only generate a summary:

```
[ntc@ntc ~]$ sudo du -hs /var/log
21M	/var/log
[ntc@ntc ~]$ 

```

---

class: middle, segue

# Regular Expressions

---


# RegEx Overview

- A Regular Expression (RegEx) is a special sequence of characters used to search patterns inside text.
- They are a powerful tool for checking if a specific pattern is present inside a text.

---

# Web Utilities for Testing

Online tools used for testing and learning regular expressions

- Regexr.com (picture below)
- Regex101.com



.center[
<img src="slides/media/regexr/regexr1.png" alt="Regexr.com Example" style="alight:middle;width:800px;height:325px;">
]

---

# Regex patterns

- **\d**: Matches any decimal digit
- **\D**: Matches any non-digit character
- **\w**: Matches any alphanumeric character
- **\W**: Matches any non-alphanumeric character
- **\s**: Matches any whitespace character
- **\S**: Matches any non-whitespace character
- **.**: Matches anything except a newline character
- **+**: Specifies that the previous character can be matched one or more times
- ** * **: Specifies that the previous character can be matched zero or more times
- ** ? **: Matches either once or zero times. Indicates something as being optional
    - Example: **ntc-?training** matches either **ntctraining** or **ntc-training**


---


class: ubuntu
# Using grep

Prints lines matching a pattern

.left-column[
Find all interfaces  and routes withing a file

```
[ntc@ntc ~]$ grep -inE "(route|gigabitethernet)" ./configs/atl-rtr01.cfg
150:interface GigabitEthernet1
155:interface GigabitEthernet2
160:interface GigabitEthernet3
165:interface GigabitEthernet4
178:ip route vrf MANAGEMENT 0.0.0.0 0.0.0.0 10.0.0.2
```
]

.right-column[

Find all IP addresses within a file:

```
[ntc@ntc ~]$ grep -nE "\b([0-9]{1,3}\.){3}[0-9]{1,3}\b" ./configs/atl-rtr01.cfg
152: ip address 10.0.0.51 255.255.255.0
178:ip route vrf MANAGEMENT 0.0.0.0 0.0.0.0 10.0.0.2

```

]

---

class: ubuntu
# Using grep context

The `-A` flag is used to print the number of lines "after" search context

```
[ntc@ntc ~]$ grep -A 4 "GigabitEthernet1" configs/nyc-rtr02.cfg 
interface GigabitEthernet1
 vrf forwarding MANAGEMENT
 ip address 10.0.0.51 255.255.255.0
 negotiation auto
!

```

Similarly use the `-B` flag for printing "before" context


---

class: ubuntu

# The `find` command

The `find` command helps search for files in a directory hierarchy


```
[ntc@ntc ~]$ sudo find / -name "yum.conf"
[sudo] password for ntc: 
/etc/yum.conf
[ntc@ntc ~]$ 

```

Use find with `xargs` and `grep` to search within files


```
[ntc@ntc ~]$ sudo find / -name "yum.conf" | xargs grep log
logfile=/var/log/yum.log
[ntc@ntc ~]$ 

```


---
# Lab Time

Lab 18 - Disk usage, find and grep


---
class: center, middle, segue

# Networking on Linux

---



class: ubuntu
# Network name resolution

- By default checks `/etc/hosts` file first.

- Based on the order defined in `/etc/nssswitch.conf`

```
[ntc@ntc ~]$ cat /etc/nsswitch.conf | grep dns
#	dns			Use DNS (Domain Name Service)
#hosts:     db files nisplus nis dns
hosts:      files dns myhostname
[ntc@ntc ~]$ 

```


---

class: ubuntu
# The `/etc/resolv.conf` file 

The `/etc/resolv.conf` is used to identify the nameserver closest to the host for DNS resolution. This is typically automatically populated via dhcp.


```
[ntc@ntc ~]$ cat /etc/resolv.conf 
; generated by /usr/sbin/dhclient-script
search localdomain
nameserver 10.0.0.1
[ntc@ntc ~]$
```


---

class: ubuntu
# nslookup

- Nslookup is a program to query Internet domain name servers.
- Nslookup has two modes: interactive and non-inte


.left-column[
Non Interactive 
```
[ntc@ntc ~]$ nslookup yahoo.com
Server:        8.8.8.8
Address:    8.8.8.8#53

Non-authoritative answer:
Name:    yahoo.com
Address: 98.138.252.38
Name:    yahoo.com
Address: 206.190.39.42
Name:    yahoo.com
Address: 98.139.180.180

[ntc@ntc ~]$
```
]

.small-code[
.right-column[
Interactive

```
[ntc@ntc ~]$ nslookup
> server
Default server: 8.8.8.8
Address: 8.8.8.8#53
> yahoo.com
Server:        8.8.8.8
Address:    8.8.8.8#53

Non-authoritative answer:
Name:    yahoo.com
Address: 98.138.252.38
Name:    yahoo.com
Address: 206.190.39.42
Name:    yahoo.com
Address: 98.139.180.180
>

> server 10.0.0.1
Default server: 10.0.0.1
Address: 10.0.0.1#53
```
]
]


---

class: ubuntu

# Using dig

- `dig` (domain information groper) is a flexible tool for
interrogating DNS name servers. It performs DNS lookups and
displays the answers that are returned from the name server(s)
that were queried. 

- Most DNS administrators use dig to
troubleshoot DNS problems because of its flexibility, ease of
use and clarity of output. Other lookup tools tend to have less
functionality than dig.


---


class: ubuntu

# An example of a dig query

```
[ntc@ntc ~]$ dig juniper.com

; <<>> DiG 9.9.4-RedHat-9.9.4-51.el7_4.2 <<>> juniper.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 4121
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;juniper.com.            IN    A

;; ANSWER SECTION:
juniper.com.        42    IN    A    192.107.16.40

;; Query time: 12 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Fri Feb 09 11:16:09 EST 2018
;; MSG SIZE  rcvd: 56

[ntc@ntc ~]$
```

Pay attention to the output particularly to the following fields: status, ANSWER SECTION, Query time and SERVER

---

class: ubuntu

# Flexible queries with `dig`

Query a specific DNS server
```
[ntc@ntc ~]$ dig @10.0.0.1 juniper.com
```

Collect the query path
```
[ntc@ntc ~]$ dig @8.8.8.8 eos.arista.com +trace
```


Short output
```
[ntc@ntc ~]$ dig @8.8.8.8 eos.arista.com  +short
ipv6.arista.com.edgekey.net.
e10346.dscb.akamaiedge.net.
96.6.55.176
[ntc@ntc ~]$
```

---


class: ubuntu
# The `ip` commands

Checking routes

```
[ntc@ntc ~]$ ip route
192.168.0.0/24 dev ens4 proto kernel scope link src 192.168.0.52 
192.168.122.0/24 dev virbr0 proto kernel scope link src 192.168.122.1 
[ntc@ntc ~]$
```


Checking the status of interfaces

```
[ntc@ntc ~]$ ip link 
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: ens3: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT qlen 1000
    link/ether 2c:c2:60:3e:dd:80 brd ff:ff:ff:ff:ff:ff
3: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT qlen 1000
    link/ether 2c:c2:60:38:11:af brd ff:ff:ff:ff:ff:ff
4: virbr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DEFAULT qlen 1000
    link/ether 52:54:00:ed:da:3f brd ff:ff:ff:ff:ff:ff
5: virbr0-nic: <BROADCAST,MULTICAST> mtu 1500 qdisc pfifo_fast master virbr0 state DOWN mode DEFAULT qlen 1000
    link/ether 52:54:00:ed:da:3f brd ff:ff:ff:ff:ff:ff
[ntc@ntc ~]$
```

---


class: ubuntu
# IP address information

```
[ntc@ntc ~]$ ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens3: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN qlen 1000
    link/ether 2c:c2:60:3e:dd:80 brd ff:ff:ff:ff:ff:ff
3: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether 2c:c2:60:38:11:af brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.52/24 brd 192.168.0.255 scope global dynamic ens4
       valid_lft 359415sec preferred_lft 359415sec
    inet6 fe80::7070:148a:e15b:b2fb/64 scope link 
       valid_lft forever preferred_lft forever
4: virbr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN qlen 1000
    link/ether 52:54:00:ed:da:3f brd ff:ff:ff:ff:ff:ff
    inet 192.168.122.1/24 brd 192.168.122.255 scope global virbr0
       valid_lft forever preferred_lft forever
5: virbr0-nic: <BROADCAST,MULTICAST> mtu 1500 qdisc pfifo_fast master virbr0 state DOWN qlen 1000
    link/ether 52:54:00:ed:da:3f brd ff:ff:ff:ff:ff:ff
[ntc@ntc ~]$
```

---

class: ubuntu
# Turning up/down an interface

Option 1

```
[ntc@ntc ~]$ sudo ip link set ens3 up
[sudo] password for ntc: 
[ntc@ntc ~]$
```
This brings up the link but you will need to manually start the dhcp client.

```
[ntc@ntc ~]$ sudo dhclient
[sudo] password for ntc: 
[ntc@ntc ~]$
```

To turn the interface down:

```
[ntc@ntc ~]$ sudo ip link set ens3 down
[sudo] password for ntc: 
[ntc@ntc ~]$
```

---

class: ubuntu
# Turning up/down an interface

Option 2

Using `ifup` and `ifdown` is the recommended way to turn interfaces up or down

```
[ntc@ntc ~]$ sudo ifup ens3

Determining IP information for ens3... done.
RTNETLINK answers: File exists
```
Shutting down an interface:

```
[ntc@ntc ~]$ sudo ifdown ens3
[ntc@ntc ~]$
```

---

class: ubuntu
# Network configuration files

The network configurations are controlled by the files in `/etc/sysconfig/network-scripts`.


```
[ntc@ntc ~]$ more /etc/sysconfig/network-scripts/ifcfg-ens3 
DEVICE=ens3
ONBOOT=no
NM_CONTROLLED=no
BOOTPROTO=dhcp
[ntc@ntc ~]$

```
The `systemctl` command is used to stop and start services on the device

```
[ntc@ntc ~]$ sudo systemctl restart network
[sudo] password for ntc: 
[ntc@ntc ~]$
```


---
# Lab Time

- Lab 19 - Basic network lab
---

class: ubuntu
# Network tools 

- **tcpdump**: Command line traffic dump tool
- **netcat**: Swiss knife tool for TCP/UDP traffic testing
- **nmap**: Network exploration tool and security / port scanner

---

class: ubuntu
# Network tools - tcpdump

Flags while using `tcpdump`

```
    -i any : Listen on all interfaces just to see if you’re seeing any traffic.
    -i eth0 : Listen on the eth0 interface.
    -D : Show the list of available interfaces
    -n : Don’t resolve hostnames.
    -nn : Don’t resolve hostnames or port names.
    -t : Give human-readable timestamp output.
    -tttt : Give maximally human-readable timestamp output.
    -X : Show the packet’s contents in both hex and ASCII.
    -XX : Same as -X, but also shows the ethernet header.
    -v, -vv, -vvv : Increase the amount of packet information you get back.
    -c : Only get x number of packets and then stop.
    -s : Define the snaplength (size) of the capture in bytes. Use -s0 to get everything, unless you are intentionally capturing less.
    -S : Print absolute sequence numbers.
    -e : Get the ethernet header as well.
    -q : Show less protocol information.
    -E : Decrypt IPSEC traffic by providing an encryption key.

```


---

class: ubuntu

# tcpdump - examples

Traffic by IP or network

```
[ntc@ntc web]$ sudo tcpdump host 1.2.3.4
```

```
[ntc@ntc web]$ sudo tcpdump net 10.0.0.0/8

```

Traffic on any interface on port 5000 (src or dest)

```bash
[ntc@ntc web]$ sudo tcpdump -i any port 5000
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on any, link-type LINUX_SLL (Linux cooked), capture size 262144 bytes
```

Traffic on any interface on port 5000 (src only)

```bash
[ntc@ntc web]$ sudo tcpdump -i any src port 5000
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on any, link-type LINUX_SLL (Linux cooked), capture size 262144 bytes
```

---

class: ubuntu
# Network Tools - netcat

Netcat is often referred to as the Swiss army knife in networking tools.

Example use cases:

- Simple File Transfer
- Streaming text 
- Testing path connectivity over TCP/UDP
- Security/Penetration use cases


---

class: ubuntu
# netcat - examples

Validating listener ports:

```bash
[ntc@ntc ~]$ nc -v 127.0.0.1 5000
Ncat: Version 7.60 ( https://nmap.org/ncat )
Ncat: Connected to 127.0.0.1:5000.
```

Scan a range of ports
```
[ntc@ntc ~]$nc -z -v 127.0.0.1 1-65535 #this will scan all ports
```

To check external connectivity:

```bash
[ntc@ntc ~]$nc -v google.com 80
Ncat: Version 7.60 ( https://nmap.org/ncat )
Ncat: Connected to 172.217.1.46:80.
```


---

class: ubuntu
# netcat - examples(Contd.)
Stream text

.left-column[
```
#Server side/listener
[ntc@ntc ~]$ nc -lvp 1111
Ncat: Version 6.40 ( http://nmap.org/ncat )
Ncat: Listening on :::1111
Ncat: Listening on 0.0.0.0:1111


```


```
#Client site
[ntc@ntc ~]$ nc localhost 1111 < vlan.yml 
[ntc@ntc ~]$ 
```
]

.right-column[

```
[ntc@ntc ~]$ nc -lvp 1111
Ncat: Version 6.40 ( http://nmap.org/ncat )
Ncat: Listening on :::1111
Ncat: Listening on 0.0.0.0:1111
Ncat: Connection from ::1.
Ncat: Connection from ::1:35846.
---                                                                                                           
output:                                                                                                       
  proposed:                                                                                                   
    name: NTC                                                                                                 
  existing_vlans_list:                                                                                        
    - '1'                                                                                                     
  end_state_vlans_list:                                                                                       
    - '1'                                                                                                     
    - '100'                                                                                                   
  existing: {}                                                                                                
  updates:                                                                                                    
  - vlan 100                  
```


]

---

class: ubuntu
# Network Tools - nmap

- Nmap (“Network Mapper”) is an open source tool for network
exploration and security auditing. It was designed to rapidly
scan large networks, although it works fine against single
hosts. 
- Nmap uses raw IP packets to determine :
  - what hosts are available on the network,
  - what services (application name and version) those hosts are offering,
  - what operating systems (and OS versions) they are running,
  - what type of packet filters/firewalls are in use etc.

Nmap is commonly used for security audits. It is useful
for routine tasks such as network inventory, managing service
upgrade schedules, and monitoring host or service uptime.



---

class: ubuntu
# Nmap - common examples/cheat sheet

Ping scan:

```
ntc@ntc:~$ nmap -sP 10.0.0.0/24

```
Full TCP port scan:

```
ntc@ntc:~$ nmap -p 1-65535 -sV -sS -T4 target
```

Scan a list of IP addresses

```
ntc@ntc:~$ nmap -iL ip-addresses.txt
```

---

class: ubuntu
# Nmap - flags

Host discovery

```
-sL List Scan - simply list targets to scan
-sn Ping Scan - disable port scan
-Pn Treat all hosts as online -- skip host discovery
-PE/PP/PM ICMP echo, timestamp, and netmask request discovery probes
-n/-R Never do DNS resolution/Always resolve

```

Scan techniques
```
-sS TCP SYN scan
-sT Connect scan
-sU UDP Scan
-sF FIN scan
-sX Xmas scan
```

---

# Lab Time

- Lab 20 - Using tcpdump
- Lab 21 - Using netcat and nmap


---

# Linux firewall - iptables

`iptables` is an administration tool for IPv4 and IPv6 packet filtering and NAT

- Iptables are  used  to  set  up,  maintain,  and
inspect  the tables of IPv4 and IPv6 packet filter rules in the
Linux kernel.

- Several different tables may be  defined.   Each
table contains a number of built-in chains and may also contain
user-defined chains.

- Each chain is a list of rules which can match a set of packets.
Each  rule  specifies  what  to  do with a packet that matches.
This is called a `target`, which may  be  a  jump  to  a  user-
defined chain in the same table.

---

class: ubuntu

# Linux firewall - iptables terminology

- **Rules**: The iptables firewall operates by comparing network traffic against a set of rules. The rules define the characteristics that a packet must have to match the rule, and the action that should be taken for matching packets.
- **Target**: When the defined pattern matches, the action that takes place is called a target. A target can be a final policy decision for the packet, such as accept, or drop. It can also be move the packet to a different chain for processing, or simply log the encounter. There are many options.
- **Chains**: The rules are organized into groups called chains. A chain is a set of rules that a packet is checked against sequentially

A user can create chains as needed. There are three chains defined by default. They are:

```
INPUT: This chain handles all packets that are addressed to your server.
OUTPUT: This chain contains rules for traffic created by your server.
FORWARD: This chain is used to deal with traffic destined for other servers
```

---

# iptables - IPv4 Versus IPv6

The netfilter firewall that is included in the Linux kernel keeps IPv4 and IPv6 traffic completely separate. 


The regular iptables command is used to manipulate the table containing rules that govern IPv4 traffic. For IPv6 traffic, a companion command called ip6tables is used. 

---

class: ubuntu
# iptables - Basic commands

```
sudo iptables -L -v -n
	List all rules with verbose and numeric output
sudo iptables -S
	Print all rules in iptables-save format
sudo iptables -A <chain> <rule>
	Append one or more rules to the end of the selected chain
sudo iptables -I <chain> <rulenum> <rule>
	Insert one or more rules in the selected chain as the given rule number
sudo iptables -D <chain> <rulenum>
	Delete one or more rules at the given position from the selected chain
sudo iptables -P <chain> <target>
	Set the policy for the chain to the given target
sudo iptables -N <chain>
	Create a new user-defined chain by the given name
sudo iptables -F <chain>
	Flush the selected chain
sudo iptables -X <chain>
	Delete the optional user-defined chain 
sudo iptables-save > /path/to/file
	Save current rules
sudo iptables-restore < /path/to/file
	Load rules from a file 
```

---

class: ubuntu

# iptables - Examples

Flush the tables:

```
iptables -F
```

Set the default chain policies:

```
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT DROP
```

Show the status
```
iptables -L -n -v --line-numbers
```
---
class: ubuntu

# iptables - examples (Contd.)

Block an IP (incoming)

```
iptables -A INPUT -s 1.2.3.4 -j DROP
```

Block outbound connectivity

```
iptables -A OUTPUT -p tcp -d www.facebook.com -j DROP
iptables -A OUTPUT -p tcp -d facebook.com -j DROP
```


---
# Lab Time

- Lab 22 - iptables part1
- Lab 23 - iptables part2


---
class: center, middle, segue

# Bash scripting

---

class: ubuntu
# Bash scripting 

Scripts are typically nothing but a collection of Linux commands placed in a file. 
Scripts begin with a `#!` followed by the shell being used.

Eg:

```
#!/bin/bash

echo "HELLO!"
```

The `#!` or "shebang" line tells the shell what interpreter to use while processing the script.


---

class: ubuntu
# Bash scripting - variables

While assigning values to variables, make sure there are no spaces. 
Shell script variables are referenced using the `$` symbol. 

.left-column[
```
#!/bin/bash

hostname=router1
echo "The core router is $hostname"
```

```
[ntc@ntc ~]$ bash router.sh 
The core router is router1
[ntc@ntc ~]$ 
```

Use `chmod +x ` to allow direct execution of the script for everyone

```
[ntc@ntc ~]$ chmod +x router.sh 
[ntc@ntc ~]$ 

```

]

.right-column[

Environment variables are also available within the script.

```
#!/bin/bash

hostname=router1
echo "The core router is $hostname"
echo "The managemet script is being executed by $USER from $HOSTNAME"

```

```
[ntc@ntc ~]$ ./router.sh 
The core router is router1
The managemet script is being executed by ntc from ntc
[ntc@ntc ~]$ 

```
]


---


class: ubuntu

# Bash scripting - Conditionals

Conditionals provide an easy way to change a scripts behavior based on a certain value, assignment, or input.

The basic syntax for conditionals is `if (test passes) then`

*testing*: Use the `test (expression)` command 

```

[ntc@ntc ~]$ test 5 -eq 5 && echo Yes || echo No
Yes
[ntc@ntc ~]$ test 5 -eq 15 && echo Yes || echo No
No
[ntc@ntc ~]$ 
```

Placing the expression within `[ ]` is common


---

class: ubuntu
# Testing files

A common operation while scripting is to check for the presence of a file or folder



```
-e
file exists

-a
file exists

This is identical in effect to -e. It has been "deprecated," [1] and its use is discouraged.

-f
file is a regular file (not a directory or device file)

-s
file is not zero size

-d
file is a directory
```

---

class: ubuntu
# Testing if directory

```
[ntc@ntc ~]$ ls -p
configs/    findme/                  ping.sh     Videos/
Desktop/    interface_validation.sh  Public/     vlan.json
Documents/  Music/                   router.sh   vlan.yml
Downloads/  Pictures/                Templates/  whitespace.data
[ntc@ntc ~]$ 

```

```
[ntc@ntc ~]$ test -d configs && echo yes || echo no
yes

```

---

class: ubuntu

# Using the `if` conditional


Example:

```
#!/bin/bash

hostname=router1
echo "The core router is $hostname"

if [ $hostname == "router1" ]; then
  echo "The managemet script is being executed by $USER from $HOSTNAME"
fi

```

output:

```
[ntc@ntc ~]$ ./router.sh 
The core router is router1
The managemet script is being executed by ntc from ntc

```

---

class: ubuntu
# Handling test failure using `else`

```
#!/bin/bash

interface="Ethernet1/1"

if [ $interface == "Tunnel101" ]
then
  echo "Configuring Tunnel101"
else
  echo "Configuring Ethernet1/1"
fi

```

---

class: ubuntu

# The `elif` statement

Can be used to test multiple conditionals


```
#!/bin/bash

interface="Ethernet1/1"

if [ $interface == "Tunnel101" ]
then
  echo "Configuring Tunnel101"
elif [ $interface == "Ethernet1/1" ]
then
  echo "Configuring Ethernet1/1"
else
  echo "unknown interface"
fi

```


---


---
# Lab Time

- Lab 24 - Basic Bash scripting
- Lab 25 - Advanced Bash scripting

---

class: ubuntu

# Using Loops - the for loop

Used for repeating tasks until exhaustion of a vairable

A script to list files
```
#!/bin/bash
for i in `ls`; do
    echo item: $i
done 
```

A script to loop through each interface

```
intfs=("Ethernet1" "Ethernet2" "Tunnel101")

for intf in ${intfs[@]} 
do
  echo $intf
  if [ $intf == "Tunnel101" ]
  then
    echo "Configuring Tunnel101"
  elif [ $intf == "Ethernet1" ]
  then
    echo "Configuring Ethernet1"
  else
   echo "unknown interface"
fi


done

```
---

class: ubuntu

# For loop - execution

```
[ntc@ntc ~]$ ./interface_validation.sh 
Configuring Ethernet1/1
Ethernet1
Configuring Ethernet1
Ethernet2
unknown interface
Tunnel101
Configuring Tunnel101
[ntc@ntc ~]$ 

```
---

class: ubuntu
# Loops - while loops

Used for repeating tasks until a change of state to a variable occurs


```
#!/bin/bash

COUNTER=0
while [  $COUNTER -lt 10 ]; do
 echo "router$COUNTER" > /home/ntc/configs/router-$COUNTER.cfg
 let COUNTER=COUNTER+1 
done

```
Execution output:

.small-code[
```
[ntc@ntc ~]$ sh generate_intf.sh 
[ntc@ntc ~]$ ls -ltr configs/
total 116
-rw-rw-r--. 1 ntc ntc 4181 Feb  8 11:43 nyc-rtr02.cfg
-rw-rw-r--. 1 ntc ntc 4181 Feb  8 11:44 nyc-rtr03.cfg
-rw-rw-r--. 1 ntc ntc 4181 Feb  8 11:44 nyc-rtr04.cfg
-rw-rw-r--. 1 ntc ntc 4181 Feb  8 11:44 nyc-rtr05.cfg
-rw-rw-r--. 1 ntc ntc 4181 Feb  8 11:44 atl-rtr01.cfg
-rw-rw-r--. 1 ntc ntc 4181 Feb  8 11:44 atl-rtr02.cfg
-rw-rw-r--. 1 ntc ntc 4181 Feb  8 11:44 atl-rtr03.cfg
-rw-rw-r--. 1 ntc ntc 4181 Feb  8 11:45 atl-rtr04.cfg
-rw-rw-r--. 1 ntc ntc 4181 Feb  8 11:45 atl-rtr05.cfg
-rw-rw-r--. 1 ntc ntc 4059 Feb 12 09:26 nyc-rtr01.cfg
-rw-rw-r--. 1 ntc ntc    8 Feb 14 15:52 router-0.cfg
-rw-rw-r--. 1 ntc ntc    8 Feb 14 15:52 router-2.cfg
-rw-rw-r--. 1 ntc ntc    8 Feb 14 15:52 router-1.cfg
-rw-rw-r--. 1 ntc ntc    8 Feb 14 15:52 router-3.cfg
-rw-rw-r--. 1 ntc ntc    8 Feb 14 15:52 router-5.cfg
-rw-rw-r--. 1 ntc ntc    8 Feb 14 15:52 router-4.cfg
-rw-rw-r--. 1 ntc ntc    8 Feb 14 15:52 router-6.cfg
-rw-rw-r--. 1 ntc ntc    8 Feb 14 15:52 router-8.cfg
-rw-rw-r--. 1 ntc ntc    8 Feb 14 15:52 router-7.cfg
-rw-rw-r--. 1 ntc ntc    8 Feb 14 15:52 router-9.cfg

```
]


---


class: ubuntu

# DRY - Using functions in bash


Allow repeated calls. 

```
#!/bin/bash

vlan_return () {
  multiplier=10
  vlan=$(($multiplier * $1))
  return $vlan
}


vlan_return 1
echo $?
vlan_return 33
echo $?
[ntc@ntc ~]$ 
```


---
# Lab Time

Lab 26  - Network functions scripting 
