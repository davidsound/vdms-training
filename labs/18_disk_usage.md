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

Navigate to the `backup_dir` directory within the `ntc` user home directory on the `centos` host.

```bash
[ntc@ntc ~]$ pwd
/home/ntc
[ntc@ntc ~]$ cd backup_dir/
[ntc@ntc backup_dir]$ 
```

##### Step 2

From the `~/backup_dir/` directory run the `du` command to show the sizes of subdirectories within the `backup_dir` directory.

```bash
[ntc@ntc backup_dir]$ du
484     ./january/week1/monday
484     ./january/week1/tuesday
484     ./january/week1/wednesday
484     ./january/week1/thursday
484     ./january/week1/friday
484     ./january/week1/saturday
484     ./january/week1/sunday
3388    ./january/week1
484     ./january/week2/monday
484     ./january/week2/tuesday
484     ./january/week2/wednesday
484     ./january/week2/thursday
484     ./january/week2/friday
484     ./january/week2/saturday
484     ./january/week2/sunday
3388    ./january/week2
484     ./january/week3/monday
484     ./january/week3/tuesday
484     ./january/week3/wednesday
484     ./january/week3/thursday
484     ./january/week3/friday
484     ./january/week3/saturday
484     ./january/week3/sunday
3388    ./january/week3
484     ./january/week4/monday
484     ./january/week4/tuesday
484     ./january/week4/wednesday
484     ./january/week4/thursday
484     ./january/week4/friday
484     ./january/week4/saturday
484     ./january/week4/sunday
3388    ./january/week4
13552   ./january
484     ./february/week1/monday
484     ./february/week1/tuesday
484     ./february/week1/wednesday
484     ./february/week1/thursday
484     ./february/week1/friday
484     ./february/week1/saturday
484     ./february/week1/sunday
3388    ./february/week1
484     ./february/week2/monday
484     ./february/week2/tuesday
484     ./february/week2/wednesday
102884  ./february/week2/thursday
484     ./february/week2/friday
484     ./february/week2/saturday
484     ./february/week2/sunday
105788  ./february/week2
484     ./february/week3/monday
484     ./february/week3/tuesday
484     ./february/week3/wednesday
484     ./february/week3/thursday
484     ./february/week3/friday
484     ./february/week3/saturday
484     ./february/week3/sunday
3388    ./february/week3
484     ./february/week4/monday
484     ./february/week4/tuesday
484     ./february/week4/wednesday
484     ./february/week4/thursday
484     ./february/week4/friday
484     ./february/week4/saturday
484     ./february/week4/sunday
3388    ./february/week4
115952  ./february
129504  .
```

This output is not very straightforward without knowing more about the `du` command and its output.  The default output of the `du` command is `kilobytes`.

##### Step 3

Run the `du` command using the `-h` option to make the output more _human readable_.

```bash
[ntc@ntc backup_dir]$ du -h
484K    ./january/week1/monday
484K    ./january/week1/tuesday
484K    ./january/week1/wednesday
484K    ./january/week1/thursday
484K    ./january/week1/friday
484K    ./january/week1/saturday
484K    ./january/week1/sunday
3.4M    ./january/week1
484K    ./january/week2/monday
484K    ./january/week2/tuesday
484K    ./january/week2/wednesday
484K    ./january/week2/thursday
484K    ./january/week2/friday
484K    ./january/week2/saturday
484K    ./january/week2/sunday
3.4M    ./january/week2
484K    ./january/week3/monday
484K    ./january/week3/tuesday
484K    ./january/week3/wednesday
484K    ./january/week3/thursday
484K    ./january/week3/friday
484K    ./january/week3/saturday
484K    ./january/week3/sunday
3.4M    ./january/week3
484K    ./january/week4/monday
484K    ./january/week4/tuesday
484K    ./january/week4/wednesday
484K    ./january/week4/thursday
484K    ./january/week4/friday
484K    ./january/week4/saturday
484K    ./january/week4/sunday
3.4M    ./january/week4
14M     ./january
484K    ./february/week1/monday
484K    ./february/week1/tuesday
484K    ./february/week1/wednesday
484K    ./february/week1/thursday
484K    ./february/week1/friday
484K    ./february/week1/saturday
484K    ./february/week1/sunday
3.4M    ./february/week1
484K    ./february/week2/monday
484K    ./february/week2/tuesday
484K    ./february/week2/wednesday
101M    ./february/week2/thursday
484K    ./february/week2/friday
484K    ./february/week2/saturday
484K    ./february/week2/sunday
104M    ./february/week2
484K    ./february/week3/monday
484K    ./february/week3/tuesday
484K    ./february/week3/wednesday
484K    ./february/week3/thursday
484K    ./february/week3/friday
484K    ./february/week3/saturday
484K    ./february/week3/sunday
3.4M    ./february/week3
484K    ./february/week4/monday
484K    ./february/week4/tuesday
484K    ./february/week4/wednesday
484K    ./february/week4/thursday
484K    ./february/week4/friday
484K    ./february/week4/saturday
484K    ./february/week4/sunday
3.4M    ./february/week4
114M    ./february
127M    .
```

Now the output of the command provides more details around the the size ok each folder by displaying kilobytes (K), megabytes (M) and gigabytes (G).

##### Step 4

You can use the `-sh` option to only show a summary of the directory instead of the size of each individual subdirectory.

Run `du -sh` and investigate the output.

```bash
[ntc@ntc backup_dir]$ du -sh
127M    .
```

##### Step 5

`du` can also be used to show the size of individual files.  You can include one or multiple files as arguments to the `du` command.

Use `du` to find the size of the file `./january/week2/monday/config_backup.01` in the _human readable_ format.

```bash
[ntc@ntc backup_dir]$ du -h ./january/week2/monday/config_backup.01
20K     ./january/week2/monday/config_backup.01
```

Now find the size of files `./january/week1/monday/config_backup.01` and `./february/week1/monday/config_backup.01`.

```bash
[ntc@ntc backup_dir]$ du -h ./january/week1/monday/config_backup.01 ./february/week1/monday/config_backup.01
20K     ./january/week1/monday/config_backup.01
20K     ./february/week1/monday/config_backup.01
```

You can pass as many individual files as needed to `du` to find their size.

##### Step 6

In the same way you pass indiviual files to `du`, you can also pass individual directories.

Use `du` to find only the size of the directory `./january/week4/` in the _human readable_ format.

> Remember, if you only want the size of a specific directory, and not all subdirectories, you must use the `-sh` option.

```bash
[ntc@ntc backup_dir]$ du -h -sh ./january/week4/
3.4M    ./january/week4/
```

To find the size of any subdirectories run the `du` command without the `-sh` option.

```bash
[ntc@ntc backup_dir]$ du -h ./january/week4/
484K    ./january/week4/monday
484K    ./january/week4/tuesday
484K    ./january/week4/wednesday
484K    ./january/week4/thursday
484K    ./january/week4/friday
484K    ./january/week4/saturday
484K    ./january/week4/sunday
3.4M    ./january/week4/
```

##### Step 7

You can also pass multiple directories to `du`.

Find only the size of directories `./january/week4/` and `./february/week4/`.

```bash
[ntc@ntc backup_dir]$ du -h -sh ./january/week4/ ./february/week4/
3.4M    ./january/week4/
3.4M    ./february/week4/
```

##### Step 8

Try mixing files and directories as arguments to `du`.

Find the sizes of `./january/week4/`, `./february/week1/monday/config_backup.01`, and `./january/week3/friday/config_backup.01`.

```bash
[ntc@ntc backup_dir]$ du -h -sh ./january/week4/ ./february/week1/monday/config_backup.01 ./january/week3/friday/config_backup.01
3.4M    ./january/week4/
20K     ./february/week1/monday/config_backup.01
20K     ./january/week3/friday/config_backup.01
```










### Task 3 - Use the `du` command to locate a large file

Within the `backup_dir` directory structure there is a file that is 100M.  Use the `du` commands we have covered to locate this file.

Scroll down for the answer.

```bash
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ 
[ntc@ntc backup_dir]$ du -h
[ntc@ntc backup_dir]$ du -h
484K    ./january/week1/monday
484K    ./january/week1/tuesday
484K    ./january/week1/wednesday
484K    ./january/week1/thursday
484K    ./january/week1/friday
484K    ./january/week1/saturday
484K    ./january/week1/sunday
3.4M    ./january/week1
484K    ./january/week2/monday
484K    ./january/week2/tuesday
484K    ./january/week2/wednesday
484K    ./january/week2/thursday
484K    ./january/week2/friday
484K    ./january/week2/saturday
484K    ./january/week2/sunday
3.4M    ./january/week2
484K    ./january/week3/monday
484K    ./january/week3/tuesday
484K    ./january/week3/wednesday
484K    ./january/week3/thursday
484K    ./january/week3/friday
484K    ./january/week3/saturday
484K    ./january/week3/sunday
3.4M    ./january/week3
484K    ./january/week4/monday
484K    ./january/week4/tuesday
484K    ./january/week4/wednesday
484K    ./january/week4/thursday
484K    ./january/week4/friday
484K    ./january/week4/saturday
484K    ./january/week4/sunday
3.4M    ./january/week4
14M     ./january
484K    ./february/week1/monday
484K    ./february/week1/tuesday
484K    ./february/week1/wednesday
484K    ./february/week1/thursday
484K    ./february/week1/friday
484K    ./february/week1/saturday
484K    ./february/week1/sunday
3.4M    ./february/week1
484K    ./february/week2/monday
484K    ./february/week2/tuesday
484K    ./february/week2/wednesday
101M    ./february/week2/thursday
484K    ./february/week2/friday
484K    ./february/week2/saturday
484K    ./february/week2/sunday
104M    ./february/week2
484K    ./february/week3/monday
484K    ./february/week3/tuesday
484K    ./february/week3/wednesday
484K    ./february/week3/thursday
484K    ./february/week3/friday
484K    ./february/week3/saturday
484K    ./february/week3/sunday
3.4M    ./february/week3
484K    ./february/week4/monday
484K    ./february/week4/tuesday
484K    ./february/week4/wednesday
484K    ./february/week4/thursday
484K    ./february/week4/friday
484K    ./february/week4/saturday
484K    ./february/week4/sunday
3.4M    ./february/week4
114M    ./february
127M    .
[ntc@ntc backup_dir]$ du -h ./february/week2/thursday
101M    ./february/week2/thursday
[ntc@ntc backup_dir]$ ls -ltr ./february/week2/thursday
[ntc@ntc backup_dir]$ ls -ltr ./february/week2/thursday
total 102880
-rw-rw-r--. 1 ntc ntc     18432 Feb 18 01:06 config_backup.01
-rw-rw-r--. 1 ntc ntc     18432 Feb 18 01:06 config_backup.02
-rw-rw-r--. 1 ntc ntc     18432 Feb 18 01:06 config_backup.03
-rw-rw-r--. 1 ntc ntc     18432 Feb 18 01:06 config_backup.04
-rw-rw-r--. 1 ntc ntc     18432 Feb 18 01:06 config_backup.05
-rw-rw-r--. 1 ntc ntc     18432 Feb 18 01:06 config_backup.06
-rw-rw-r--. 1 ntc ntc     18432 Feb 18 01:06 config_backup.07
-rw-rw-r--. 1 ntc ntc     18432 Feb 18 01:06 config_backup.08
-rw-rw-r--. 1 ntc ntc     18432 Feb 18 01:06 config_backup.09
-rw-rw-r--. 1 ntc ntc     18432 Feb 18 01:06 config_backup.10
-rw-rw-r--. 1 ntc ntc     18432 Feb 18 01:06 config_backup.11
-rw-rw-r--. 1 ntc ntc     18432 Feb 18 01:06 config_backup.12
-rw-rw-r--. 1 ntc ntc     18432 Feb 18 01:06 config_backup.13
-rw-rw-r--. 1 ntc ntc     18432 Feb 18 01:06 config_backup.14
-rw-rw-r--. 1 ntc ntc     18432 Feb 18 01:06 config_backup.15
-rw-rw-r--. 1 ntc ntc     18432 Feb 18 01:06 config_backup.16
-rw-rw-r--. 1 ntc ntc     18432 Feb 18 01:06 config_backup.17
-rw-rw-r--. 1 ntc ntc     18432 Feb 18 01:06 config_backup.18
-rw-rw-r--. 1 ntc ntc     18432 Feb 18 01:06 config_backup.19
-rw-rw-r--. 1 ntc ntc     18432 Feb 18 01:06 config_backup.20
-rw-rw-r--. 1 ntc ntc     18432 Feb 18 01:06 config_backup.21
-rw-rw-r--. 1 ntc ntc     18432 Feb 18 01:06 config_backup.22
-rw-rw-r--. 1 ntc ntc     18432 Feb 18 01:06 config_backup.23
-rw-rw-r--. 1 ntc ntc     18432 Feb 18 01:06 config_backup.24
-rw-r--r--. 1 ntc ntc 104857600 Feb 18 01:07 largeimg
[ntc@ntc backup_dir]$ du -h ./february/week2/thursday/largeimg
100M    ./february/week2/thursday/largeimg
```









### Task 3 - Use the `find` command to locate files

The `find` command is useful for locating files based on multiple attributes such as name, size, or contents.  `find` is very flexible and can help locate files quickly.



##### Step 1

The basic structure to run `find` is the command `find`, followed by the location you want to search in, then an expression, followed by a definition for that expression.  Here is a basic example:

`find . -name config_backup.01`

This command tells find to begin looking in the currenty directory for a file named `config_backup.01`.  This will look for a file or files named `config_backup.01` in the current directory and all subdirectories.

Run this command from the `home` directory of the `ntc` user on the `centos` host.

```bash
[ntc@ntc ~]$ pwd
/home/ntc
[ntc@ntc ~]$ find . -name config_backup.01
./backup_dir/january/week1/monday/config_backup.01
./backup_dir/january/week1/tuesday/config_backup.01
./backup_dir/january/week1/wednesday/config_backup.01
./backup_dir/january/week1/thursday/config_backup.01
./backup_dir/january/week1/friday/config_backup.01
./backup_dir/january/week1/saturday/config_backup.01
./backup_dir/january/week1/sunday/config_backup.01
./backup_dir/january/week2/monday/config_backup.01
./backup_dir/january/week2/tuesday/config_backup.01
./backup_dir/january/week2/wednesday/config_backup.01
./backup_dir/january/week2/thursday/config_backup.01
./backup_dir/january/week2/friday/config_backup.01
./backup_dir/january/week2/saturday/config_backup.01
./backup_dir/january/week2/sunday/config_backup.01
./backup_dir/january/week3/monday/config_backup.01
./backup_dir/january/week3/tuesday/config_backup.01
./backup_dir/january/week3/wednesday/config_backup.01
./backup_dir/january/week3/thursday/config_backup.01
./backup_dir/january/week3/friday/config_backup.01
./backup_dir/january/week3/saturday/config_backup.01
./backup_dir/january/week3/sunday/config_backup.01
./backup_dir/january/week4/monday/config_backup.01
./backup_dir/january/week4/tuesday/config_backup.01
./backup_dir/january/week4/wednesday/config_backup.01
./backup_dir/january/week4/thursday/config_backup.01
./backup_dir/january/week4/friday/config_backup.01
./backup_dir/january/week4/saturday/config_backup.01
./backup_dir/january/week4/sunday/config_backup.01
./backup_dir/february/week1/monday/config_backup.01
./backup_dir/february/week1/tuesday/config_backup.01
./backup_dir/february/week1/wednesday/config_backup.01
./backup_dir/february/week1/thursday/config_backup.01
./backup_dir/february/week1/friday/config_backup.01
./backup_dir/february/week1/saturday/config_backup.01
./backup_dir/february/week1/sunday/config_backup.01
./backup_dir/february/week2/monday/config_backup.01
./backup_dir/february/week2/tuesday/config_backup.01
./backup_dir/february/week2/wednesday/config_backup.01
./backup_dir/february/week2/thursday/config_backup.01
./backup_dir/february/week2/friday/config_backup.01
./backup_dir/february/week2/saturday/config_backup.01
./backup_dir/february/week2/sunday/config_backup.01
./backup_dir/february/week3/monday/config_backup.01
./backup_dir/february/week3/tuesday/config_backup.01
./backup_dir/february/week3/wednesday/config_backup.01
./backup_dir/february/week3/thursday/config_backup.01
./backup_dir/february/week3/friday/config_backup.01
./backup_dir/february/week3/saturday/config_backup.01
./backup_dir/february/week3/sunday/config_backup.01
./backup_dir/february/week4/monday/config_backup.01
./backup_dir/february/week4/tuesday/config_backup.01
./backup_dir/february/week4/wednesday/config_backup.01
./backup_dir/february/week4/thursday/config_backup.01
./backup_dir/february/week4/friday/config_backup.01
./backup_dir/february/week4/saturday/config_backup.01
./backup_dir/february/week4/sunday/config_backup.01
```

As you see `find` was able to locate multiple files with the name `config_backup.01`!

> The `-name` expression is case sensitive. You can use the `-iname` expression to ignore the case of a name.



##### Step 2

The `find` command can be used to locate files by size using the `-size` expression.

You can find files that are an exact size or larger/smaller than a given size.  Sizes are most commonly passed as `c` (bytes), `k` (kilobytes), `M` (Megabytes), `G` (Gigabytes).

If you would like to search for all files in the current directory that are over 50 megabytes you would use `find . -size +50M`.  To find files less than 50 megabytes you would just change the command to use `-50M`.  To only return files the are exactly 50 megabytes leave off the +/- sign.

Use the `find` command to locate the large file (`backup_dirbig`) that you located using the `du` command is Task 2.

```bash
[ntc@ntc ~]$ find . -size 100M
./backup_dir/february/week2/thursday/largeimg
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
./.bashrc
./backup_dir/january/week4/sunday/oldconfig
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
