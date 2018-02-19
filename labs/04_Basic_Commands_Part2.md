## Lab 4 - Copying, moving and deleting files

In this lab you will learn how to copy files and directories, move them permanently and how to delete them.

> **NOTE: At any point, typing the command `clear` or pressing the key combination "CTRL-l" will clear the screen.**
> **NOTE: Using the `UP` and `DOWN` arrow allows you to cycle through the previous commands typed.**


### Task 1 - Copying files and directories

##### Step 1 

Copy the `ospf_config_guide.txt` file from the home directory into `dc_sites` and every sub-directory within.


```
[ntc@ntc ~]$ cp ospf_config_guide.txt dc_sites/
[ntc@ntc ~]$ 

```

```

[ntc@ntc ~]$ cp ospf_config_guide.txt dc_sites/USA/
[ntc@ntc ~]$ 

```

```

[ntc@ntc ~]$ cp ospf_config_guide.txt dc_sites/USA/ATL/
[ntc@ntc ~]$ 

```

```

[ntc@ntc ~]$ cp ospf_config_guide.txt dc_sites/USA/ATL/alpha_dc/
[ntc@ntc ~]$ 

```

```

[ntc@ntc ~]$ tree dc_sites
dc_sites
├── ospf_config_guide.txt
└── USA
    ├── ospf_config_guide.txt
    └── ATL
        ├── ospf_config_guide.txt
        └── alpha_dc
            └── ospf_config_guide.txt

3 directories, 5 files
[ntc@ntc ~]$ 

```


##### Step 2

Try using the `cp` command to make a backup of the `dc_sites` directory. Call it `dc_sites-backup`.

```
[ntc@ntc ~]$ cp dc_sites dc_sites-backup
cp: omitting directory ‘dc_sites’
[ntc@ntc ~]$ 
```
While copying a directory, you have to use the `-r` flag to instruct the shell to copy the contents of the directories "recursively". Go ahead and make the backup using the `cp -r` command:

```
[ntc@ntc ~]$ cp -r dc_sites dc_sites-backup
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
-rw-rw-r--. 1 ntc ntc  92 Jan 25 17:20 ospf_config_guide.txt
-rw-rw-r--. 1 ntc ntc   2 Jan 25 18:00 file_count.txt
drwxrwxr-x. 3 ntc ntc  71 Jan 25 18:09 dc_sites
drwxrwxr-x. 3 ntc ntc  71 Jan 26 10:44 dc_sites-backup

```


##### Step 3
Rename the `ospf_config_guide.txt` file within all the subdirectories of `dc_sites` to the format `$directoryName-ospf_config_guide.txt`. Use the `mv` command to accomplish this.

```
[ntc@ntc ~]$ cd dc_sites
[ntc@ntc dc_sites]$ ls
directory_listing.txt  ospf_config_guide.txt  USA
[ntc@ntc dc_sites]$ mv ospf_config_guide.txt dc_sites-ospf_config_guide.txt
[ntc@ntc dc_sites]$ 

```

```

[ntc@ntc dc_sites]$ mv USA/ospf_config_guide.txt USA/USA-ospf_config_guide.txt
[ntc@ntc dc_sites]$ 

```

```

[ntc@ntc dc_sites]$ mv USA/ATL/ospf_config_guide.txt USA/ATL/ATL-ospf_config_guide.txt
[ntc@ntc dc_sites]$ 

```

```

[ntc@ntc dc_sites]$ mv USA/ATL/alpha_dc/ospf_config_guide.txt USA/ATL/alpha_dc/alpha_dc-ospf_config_guide.txt
[ntc@ntc dc_sites]$ 

```



```
[ntc@ntc dc_sites]$ tree
.
├── USA
│   ├── USA-ospf_config_guide.txt
│   └── ATL
│       ├── ATL-ospf_config_guide.txt
│       └── alpha_dc
│           └── alpha_dc-ospf_config_guide.txt
└── dc_sites-ospf_config_guide.txt

3 directories, 5 files
[ntc@ntc dc_sites]$ 
```


##### Step 4

Use the `rm` command to delete the `$directoryName-ospf_config_guide.txt` file from each subdirectory of `dc_sites`.

```
[ntc@ntc dc_sites]$ pwd
/home/ntc/dc_sites
[ntc@ntc dc_sites]$ rm dc_sites-ospf_config_guide.txt 
[ntc@ntc dc_sites]$ 
[ntc@ntc dc_sites]$ rm USA/USA-ospf_config_guide.txt 
[ntc@ntc dc_sites]$ 
[ntc@ntc dc_sites]$ rm USA/ATL/ATL-ospf_config_guide.txt 
[ntc@ntc dc_sites]$ 
[ntc@ntc dc_sites]$ rm USA/ATL/alpha_dc/alpha_dc-ospf_config_guide.txt 
[ntc@ntc dc_sites]$ 

```

> On some implementations you might be prompted with a "Are you sure you want to delete this file" prompt.


```
[ntc@ntc dc_sites]$ tree
.
└── USA
    └── ATL
        └── alpha_dc

3 directories, 1 file
[ntc@ntc dc_sites]$ 

```

##### Step 5

Similar to the recursive copy, use the `-r` flag with the `rm` command to remove the `dc_sites-backup` directory and all files within it.

```
[ntc@ntc ~]$ rm -r dc_sites-backup
[ntc@ntc ~]$ ls -ltr
total 12
drwxrwxr-x. 3 ntc ntc  51 Jan 22 19:56 configs
drwxrwxr-x. 3 ntc ntc  45 Jan 23 17:02 VirtualBoxVMs
-rw-rw-r--. 1 ntc ntc   0 Jan 25 16:01 test
-rw-rw-r--. 1 ntc ntc   0 Jan 25 16:06 error
-rw-rw-r--. 1 ntc ntc 225 Jan 25 16:11 output
-rw-rw-r--. 1 ntc ntc  92 Jan 25 17:20 greetings.txt
-rw-rw-r--. 1 ntc ntc   2 Jan 25 18:00 file_count.txt
drwxrwxr-x. 3 ntc ntc  50 Jan 26 11:03 dc_sites
[ntc@ntc ~]$ 

```

> A commonly used pattern is the `rm -fr` command. The `f` will forcibly remove all files and recursively, in combination with `r`. 

This is a very powerful command and needs careful consideration before using. [rm -fr](https://media2.giphy.com/media/7cxkulE62EV2/giphy.gif)

