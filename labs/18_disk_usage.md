## Lab 18 - Using the `du`, `df`, `find`, and `grep` Commands

In this lab you will learn how to search for files based on things such as name, size, age, and contents.  This could be useful if you are searching for a misplaced image file, a backup create after a certain date, or a configuration that contains a specific IP address. 



### Task 1 - Use the `df` command to investigate directory structure size

The `df` command is used to display information concerning the file systems avaible to the host.  The `df` command is useful for showing the total, used, and available size of file systems.

##### Step 1

Use the `df` command to show the stats of the filesystem(s) ont he `centos` host.

```bash
[ntc@ntc ~]$ pwd
/home/ntc
[ntc@ntc ~]$ df
Filesystem     1K-blocks    Used Available Use% Mounted on
/dev/sda2      104806400 3577204 101229196   4% /
devtmpfs         8118892       0   8118892   0% /dev
tmpfs            8133536       0   8133536   0% /dev/shm
tmpfs            8133536    8944   8124592   1% /run
tmpfs            8133536       0   8133536   0% /sys/fs/cgroup
/dev/sda5       94323716  145360  94178356   1% /home
/dev/sda1        5232640  230680   5001960   5% /boot
tmpfs            1626708      12   1626696   1% /run/user/42
tmpfs            1626708       0   1626708   0% /run/user/1001
```

This output is not very straightforward without knowing more about the `df` command and its output.  The default output of the `df` command is `kilobytes`.

##### Step 2

Run the `df` command using the `-h` option to make the output more _human readable_.

```bash
[ntc@ntc ~]$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2       100G  3.5G   97G   4% /
devtmpfs        7.8G     0  7.8G   0% /dev
tmpfs           7.8G     0  7.8G   0% /dev/shm
tmpfs           7.8G  8.8M  7.8G   1% /run
tmpfs           7.8G     0  7.8G   0% /sys/fs/cgroup
/dev/sda5        90G  142M   90G   1% /home
/dev/sda1       5.0G  226M  4.8G   5% /boot
tmpfs           1.6G   12K  1.6G   1% /run/user/42
tmpfs           1.6G     0  1.6G   0% /run/user/1001
```

With the `-h` option you will see that the output is much easier to interpret.  Focusing on the `/home` directory:

```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda5        90G  142M   90G   1% /home
```

We see that the filesystem `/dev/sda5` is mounted to the `/home` directory.  It has a configured size of 90 gigabytes and 142 megabytes are being used, which is only 1% of the allocated space.

This command is very useful when checking filesystem utilization and ensuring things, such as logs, downloads, etc, are being stored in the proper locations.










### Task 2 - Use the `du` command to investigate directory structure size

At times it can be beneficial to see the size of directories when investigating things such as storage hogs, or when searching for certain large files.  The `du` command reports the sizes of directory trees including all files within that tree.  That makes this command very useful during this process.

##### Step 1

Navigate to the `findme` directory within the `ntc` user home directory on the `centos` host.

```bash
[ntc@ntc ~]$ pwd
/home/ntc
[ntc@ntc ~]$ cd findme/
[ntc@ntc findme]$ 
```

##### Step 2

From the `~/findme/` directory run the `du` command to show the sizes of subdirectories within the `findme` directory.

```bash
[ntc@ntc findme]$ du
120     ./a/e/h
120     ./a/e/i
120     ./a/e/j
480     ./a/e
120     ./a/f/h
120     ./a/f/i
120     ./a/f/j
480     ./a/f
128     ./a/g/h
120     ./a/g/i
120     ./a/g/j
488     ./a/g
1568    ./a
120     ./b/e/h
120     ./b/e/i
102520  ./b/e/j
102880  ./b/e
120     ./b/f/h
120     ./b/f/i
120     ./b/f/j
480     ./b/f
120     ./b/g/h
120     ./b/g/i
120     ./b/g/j
480     ./b/g
103960  ./b
105528  .
```

This output is not very straightforward without knowing more about the `du` command and its output.  The default output of the `du` command is `kilobytes`.

##### Step 3

Run the `du` command using the `-h` option to make the output more _human readable_.

```bash
[ntc@ntc findme]$ du -h
120K    ./a/e/h
120K    ./a/e/i
120K    ./a/e/j
480K    ./a/e
120K    ./a/f/h
120K    ./a/f/i
120K    ./a/f/j
480K    ./a/f
128K    ./a/g/h
120K    ./a/g/i
120K    ./a/g/j
488K    ./a/g
1.6M    ./a
120K    ./b/e/h
120K    ./b/e/i
101M    ./b/e/j
101M    ./b/e
120K    ./b/f/h
120K    ./b/f/i
120K    ./b/f/j
480K    ./b/f
120K    ./b/g/h
120K    ./b/g/i
120K    ./b/g/j
480K    ./b/g
102M    ./b
104M    .
```

Now the output of the command provides more details around the the size ok each folder by displaying kilobytes (K), megabytes (M) and gigabytes (G).

##### Step 4

You can use the `-sh` option to only show a summary of the directory instead of the size of each individual subdirectory.

Run `du -sh` and investigate the output.

```bash
[ntc@ntc findme]$ du -sh
104M    .
```

##### Step 5

`du` can also be used to show the size of individual files.  You can include one or multiple files as arguments to the `du` command.

Use `du` to find the size of the file `./a/e/file.01` in the _human readable_ format.

```bash
[ntc@ntc findme]$ du -h ./a/e/file.01
12K     ./a/e/file.01
```

Now find the size of files `./a/file.01` and `./b/file.01`.

```bash
[ntc@ntc findme]$ du -h ./a/file.01 ./b/file.01
12K     ./a/file.01
12K     ./b/file.01
```

You can pass as many individual files as needed to `du` to find their size.

##### Step 6

In the same way you pass indiviual files to `du`, you can also pass individual directories.

Use `du` to find only the size of the directory `./a/e/` in the _human readable_ format.

> Remember, if you only want the size of a specific directory, and not all subdirectories, you must use the `-sh` option.

```bash
[ntc@ntc findme]$ du -h -sh ./a/e/
480K    ./a/e/
```

To find the size of any subdirectories run the `du` command without the `-sh` option.

```bash
[ntc@ntc findme]$ du -h ./a/e/
120K    ./a/e/h
120K    ./a/e/i
120K    ./a/e/j
480K    ./a/e/
```

##### Step 7

You can also pass multiple directories to `du`.

Find only the size of directories `./a/e/` and `./b/e/`.

```bash
[ntc@ntc findme]$ du -h -sh ./a/e/ ./b/e/
480K    ./a/e/
101M    ./b/e/
```

##### Step 8

Try mixing files and directories as arguments to `du`.

Find the sizes of `./a/f/`, `./b/f/file.01`, and `./b/g/j/file.01`.

```bash
[ntc@ntc findme]$ du -h -sh ./a/f/ ./b/f/file.01 ./b/g/j/file.01           
480K    ./a/f/
12K     ./b/f/file.01
12K     ./b/g/j/file.01
```










### Task 3 - Use the `du` command to locate a large file

Within the `findme` directory structure there is a file that is 100M.  Use the `du` commands we have covered to locate this file.

Scroll down for the answer.

```bash
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ 
[ntc@ntc findme]$ du -h
120K    ./a/e/h
120K    ./a/e/i
120K    ./a/e/j
480K    ./a/e
120K    ./a/f/h
120K    ./a/f/i
120K    ./a/f/j
480K    ./a/f
128K    ./a/g/h
120K    ./a/g/i
120K    ./a/g/j
488K    ./a/g
1.6M    ./a
120K    ./b/e/h
120K    ./b/e/i
101M    ./b/e/j
101M    ./b/e
120K    ./b/f/h
120K    ./b/f/i
120K    ./b/f/j
480K    ./b/f
120K    ./b/g/h
120K    ./b/g/i
120K    ./b/g/j
480K    ./b/g
102M    ./b
104M    .
[ntc@ntc findme]$ du -h ./b/e/j  
101M    ./b/e/j
[ntc@ntc findme]$ ls -ltr ./b/e/j
total 102520
-rw-rw-r--. 1 ntc ntc      9216 Feb  9 16:56 file.01
-rw-rw-r--. 1 ntc ntc      9216 Feb  9 16:56 file.02
-rw-rw-r--. 1 ntc ntc      9216 Feb  9 16:56 file.03
-rw-rw-r--. 1 ntc ntc      9216 Feb  9 16:56 file.04
-rw-rw-r--. 1 ntc ntc      9216 Feb  9 16:56 file.05
-rw-rw-r--. 1 ntc ntc      9216 Feb  9 16:56 file.06
-rw-rw-r--. 1 ntc ntc      9216 Feb  9 16:56 file.07
-rw-rw-r--. 1 ntc ntc      9216 Feb  9 16:56 file.08
-rw-rw-r--. 1 ntc ntc      9216 Feb  9 16:56 file.09
-rw-rw-r--. 1 ntc ntc      9216 Feb  9 16:56 file.10
-rw-r--r--. 1 ntc ntc 104857600 Feb  9 16:56 findmebig
[ntc@ntc findme]$ du -h ./b/e/j/findmebig
100M    ./b/e/j/findmebig
```









### Task 3 - Use the `find` command to locate files

The `find` command is useful for locating files based on multiple attributes such as name, size, or contents.  `find` is very flexible and can help locate files quickly.



##### Step 1

The basic structure to run `find` is the command `find`, followed by the location you want to search in, then an expression, followed by a definition for that expression.  Here is a basic example:

`find . -name file.01`

This command tells find to begin looking in the currenty directory for a file named `file.01`.  This will look for a file or files named `file.01` in the current directory and all subdirectories.

Run this command from the `home` directory of the `ntc` user on the `centos` host.

```bash
[ntc@ntc ~]$ pwd
/home/ntc
[ntc@ntc ~]$ find . -name file.01
./findme/a/e/h/file.01
./findme/a/e/i/file.01
./findme/a/e/j/file.01
./findme/a/e/file.01
./findme/a/f/h/file.01
./findme/a/f/i/file.01
./findme/a/f/j/file.01
./findme/a/f/file.01
./findme/a/g/h/file.01
./findme/a/g/i/file.01
./findme/a/g/j/file.01
./findme/a/g/file.01
./findme/a/file.01
./findme/b/e/h/file.01
./findme/b/e/i/file.01
./findme/b/e/j/file.01
./findme/b/e/file.01
./findme/b/f/h/file.01
./findme/b/f/i/file.01
./findme/b/f/j/file.01
./findme/b/f/file.01
./findme/b/g/h/file.01
./findme/b/g/i/file.01
./findme/b/g/j/file.01
./findme/b/g/file.01
./findme/b/file.01
```

As you see `find` was able to locate multiple files with the name `file.01`!

> The `-name` expression is case sensitive. You can use the `-iname` expression to ignore the case of a name.



##### Step 2

The `find` command can be used to locate files by size using the `-size` expression.

You can find files that are an exact size or larger/smaller than a given size.  Sizes are most commonly passed as `c` (bytes), `k` (kilobytes), `M` (Megabytes), `G` (Gigabytes).

If you would like to search for all files in the current directory that are over 50 megabytes you would use `find . -size +50M`.  To find files less than 50 megabytes you would just change the command to use `-50M`.  To only return files the are exactly 50 megabytes leave off the +/- sign.

Use the `find` command to locate the large file (`findmebig`) that you located using the `du` command is Task 2.

```bash
[ntc@ntc ~]$ find . -size 100M
./findme/b/e/j/findmebig
```

##### Step 3

The `find` command can also be used to find files based on time including the last time the file was accessed (`-atime`), the last time it was modified (`-mtime`), and the last time it was changed (`-ctime`).

These times can be searched in much the same way as sizes.  `+1` would mean that the action took place more than one day ago.  `-1` would mean the action took place less than 1 day ago.  And, `1` would mean that the action took place 1 day ago.

Let search for files in the current directory that were modified over 14 days ago.

```bash
[ntc@ntc ~]$ find . -mtime +14
./.mozilla
./.mozilla/extensions
./.mozilla/plugins
./.bash_logout
./.bash_profile
./.bashrc
./findme/a/f/i/findmeold
```









### Task 4 - Search contents of files using `grep`

`grep` is a very useful command within the *nix environment.  `grep` checks the provided input against a pattern that the user provides and reports matches.  These patterns can be as complex as the user needs to accomplish their goal.



##### Step 1

Using `grep`, perform a literal search of the `./configs/` directory looking for the word `interface`.  This is one of the basic use cases for `grep`.

```bash
[ntc@ntc ~]$ grep "interface" ./configs/*
./configs/atl-rtr01.cfg:interface GigabitEthernet1
./configs/atl-rtr01.cfg:interface GigabitEthernet2
./configs/atl-rtr01.cfg:interface GigabitEthernet3
./configs/atl-rtr01.cfg:interface GigabitEthernet4
./configs/atl-rtr02.cfg:interface GigabitEthernet1
./configs/atl-rtr02.cfg:interface GigabitEthernet2
./configs/atl-rtr02.cfg:interface GigabitEthernet3
./configs/atl-rtr02.cfg:interface GigabitEthernet4
./configs/atl-rtr03.cfg:interface GigabitEthernet1
./configs/atl-rtr03.cfg:interface GigabitEthernet2
./configs/atl-rtr03.cfg:interface GigabitEthernet3
./configs/atl-rtr03.cfg:interface GigabitEthernet4
./configs/atl-rtr04.cfg:interface GigabitEthernet1
./configs/atl-rtr04.cfg:interface GigabitEthernet2
./configs/atl-rtr04.cfg:interface GigabitEthernet3
./configs/atl-rtr04.cfg:interface GigabitEthernet4
./configs/atl-rtr05.cfg:interface GigabitEthernet1
./configs/atl-rtr05.cfg:interface GigabitEthernet2
./configs/atl-rtr05.cfg:interface GigabitEthernet3
./configs/atl-rtr05.cfg:interface GigabitEthernet4
./configs/nyc-rtr01.cfg:interface GigabitEthernet1
./configs/nyc-rtr01.cfg:interface GigabitEthernet2
./configs/nyc-rtr01.cfg:interface GigabitEthernet3
./configs/nyc-rtr01.cfg:interface GigabitEthernet4
./configs/nyc-rtr02.cfg:interface GigabitEthernet1
./configs/nyc-rtr02.cfg:interface GigabitEthernet2
./configs/nyc-rtr02.cfg:interface GigabitEthernet3
./configs/nyc-rtr02.cfg:interface GigabitEthernet4
./configs/nyc-rtr03.cfg:interface GigabitEthernet1
./configs/nyc-rtr03.cfg:interface GigabitEthernet2
./configs/nyc-rtr03.cfg:interface GigabitEthernet3
./configs/nyc-rtr03.cfg:interface GigabitEthernet4
./configs/nyc-rtr04.cfg:interface GigabitEthernet1
./configs/nyc-rtr04.cfg:interface GigabitEthernet2
./configs/nyc-rtr04.cfg:interface GigabitEthernet3
./configs/nyc-rtr04.cfg:interface GigabitEthernet4
./configs/nyc-rtr05.cfg:interface GigabitEthernet1
./configs/nyc-rtr05.cfg:interface GigabitEthernet2
./configs/nyc-rtr05.cfg:interface GigabitEthernet3
./configs/nyc-rtr05.cfg:interface GigabitEthernet4
```

As you can see, grep outputs every line from every file that is a match.


##### Step 2

It can often be more useful to know the line number of a match.  Use the `-n` option to show the line numbers.

```bash
[ntc@ntc ~]$ grep -n "interface" ./configs/*
./configs/atl-rtr01.cfg:150:interface GigabitEthernet1
./configs/atl-rtr01.cfg:155:interface GigabitEthernet2
./configs/atl-rtr01.cfg:160:interface GigabitEthernet3
./configs/atl-rtr01.cfg:165:interface GigabitEthernet4
./configs/atl-rtr02.cfg:150:interface GigabitEthernet1
./configs/atl-rtr02.cfg:155:interface GigabitEthernet2
./configs/atl-rtr02.cfg:160:interface GigabitEthernet3
./configs/atl-rtr02.cfg:165:interface GigabitEthernet4
./configs/atl-rtr03.cfg:150:interface GigabitEthernet1
./configs/atl-rtr03.cfg:155:interface GigabitEthernet2
./configs/atl-rtr03.cfg:160:interface GigabitEthernet3
./configs/atl-rtr03.cfg:165:interface GigabitEthernet4
./configs/atl-rtr04.cfg:150:interface GigabitEthernet1
./configs/atl-rtr04.cfg:155:interface GigabitEthernet2
./configs/atl-rtr04.cfg:160:interface GigabitEthernet3
./configs/atl-rtr04.cfg:165:interface GigabitEthernet4
./configs/atl-rtr05.cfg:150:interface GigabitEthernet1
./configs/atl-rtr05.cfg:155:interface GigabitEthernet2
./configs/atl-rtr05.cfg:160:interface GigabitEthernet3
./configs/atl-rtr05.cfg:165:interface GigabitEthernet4
./configs/nyc-rtr01.cfg:164:interface GigabitEthernet1
./configs/nyc-rtr01.cfg:169:interface GigabitEthernet2
./configs/nyc-rtr01.cfg:174:interface GigabitEthernet3
./configs/nyc-rtr01.cfg:179:interface GigabitEthernet4
./configs/nyc-rtr02.cfg:150:interface GigabitEthernet1
./configs/nyc-rtr02.cfg:155:interface GigabitEthernet2
./configs/nyc-rtr02.cfg:160:interface GigabitEthernet3
./configs/nyc-rtr02.cfg:165:interface GigabitEthernet4
./configs/nyc-rtr03.cfg:150:interface GigabitEthernet1
./configs/nyc-rtr03.cfg:155:interface GigabitEthernet2
./configs/nyc-rtr03.cfg:160:interface GigabitEthernet3
./configs/nyc-rtr03.cfg:165:interface GigabitEthernet4
./configs/nyc-rtr04.cfg:150:interface GigabitEthernet1
./configs/nyc-rtr04.cfg:155:interface GigabitEthernet2
./configs/nyc-rtr04.cfg:160:interface GigabitEthernet3
./configs/nyc-rtr04.cfg:165:interface GigabitEthernet4
./configs/nyc-rtr05.cfg:150:interface GigabitEthernet1
./configs/nyc-rtr05.cfg:155:interface GigabitEthernet2
./configs/nyc-rtr05.cfg:160:interface GigabitEthernet3
./configs/nyc-rtr05.cfg:165:interface GigabitEthernet4
```


##### Step 3

Now lets focus on a single file.  In the `./configs/atl-rtr01.cfg` search for all the instances of `gigabitethernet` with line number.

```bash
[ntc@ntc ~]$ grep -n "gigabitethernet" ./configs/atl-rtr01.cfg          
[ntc@ntc ~]$
```

> As you can see there was no output.  This is because `grep` is case sensitive.  Use the `-i` option to make the command case insensitive.

```bash
[ntc@ntc ~]$ grep -in "gigabitethernet" ./configs/atl-rtr01.cfg 
150:interface GigabitEthernet1
155:interface GigabitEthernet2
160:interface GigabitEthernet3
165:interface GigabitEthernet4
```

##### Step 4

Use REGEX to find instances of `gigabitethernet` or `route` within the same file (`./configs/atl-router01.cfg`) with line numbers.

> To perform this action you must use the `-E` option.  This allows for extended regular expressions and grouping.  This will use an example of grouping using parenthesis to outline the grouping.

> Don't forget to use the `-i` option as well.

```bash
[ntc@ntc ~]$ grep -inE "(route|gigabitethernet)" ./configs/atl-rtr01.cfg
150:interface GigabitEthernet1
155:interface GigabitEthernet2
160:interface GigabitEthernet3
165:interface GigabitEthernet4
178:ip route vrf MANAGEMENT 0.0.0.0 0.0.0.0 10.0.0.2
```

##### Step 5

Use REGEX to find all IP addresses within the same file (`./configs/atl-router01.cfg`) with line numbers.

```bash
[ntc@ntc ~]$ grep -nE "\b([0-9]{1,3}\.){3}[0-9]{1,3}\b" ./configs/atl-rtr01.cfg
152: ip address 10.0.0.51 255.255.255.0
178:ip route vrf MANAGEMENT 0.0.0.0 0.0.0.0 10.0.0.2
```

> As you can see, REGEX can be very powerful.  This REGEX only matches the format of an IP addresses and does not validate that an IP address is correct.  For example, this REGEX pattern would match `999.999.999.999`.
