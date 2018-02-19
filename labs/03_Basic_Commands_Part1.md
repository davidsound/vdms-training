
## Lab 3 - Viewing file contents, redirection and command chaining.

In this lab you will master basic commands to view files and understand powerful features of output redirection and command chaining in *nix.

> **NOTE: At any point, typing the command `clear` or pressing the key combination "CTRL-l" will clear the screen.**
> **NOTE: Using the `UP` and `DOWN` arrow allows you to cycle through the previous commands typed.**


### Task 1 - Viewing files

##### Step 1 

The `cat` command is a very versatile command that can be used to read and write files. Use the `cat` command to read the contents of the `/etc/passwd` file.


```
[ntc@ntc ~]$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:99:99:Nobody:/:/sbin/nologin
systemd-bus-proxy:x:999:997:systemd Bus Proxy:/:/sbin/nologin
systemd-network:x:192:192:systemd Network Management:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
polkitd:x:998:996:User for polkitd:/:/sbin/nologin
rpc:x:32:32:Rpcbind Daemon:/var/lib/rpcbind:/sbin/nologin
tss:x:59:59:Account used by the trousers package to sandbox the tcsd daemon:/dev/null:/sbin/nologin
rpcuser:x:29:29:RPC Service User:/var/lib/nfs:/sbin/nologin
nfsnobody:x:65534:65534:Anonymous NFS User:/var/lib/nfs:/sbin/nologin
postfix:x:89:89::/var/spool/postfix:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
chrony:x:997:995::/var/lib/chrony:/sbin/nologin
vagrant:x:1000:1000:vagrant:/home/vagrant:/bin/bash
ntc:x:1001:1001::/home/ntc:/bin/bash
[ntc@ntc ~]$ 

```

> Note: Your output may vary slightly.



##### Step 2

The `more` command allows the user to paginate the contents of a lengthy file. Use `more` to view the contents of `/etc/services`. When the output is displayed, you can use the `RETURN` key to proceed one line at a time or you can type the `SPACE` to view the output one page at a time (only in the forward direction). Use `q` to exit.

```
[ntc@ntc ~]$ more /etc/services 
# /etc/services:
# $Id: services,v 1.55 2013/04/14 ovasik Exp $
#
# Network services, Internet style
# IANA services version: last updated 2013-04-10
#
# Note that it is presently the policy of IANA to assign a single well-known
# port number for both TCP and UDP; hence, most entries here have two entries
# even if the protocol doesn't support UDP operations.
# Updated from RFC 1700, ``Assigned Numbers'' (October 1994).  Not all ports
# are included, only the more common ones.
#
# The latest IANA port assignments can be gotten from
#       http://www.iana.org/assignments/port-numbers
# The Well Known Ports are those from 0 through 1023.
# The Registered Ports are those from 1024 through 49151
# The Dynamic and/or Private Ports are those from 49152 through 65535
#
# Each line describes one service, and is of the form:
#
# service-name  port/protocol  [aliases ...]   [# comment]

tcpmux          1/tcp                           # TCP port service multiplexer
tcpmux          1/udp                           # TCP port service multiplexer
rje             5/tcp                           # Remote Job Entry
rje             5/udp                           # Remote Job Entry
echo            7/tcp
echo            7/udp
discard         9/tcp           sink null
discard         9/udp           sink null
systat          11/tcp          users

.
.
.
.
.
<output truncated for readability>
```

> Try viewing the contents of this file using the `cat` command you learned in the previous step.


##### Step 3

Similar to the `more` command, there exists a `less` command as well. The `less` command allows the user to view the output forwards and backwards. Use the `less` command to view the contents of `/etc/services`. 

> You can use the `PAGE-UP/DOWN` keys to view the contents of this file in either direction. Use `q` to exit.


```
[ntc@ntc ~]$ less /etc/services
[ntc@ntc ~]$ more /etc/services 
# /etc/services:
# $Id: services,v 1.55 2013/04/14 ovasik Exp $
#
# Network services, Internet style
# IANA services version: last updated 2013-04-10
#
# Note that it is presently the policy of IANA to assign a single well-known
# port number for both TCP and UDP; hence, most entries here have two entries
# even if the protocol doesn't support UDP operations.
# Updated from RFC 1700, ``Assigned Numbers'' (October 1994).  Not all ports
# are included, only the more common ones.
#
# The latest IANA port assignments can be gotten from
#       http://www.iana.org/assignments/port-numbers
# The Well Known Ports are those from 0 through 1023.
# The Registered Ports are those from 1024 through 49151
# The Dynamic and/or Private Ports are those from 49152 through 65535
#
# Each line describes one service, and is of the form:
#
# service-name  port/protocol  [aliases ...]   [# comment]

tcpmux          1/tcp                           # TCP port service multiplexer
tcpmux          1/udp                           # TCP port service multiplexer
rje             5/tcp                           # Remote Job Entry
rje             5/udp                           # Remote Job Entry
echo            7/tcp
echo            7/udp
discard         9/tcp           sink null
discard         9/udp           sink null
systat          11/tcp          users

.
.
.
.
.
<output truncated for readability>
```



##### Step 4

What happens if you try to use any of these commands against a directory? Try using `cat/less/more` to view the `dc_sites` directory you created in the previous lab:


```
[ntc@ntc ~]$ cat dc_sites/
cat: dc_sites/: Is a directory

```

```
[ntc@ntc ~]$ more dc_sites

*** dc_sites: directory ***

[ntc@ntc ~]$ 

```

```
[ntc@ntc ~]$ less dc_sites
total 0
drwxrwxr-x. 3 ntc ntc  21 Jan 24 17:56 ./
drwx------. 7 ntc ntc 197 Jan 24 17:53 ../
drwxrwxr-x. 3 ntc ntc  21 Jan 24 17:56 level-1/
dc_sites (END)

```

> Review the man pages for these commands



##### Step 5

In the previous step we learned that using the `cat` or `more` command helps identify a regular file vs a directory. There is a specific command in *nix that can be used to identify file type called `file`. Use the `file` command to test they file type of both `dc_sites` and `/etc/services`.

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


### Task 2 - Output redirection and command chaining

In *nix everything is a file. The standard output to the screen as we see it, is internally a redirection to the **stdout** file in *nix. We can take advantage of this fact to send the output of commands to either files or even other commands.


##### Step 1

Use the `echo` command to print "OSPF STANDARDS". Redirect this into a file called `ospf_config_guide.txt`. *nix uses the `>` and `<` symbols to redirect output.


```
[ntc@ntc ~]$ echo "OSPF STANDARDS"
OSPF STANDARDS
[ntc@ntc ~]$
```

```
[ntc@ntc ~]$ echo "OSPF STANDARDS" > ospf_config_guide.txt
[ntc@ntc ~]$ 
```

> Note: Observe that the output is not sent to the screen. It is instead, redirected to the file called `ospf_config_guide.txt`

```
[ntc@ntc ~]$ cat ospf_config_guide.txt 
OSPF STANDARDS
[ntc@ntc ~]$ 

```


##### Step 2

Create a directory called `test_redirection` in the ntc home directory

```
[ntc@ntc ~]$mkdir test_redirection
[ntc@ntc ~]$

```

##### Step 3

From the user home, redirect the output of the `ls -ltr` command to a file called `directroy_listing.txt` under the `test_redirection` directory.

```
[ntc@ntc ~]$ ls -ltr > test_redirection/directory_listing.txt 
[ntc@ntc ~]$ 
```


##### Step 4

Redirection also works for input. Redirect the contents of the file created in the previous step as an input to the `cat` command:


```
[ntc@ntc ~]$ cat < test_redirection/directory_listing.txt 
total 4
drwxrwxr-x. 3 ntc ntc 51 Jan 22 19:56 configs
drwxrwxr-x. 3 ntc ntc 45 Jan 23 17:02 VirtualBoxVMs
drwxrwxr-x. 3 ntc ntc 50 Jan 25 15:44 dc_sites
-rw-rw-r--. 1 ntc ntc 16 Jan 25 15:48 ospf_config_guide.txt
[ntc@ntc ~]$ 

```



##### Step 5

By default, the redirect will only redirect the **stdout** to a file. What if the command errored-out? How would you capture both the **stdout** and the **stderr** to different files? The pattern `>2` is used to capture the error output.

Capture the **stdout** and **stderr** while incorrectly executing `cat dc_sites` from the user home:



```
[ntc@ntc ~]$ cat test_redirection >output 2>error
[ntc@ntc ~]$ 

```

```

[ntc@ntc ~]$ cat error
cat: test_redirection: Is a directory
[ntc@ntc ~]$ 

```

```

[ntc@ntc ~]$ cat output
[ntc@ntc ~]$ 

```

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
drwxrwxr-x. 3 ntc ntc 50 Jan 25 16:44 test_redirection
-rw-rw-r--. 1 ntc ntc 16 Jan 25 15:48 ospf_config_guide.txt
[ntc@ntc ~]$ 
[ntc@ntc ~]$ 

```



##### Step 6

A common pattern  with redirection to send standard output and standard error to a single file is `_command_ > filename 2>&1 `. Here `1` indicates **stdout** and `2` indicates **stderr**.

Use this pattern to capture the standard output and error while executing `cat test_redirection test_redirection/directory_listing.txt`



```
[ntc@ntc ~]$ cat test_redirection test_redirection/directory_listing.txt >output  2>&1 
[ntc@ntc ~]$ 
```

```
[ntc@ntc ~]$ cat output 
cat: test_redirection: Is a directory
total 4
drwxrwxr-x. 3 ntc ntc 51 Jan 22 19:56 configs
drwxrwxr-x. 3 ntc ntc 45 Jan 23 17:02 VirtualBoxVMs
drwxrwxr-x. 3 ntc ntc 50 Jan 25 15:44 dc_sites
drwxrwxr-x. 3 ntc ntc 50 Jan 25 16:44 test_redirection
-rw-rw-r--. 1 ntc ntc 16 Jan 25 15:48 ospf_config_guide.txt
[ntc@ntc ~]$ 

```



##### Step 7

Whenever you use the `>` to redirect output, the latest output will overwrite existing data. Update the contents of `test_redirection/directory_listing.txt` to contain the directory listing of the `dc_sites/USA` subdirectory.

**BEFORE**

```
[ntc@ntc ~]$ cat test_redirection/directory_listing.txt 
total 4
drwxrwxr-x. 3 ntc ntc 51 Jan 22 19:56 configs
drwxrwxr-x. 3 ntc ntc 45 Jan 23 17:02 VirtualBoxVMs
drwxrwxr-x. 3 ntc ntc 50 Jan 25 15:44 dc_sites
drwxrwxr-x. 3 ntc ntc 50 Jan 25 16:44 test_redirection
-rw-rw-r--. 1 ntc ntc 16 Jan 25 15:48 ospf_config_guide.txt

```


```
[ntc@ntc ~]$ ls -ltr dc_sites/USA > test_redirection/directory_listing.txt 
[ntc@ntc ~]$ 
```

**AFTER**

```
[ntc@ntc ~]$ cat test_redirection/directory_listing.txt 
total 0
drwxrwxr-x. 3 ntc ntc 20 Jan 24 17:56 ATL
[ntc@ntc ~]$ 

```



##### Step 8

A commonly observed pattern is to use the `>` to "zero-out" a file by redirecting "null" to it. Go ahead and zero out the contents of `directory_listing.txt` without deleting the file:


```
[ntc@ntc ~]$ > test_redirection/directory_listing.txt 
[ntc@ntc ~]$ 

```

```

[ntc@ntc ~]$ cat test_redirection/directory_listing.txt 
[ntc@ntc ~]$ 

```


##### Step 9

The `>>` symbol is used to "concatenate" the output to an existing file. Use the `echo` command to add the following line to `ospf_config_guide.txt`: 

>"OSPF will be used as the standard IGP routing protocol"

```
[ntc@ntc ~]$ echo "OSPF will be used as the standard IGP routing protocol" >> ospf_config_guide.txt 
[ntc@ntc ~]$ 
```

```
[ntc@ntc ~]$ cat ospf_config_guide.txt 
OSPF STANDARDS
OSPF will be used as the standard IGP routing protocol
[ntc@ntc ~]$ 

```



### Task 3 - Using "pipes" to chain commands

##### Step 1 
"Pipe" denoted by the `|` symbol is used to send the output of one command as an input to another. Execute a `ls -l` on the `/usr/bin` directory and "pipe" the output to `more`.

```
[ntc@ntc ~]$ ls -l /usr/bin/ | more
total 72192
-rwxr-xr-x.   1 root root     41496 Nov  5  2016 [
-rwxr-xr-x.   1 root root    107856 Aug  2 17:46 a2p
-rwxr-xr-x.   1 root root     29112 Sep  6 16:47 addr2line
-rwxr-xr-x.   1 root root        29 Sep  6 16:25 alias
-rwxr-xr-x.   1 root root      7200 Jun 16  2016 animate
-rwxr-xr-x.   1 root root     70176 Jun  9  2014 applydeltarpm
lrwxrwxrwx.   1 root root         6 Jul  5  2017 apropos -> whatis
-rwxr-xr-x.   1 root root     62680 Sep  6 16:47 ar
-rwxr-xr-x.   1 root root     33080 Nov  5  2016 arch
-rwxr-xr-x.   1 root root    365344 Sep  6 16:47 as
-rwxr-xr-x.   1 root root     28808 Aug  2 16:09 aserver
-rwxr-xr-x.   1 root root     19872 Aug  4 21:29 aulast
-rwxr-xr-x.   1 root root     11544 Aug  4 21:29 aulastlog
-rwxr-xr-x.   1 root root     11376 Aug  4 21:29 ausyscall
-rwxr-xr-x.   1 root root     28184 Jun 10  2014 autotrace
-rwxr-xr-x.   1 root root     36808 Aug  4 21:29 auvirt
.
.
.
.
<output truncated for readability>


```

> This allows you to review all the files within the `/usr/bin` directory one page at a time.



##### Step 2

Pipes can be added in series. The output of one command can serve as the input for the next command whose output can then be used as an input for the next and so on...
Pipes can also be used in combination with redirection.

Write a combination of commands to count the number of files and directories in the `dc_sites` directory and write this to a file called `file_count.txt`. For this you will use the `wc -l` command to count the lines. Review the man pages for the `wc` command, which by default does a "word count" on the input.

```
[ntc@ntc ~]$ ls -l dc_sites/ | wc -l > file_count.txt
[ntc@ntc ~]$ 

```

```

[ntc@ntc ~]$ cat file_count.txt 
3
[ntc@ntc ~]$ 

```

> Break down each command and try them out separately. Does this number correctly reflect the number of files and directories within the `dc_sites` directory?




    
