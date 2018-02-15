## Lab 1 - Starting with Git

This lab will help you build familiarity with Git and master the key commands to become proficient.

** This lab requires that Lab 9: Cyberduck be completed. **



### Task 1 - Initialize a new repository

##### Step 1

Navigate to the `configs` directory on your **jumphost**. We will use this directory as a version controlled repository of router configurations.

```bash
ntc@ntc:~$ cd configs/
ntc@ntc:configs$
```

##### Step 2

Use the `ls` command to list the content of the directory. 

```bash
ntc@ntc:configs$ ls
atl-rtr01.cfg  atl-rtr03.cfg  nyc-rtr02.cfg
atl-rtr02.cfg  nyc-rtr01.cfg  nyc-rtr03.cfg
```


##### Step 3

Before we continue, add a Git user and email to track commits.

```bash
ntc@ntc:configs$ git config --global user.name "smith-ntc"
ntc@ntc:configs$ git config --global user.email "john.smith@networktocode.com"
```


##### Step 4

Use `git config --list` to verify the configuration.

```bash
ntc@ntc:configs$ git config --list
user.name=smith-ntc
user.email=john.smith@networktocode.com
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true

```


##### Step 5

Now run `git init` to inizialize the repository.

```bash
ntc@ntc:configs$ git init
Initialized empty Git repository in /home/ntc/configs/.git/
```

From now on the directory will be treated as a Git repository.











### Task 2 - Work with a newly initialized repository

##### Step 1

Check the status of the repository using `git status`.

```bash
ntc@ntc:configs$ git status
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        atl-rtr01.cfg
        atl-rtr02.cfg
        atl-rtr03.cfg
        nyc-rtr01.cfg
        nyc-rtr02.cfg
        nyc-rtr03.cfg

nothing added to commit but untracked files present (use "git add" to track)
```

The output shows that the current branch is `master` and that several untracked files exist. 


##### Step 2

Create a new file called `README.md`.

```bash
ntc@ntc:configs$ touch README.md
```


##### Step 3

Check again the status of the repository.

```bash
ntc@ntc:configs$ git status
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        README.md
        atl-rtr01.cfg
        atl-rtr02.cfg
        atl-rtr03.cfg
        nyc-rtr01.cfg
        nyc-rtr02.cfg
        nyc-rtr03.cfg

nothing added to commit but untracked files present (use "git add" to track)
```

Git is now recognizing the new file as untracked.


##### Step 4

Add all changes to the staging phase.

```bash
ntc@ntc:configs$ git add .
```


##### Step 5

Verify the status of the repository.

```bash
ntc@ntc:configs$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   README.md
        new file:   atl-rtr01.cfg
        new file:   atl-rtr02.cfg
        new file:   atl-rtr03.cfg
        new file:   nyc-rtr01.cfg
        new file:   nyc-rtr02.cfg
        new file:   nyc-rtr03.cfg
```

This time Git shows the all files are ready to be commited.


##### Step 6

Commit the changes with a message.

```bash
ntc@ntc:configs$ git commit -m "Adding configuration files and README"
[master (root-commit) 82a4df4] Adding configuration files and README
 7 files changed, 210 insertions(+)
 create mode 100644 README.md
 create mode 100644 atl-rtr01.cfg
 create mode 100644 atl-rtr02.cfg
 create mode 100644 atl-rtr03.cfg
 create mode 100644 nyc-rtr01.cfg
 create mode 100644 nyc-rtr02.cfg
 create mode 100644 nyc-rtr03.cfg
```


##### Step 7

Check the status again.

```bash
ntc@ntc:configs (master)$ git status
On branch master
nothing to commit, working directory clean
```

Since we have just commited our changes, the working directory is clean now.


##### Step 8

Open the `nyc-rtr01.cfg` file and add `Loopback101`.

```
<--omitted-->
!
interface Loopback101
 ip address 10.1.1.1 255.255.255.255
!
<--omitted-->
```


##### Step 9

Open the `atl-rtr01.cfg` file and add `lo0`.

```
<--omitted-->
!
interface Loopback0
 ip address 10.2.1.1 255.255.255.255
!
<--omitted-->
```


##### Step 10

Add a new `emea-rtr01.cfg` by copying `nyc-rtr01.cfg`.

```bash
ntc@ntc:configs (master)$ cp nyc-rtr01.cfg emea-rtr01.cfg
```


##### Step 11

Check git status.

```bash
ntc@ntc:configs (master)$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   atl-rtr01.cfg
        modified:   nyc-rtr01.cfg

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        emea-rtr01.cfg

no changes added to commit (use "git add" and/or "git commit -a")
```

We now have two unstaged changes and one untracked file.


##### Step 12

Add and commit the changes on `nyc-rtr01.cfg`.

```bash
ntc@ntc:configs (master)$ git add nyc-rtr01.cfg
ntc@ntc:configs (master)$ git commit -m "Adding loopback on nyc-rtr01"
[master f0bf1fd] Adding loopback on nyc-rtr01
 1 file changed, 2 insertions(+)
```


##### Step 13

Add and commit the changes on `atl-rtr01.cfg`.

```bash
ntc@ntc:configs (master)$ git add atl-rtr01.cfg
ntc@ntc:configs (master)$ git commit -m "Adding loopback on atl-rtr01"          [master 424dbae] Adding loopback on atl-rtr01
 1 file changed, 7 insertions(+)
```


##### Step 14

Add and commit the new file `emea-rtr01.cfg`.

```bash
ntc@ntc:configs (master)$ git add emea-rtr01.cfg
ntc@ntc:configs (master)$ git commit -m "Add new configuration file"
[master d9b108c] Add new configuration file
 1 file changed, 210 insertions(+)
 create mode 100644 emea-rtr01.cfg
ntc@ntc:configs (master)$

```


##### Step 15

Update the hostname on `nyc-rtr01.cfg`.

```
<--omitted-->
!
hostname nyc-rtr011
!
<--omitted-->
```


##### Step 16

Add and commit the latest change.

```bash
ntc@ntc:configs (master)$ git commit -m "Update hostname on nyc-rtr01.cfg"
[master 210f548] Update hostname on nyc-rtr01.cfg
 1 file changed, 1 insertion(+), 1 deletion(-)
```


##### Step 17

Remove last commit from history via `reset`.

```bash
ntc@ntc:configs (master)$ git reset --hard HEAD~1
HEAD is now at d9b108c Add new configuration file
```

Feel free to verify that the hostname on `nyc-rtr01.cfg` has not changed.


##### Step 18

Add an OSPF instance on `nyc-rtr01.cfg`.

```
<--omitted-->
!
router ospf 100
 network 10.1.100.0 0.0.0.255 area 0
!
<--omitted-->
```


##### Step 19

Add the new changes.

```bash
ntc@ntc:configs (master)$ git commit -m "Add OSPF on nyc-rtr01.cfg"
[master 2cf27e2] Add OSPF on nyc-rtr01.cfg
 1 file changed, 4 insertions(+)
```
