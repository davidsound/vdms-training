## Lab 4 - Copying, moving and deleting files

In this lab you will learn how to copy files and directories, move them permanently and how to delete them.

> **NOTE: At any point, typing the command `clear` or pressing the key combination "CTRL-l" will clear the screen.**
> **NOTE: Using the `UP` and `DOWN` arrow allows you to cycle through the previous commands typed.**


### Task 1 - Copying files and directories

##### Step 1 

Copy the `greetings.txt` file from the home directory into `tux` and every sub-directory within.


```
[ntc@ntc ~]$ cp greetings.txt tux/
[ntc@ntc ~]$ 

```

```

[ntc@ntc ~]$ cp greetings.txt tux/level-1/
[ntc@ntc ~]$ 

```

```

[ntc@ntc ~]$ cp greetings.txt tux/level-1/level-2/
[ntc@ntc ~]$ 

```

```

[ntc@ntc ~]$ cp greetings.txt tux/level-1/level-2/level-3/
[ntc@ntc ~]$ 

```

```

[ntc@ntc ~]$ tree tux
tux
├── directory_listing.txt
├── greetings.txt
└── level-1
    ├── greetings.txt
    └── level-2
        ├── greetings.txt
        └── level-3
            └── greetings.txt

3 directories, 5 files
[ntc@ntc ~]$ 

```


##### Step 2

Try using the `cp` command to make a backup of the `tux` directory. Call it `tux-backup`.

```
[ntc@ntc ~]$ cp tux tux-backup
cp: omitting directory ‘tux’
[ntc@ntc ~]$ 
```
While copying a directory, you have to use the `-r` flag to instruct the shell to copy the contents of the directories "recursively". Go ahead and make the backup using the `cp -r` command:

```
[ntc@ntc ~]$ cp -r tux tux-backup
[ntc@ntc ~]$ 

```

```
[ntc@ntc ~]$ ls -ltr
total 12
drwxrwxr-x. 3 ntc ntc  51 Jan 22 19:56 configs
drwxrwxr-x. 3 ntc ntc  45 Jan 23 17:02 VirtualBoxVMs
-rw-rw-r--. 1 ntc ntc   0 Jan 25 16:01 test
-rw-rw-r--. 1 ntc ntc   0 Jan 25 16:06 error
-rw-rw-r--. 1 ntc ntc 225 Jan 25 16:11 output
-rw-rw-r--. 1 ntc ntc  92 Jan 25 17:20 greetings.txt
-rw-rw-r--. 1 ntc ntc   2 Jan 25 18:00 file_count.txt
drwxrwxr-x. 3 ntc ntc  71 Jan 25 18:09 tux
drwxrwxr-x. 3 ntc ntc  71 Jan 26 10:44 tux-backup

```


##### Step 3
Rename the `greetings.txt` file within all the subdirectories of `tux` to the format `$directoryName-greetings.txt`. Use the `mv` command to accomplish this.

```
[ntc@ntc ~]$ cd tux
[ntc@ntc tux]$ ls
directory_listing.txt  greetings.txt  level-1
[ntc@ntc tux]$ mv greetings.txt tux-greetings.txt
[ntc@ntc tux]$ 

```

```

[ntc@ntc tux]$ mv level-1/greetings.txt level-1/level-1-greetings.txt
[ntc@ntc tux]$ 

```

```

[ntc@ntc tux]$ mv level-1/level-2/greetings.txt level-1/level-2/level-2-greetings.txt
[ntc@ntc tux]$ 

```

```

[ntc@ntc tux]$ mv level-1/level-2/level-3/greetings.txt level-1/level-2/level-3/level-3-greetings.txt
[ntc@ntc tux]$ 

```



```
[ntc@ntc tux]$ tree
.
├── directory_listing.txt
├── level-1
│   ├── level-1-greetings.txt
│   └── level-2
│       ├── level-2-greetings.txt
│       └── level-3
│           └── level-3-greetings.txt
└── tux-greetings.txt

3 directories, 5 files
[ntc@ntc tux]$ 
```


##### Step 4

Use the `rm` command to delete the `$directoryName-greetings.txt` file from each subdirectory of `tux`.

```
[ntc@ntc tux]$ pwd
/home/ntc/tux
[ntc@ntc tux]$ rm tux-greetings.txt 
[ntc@ntc tux]$ 
[ntc@ntc tux]$ rm level-1/level-1-greetings.txt 
[ntc@ntc tux]$ 
[ntc@ntc tux]$ rm level-1/level-2/level-2-greetings.txt 
[ntc@ntc tux]$ 
[ntc@ntc tux]$ rm level-1/level-2/level-3/level-3-greetings.txt 
[ntc@ntc tux]$ 

```

> On some implementations you might be prompted with a "Are you sure you want to delete this file" prompt.


```
[ntc@ntc tux]$ tree
.
├── directory_listing.txt
└── level-1
    └── level-2
        └── level-3

3 directories, 1 file
[ntc@ntc tux]$ 

```

##### Step 5

Similar to the recursive copy, use the `-r` flag with the `rm` command to remove the `tux-backup` directory and all files within it.

```
[ntc@ntc ~]$ rm -r tux-backup
[ntc@ntc ~]$ ls -ltr
total 12
drwxrwxr-x. 3 ntc ntc  51 Jan 22 19:56 configs
drwxrwxr-x. 3 ntc ntc  45 Jan 23 17:02 VirtualBoxVMs
-rw-rw-r--. 1 ntc ntc   0 Jan 25 16:01 test
-rw-rw-r--. 1 ntc ntc   0 Jan 25 16:06 error
-rw-rw-r--. 1 ntc ntc 225 Jan 25 16:11 output
-rw-rw-r--. 1 ntc ntc  92 Jan 25 17:20 greetings.txt
-rw-rw-r--. 1 ntc ntc   2 Jan 25 18:00 file_count.txt
drwxrwxr-x. 3 ntc ntc  50 Jan 26 11:03 tux
[ntc@ntc ~]$ 

```

> A commonly used pattern is the `rm -fr` command. The `f` will forcibly remove all files and recursively, in combination with `r`. 

This is a very powerful command and needs careful consideration before using. [rm -fr](https://media2.giphy.com/media/7cxkulE62EV2/giphy.gif)

