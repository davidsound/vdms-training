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
# The Linux directory layout

*/bin*: Binaries for system operations
*/sbin*: Binaries that requires "Superuser" privileges
*/lib*: System libraries
*/dev*: Hardware devices
*/opt*: Optional, 3rd Party applications
*/home*: User home directories
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
[ntc@ntc ~]$ mkdir tux

[ntc@ntc ~]$ ls -ltr
total 0
drwxrwxr-x. 3 ntc ntc 51 Jan 22 19:56 configs
drwxrwxr-x. 3 ntc ntc 45 Jan 23 17:02 VirtualBoxVMs
drwxrwxr-x. 2 ntc ntc  6 Jan 24 17:53 tux
[ntc@ntc ~]$ 


```


```
[ntc@ntc ~]$ mkdir -p tux/level-1/level-2/level-3

```

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
[ntc@ntc ~]$ file tux
tux: directory
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
[ntc@ntc ~]$ echo "Hello, World :)" > greetings.txt
[ntc@ntc ~]$ 

```
>Note: Observe that the output is not sent to the screen. It is instead, redirected to the file called greetings.txt

```

[ntc@ntc ~]$ cat greetings.txt 
Hello, World :)
[ntc@ntc ~]$ 


```


---
class: ubuntu

# Redirecting output(Contd.)

.left-column[
Redirect output and errors into separate files

```

[ntc@ntc ~]$ cat tux/directory_listing.txt > output 2>error
[ntc@ntc ~]$ 
[ntc@ntc ~]$ cat error
[ntc@ntc ~]$ 
[ntc@ntc ~]$ cat output
total 4
drwxrwxr-x. 3 ntc ntc 51 Jan 22 19:56 configs
drwxrwxr-x. 3 ntc ntc 45 Jan 23 17:02 VirtualBoxVMs
drwxrwxr-x. 3 ntc ntc 50 Jan 25 15:44 tux
-rw-rw-r--. 1 ntc ntc 16 Jan 25 15:48 greetings.txt
[ntc@ntc ~]$ 
[ntc@ntc ~]$ 
```

]

.right-column[
Redirect output and errors into the same file:

```
[ntc@ntc ~]$ cat tux tux/directory_listing.txt >output  2>&1 
[ntc@ntc ~]$ 

[ntc@ntc ~]$ cat output 
cat: tux: Is a directory
total 4
drwxrwxr-x. 3 ntc ntc 51 Jan 22 19:56 configs
drwxrwxr-x. 3 ntc ntc 45 Jan 23 17:02 VirtualBoxVMs
drwxrwxr-x. 3 ntc ntc 50 Jan 25 15:44 tux
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
[ntc@ntc ~]$ ls -l tux/ | wc -l > file_count.txt
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
[ntc@ntc ~]$ cp -r tux tux-backup
[ntc@ntc ~]$
```

```
[ntc@ntc ~]$ mv nyc-rtr01_backup.cfg nyc-rtr01.cfg

```

```
[ntc@ntc ~]$ rm -r tux-backup
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

3. Type `se nu`; short for "set numbers" to enable line numbers.


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

---
class: ubuntu

# Opening multiple files in split screens

- Use `sp` for horizontal split or `vsp` for vertical

<img src="slides/media/vim/vim4.png" alt="Pep8" style="align: right:;width:800px;height:350px;">

```
Use `CTRL-ww` to jump between the split

```


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
- Stores user customizations and  placed in the `home directory`
- Not automatically generated
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

- Lab 12 - Aliases
- Lab 13 - SSH Configuration



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

class: center, middle, segue

# System Administration

---

class: ubuntu

# System Identification

Release information is in `/etc/redhat-release`

```
[ntc@ntc ~]$ cat /etc/redhat-release 
CentOS Linux release 7.4.1708 (Core)
```

For more details:

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

---

class: ubuntu
# System Information(Contd.)

Using `hostnamectl`

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

`hostnamectl` can also be used to update the hostname of the server

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

---

class: ubuntu
# Health check using `vmstat` and `free`

.left-column[
`vmstat` reports information about processes, memory, paging, block IO, traps, disks and cpu activity.

```
[ntc@ntc ~]$ vmstat
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 1  0      0 3489668   2108 283628    0    0     2     1    9    8  0  0 100  0  0

```

]

.right-column[
`free`  displays  the total amount of free and used physical and swap memory in the system, as well as the buffers and caches used by the kernel. The information is gathered by parsing /proc/meminfo. 

```
[ntc@ntc ~]$ free -h
              total        used        free      shared  buff/cache   available
Mem:           3.7G        103M        3.3G        8.3M        279M        3.4G
Swap:          1.5G          0B        1.5G
```


]

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
# The process ID

Use `ps -ef` to list current running processes

.left-column[
```
[ntc@ntc ~]$ ps -ef | more
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 06:42 ?        00:00:16 /usr/lib/systemd/systemd --switched-root --system --deserialize 21
root         2     0  0 06:42 ?        00:00:00 [kthreadd]
root         3     2  0 06:42 ?        00:00:00 [ksoftirqd/0]
root         5     2  0 06:42 ?        00:00:00 [kworker/0:0H]
root         6     2  0 06:42 ?        00:00:00 [kworker/u16:0]
root         7     2  0 06:42 ?        00:00:00 [migration/0]
root         8     2  0 06:42 ?        00:00:00 [rcu_bh]
root         9     2  0 06:42 ?        00:00:01 [rcu_sched]
root        10     2  0 06:42 ?        00:00:00 [watchdog/0]
root        11     2  0 06:42 ?        00:00:00 [watchdog/1

.
.
.
.
.
<output truncated for readability>
```
]

.right-column[
Extensive command with many options
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
# Lab Time

Lab 15 - System health
Lab 16 - Process management

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
# Disk usage(Contd.)

The `du` command reports the sizes of directory trees including all files within that tree.

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


