## Lab - Class Repo

### Task 1 - Clone Class Repo

##### Step 1

Navigate to the class repo:

[`https://github.com/networktocode/training`](https://github.com/networktocode/training)

![](images/github05.png)


##### Step 2

Fork this repo using to public GitHub account.  Use the `fork` button to fork the repo (see red rectangle).

![](images/github06.png)

![](images/github07.png)


##### Step 3

Once the repo has finished forking you will need to fork it to your **jumphost**.

To clone the repo you will need to use the `https` link provided on GitHub.

> See read box.  Click the `Use HTTPS` link to populate the `https` clone link.

![](images/github08.png)

![](images/github09.png)

Once you have the link you will use `git clone` from your home directory on the **jumphost** host.

> I am using my `https` clone link.  You will need to use the one you collected above.

```bash
ntc@ntc:~$ git clone https://github.com/dancwilliams/training.git
Cloning into 'training'...
remote: Counting objects: 150, done.
remote: Total 150 (delta 0), reused 0 (delta 0), pack-reused 150
Receiving objects: 100% (150/150), 28.20 KiB | 0 bytes/s, done.
Resolving deltas: 100% (56/56), done.
Checking connectivity... done.
```


##### Step 4

Once the repo has been cloned, investigate the contents of the directory.

```bash
ntc@ntc:~$ ls -ltr training/
total 48
-rw-rw-r-- 1 ntc ntc   94 Feb 14 19:12 README.md
-rw-rw-r-- 1 ntc ntc  539 Feb 14 19:12 csr1.md
-rw-rw-r-- 1 ntc ntc   72 Feb 14 19:12 switch6.cfg
-rw-rw-r-- 1 ntc ntc   17 Feb 14 19:12 switch5.cfg
-rw-rw-r-- 1 ntc ntc   98 Feb 14 19:12 switch4.cfg
-rw-rw-r-- 1 ntc ntc  403 Feb 14 19:12 switch3.cfg
-rw-rw-r-- 1 ntc ntc  269 Feb 14 19:12 switch2.cfg
-rw-rw-r-- 1 ntc ntc  323 Feb 14 19:12 switch1.cfg
drwxrwxr-x 2 ntc ntc 4096 Feb 14 19:12 snippets
-rw-rw-r-- 1 ntc ntc 1829 Feb 14 19:12 ping_test.py
-rw-rw-r-- 1 ntc ntc  715 Feb 14 19:12 hostname_test.py
-rw-rw-r-- 1 ntc ntc  147 Feb 14 19:12 gabriele_switch.cfg
```


##### Step 5

Make changes to the repo.  This can be adding a file or updating an existing file.

After making your changes commit the changes and push them to your GitHub repo.

> The output below is an example.  Feel free to be creative.

```bash
ntc@ntc:~$ cd training/
ntc@ntc:training (master)$ touch rtr1.cfg
ntc@ntc:training (master)$ 
ntc@ntc:training (master)$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Untracked files:
  (use "git add <file>..." to include in what will be committed)

        rtr1.cfg

nothing added to commit but untracked files present (use "git add" to track)
ntc@ntc:training (master)$ git add rtr1.cfg 
ntc@ntc:training (master)$ 
ntc@ntc:training (master)$ git commit -m "Added rtr1.cfg"
[master 2ea5527] Added rtr1.cfg
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 rtr1.cfg
ntc@ntc:training (master)$ 
ntc@ntc:training (master)$ git push
Username for 'https://github.com': dancwilliams
Password for 'https://dancwilliams@github.com': 
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 278 bytes | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/dancwilliams/training.git
   d21c412..2ea5527  master -> master
```


##### Step 6

Now that your repo has been updated, create a pull request again the class repo.

On the repo GitHub site you will see that your repo is some number of commits ahead of the `networktocode:master` repo.

![](images/github10.png)


##### Step 7

Create a pull request to `networktocode:master` by clicking the `Pull Request` link.

![](images/github11.png)

![](images/github12.png)

After clicking `Create Pull Request` above you will be presented with a screen to give a summary and details around your pull request.  

![](images/github13.png)

After filling out your details click the final `Create Pull Request` button.  You will be taken to a screen that will show you the status of your newly created pull request!