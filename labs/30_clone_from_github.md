## Lab 4 - Clone from GitHub



### Task 1 - Clone remote repository


##### Step 1

On you **jumphost** remove the local `~/configs/` directory we have worked on so far.

```bash
ntc@ntc:~$ sudo rm -rf configs/
ntc@ntc:~$
```


##### Step 2

While in your home directory, clone the remote repository created in the previous lab and name it `backup_configs`.

> You should have the address of your GitHub repository form the previous lab.

```bash
ntc@ntc:~$ git clone https://github.com/smith-ntc/JS_configs backup_configs
Cloning into 'backup_configs'...
remote: Counting objects: 22, done.
remote: Compressing objects: 100% (9/9), done.
remote: Total 22 (delta 13), reused 22 (delta 13), pack-reused 0
Unpacking objects: 100% (22/22), done.
Checking connectivity... done.
```


### Challenge task: Add a new config file and push it

##### Step 1

Move into the cloned repository and create a new file called `emea-rtr02.cfg` 

```bash
ntc@ntc:~$ cd backup_configs/
ntc@ntc:backup_configs (master)$ touch emea-rtr02.cfg
```


##### Step 2

Add and commit the new file.

```bash
ntc@ntc:backup_configs (master)$ git add emea-rtr02.cfg
ntc@ntc:backup_configs (master)$ git commit -m "Adding emea-rtr02.cfg"
[master b2c553b] Adding emea-rtr02.cfg
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 emea-rtr02.cfg
```


##### Step 3

Push the changes to the remote repository on GitHub.

```bash
ntc@ntc:backup_configs (master)$ git push origin master
Username for 'https://github.com': smith-ntc
Password for 'https://smith-ntc@github.com':
Counting objects: 3, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 284 bytes | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/smith-ntc/JS_configs
   fc348cd..b2c553b  master -> master
```

Verify that the new file is present on GitHub.  You may need to refresh your webpage to see the changes.