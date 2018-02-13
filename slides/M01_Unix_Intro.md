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


---


