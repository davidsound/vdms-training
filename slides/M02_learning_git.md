layout: true

.footer-picture[![Network to Code Logo](slides/media/Footer2.PNG)]
.footnote-left[(C) 2015 Network to Code, LLC. All Rights Reserved. ]
.footnote-con[CONFIDENTIAL]

---

class: center, middle, title
.footer-picture[<img src="slides/media/Footer1.PNG" alt="Blue Logo" style="alight:middle;width:350px;height:60px;">]

# Introduction to Git and GitHub


---

# Module Overview

- A brief history of git
- Git and GitHub architecture overview
- Using git for version control
- Github
- Cloning repositories
- Working with branches and forks
- Git conflicts and pull requests

---

# Git - a brief history

In 2002, the Linux kernel project began using a proprietary VCS called BitKeeper. In 2005, the relationship between the community that developed the Linux kernel and the commercial company that developed BitKeeper broke down, and the tool’s free-of-charge status was revoked. This prompted the Linux development community (and in particular Linus Torvalds, the creator of Linux) to develop their own tool based on some of the lessons they learned while using BitKeeper.

Goals:

- Speed
- Simple design
- Strong support for non-linear development (thousands of parallel branches)
- Fully distributed
- Able to handle large projects like the Linux kernel efficiently (speed and data size)



---

# Git architecture

Conceptually, most other systems store information as a list of file-based changes. They think of the information as a set of files and the changes made to each file over time (this is commonly described as delta-based version control).

<img src="slides/media/git/legacy_vcs.png" alt="Pep8" style="align: middle:;width:400px;height:350px;">


---
# Git architecture

Git doesn’t think of or store its data this way. Instead, Git thinks of its data more like a series of snapshots of a miniature filesystem. Git  a picture of what all the files look like at that moment and stores a reference to that snapshot. To be efficient, if files have not changed, Git doesn’t store the file again, just a link to the previous identical file it has already stored. 

<img src="slides/media/git/git_vcs.png" alt="Pep8" style="align: middle:;width:400px;height:350px;">

---
# Git architecture - The 3 stages


.left-column[
<img src="slides/media/git/git_stages.png" alt="Pep8" style="align: left:;width:400px;height:350px;">
]

.right-column[

 Git has three main states that your files can reside in: committed, modified, and staged:

- **Committed** means that the data is safely stored in your local database.

- **Modified** means that you have changed the file but have not committed it to your database yet.

- **Staged** means that you have marked a modified file in its current version to go into your next commit snapshot.

]

---

# Git workflow

1. Modify files in your working tree.

2. Selectively stage just those changes - this adds only those changes to the staging area.

3. Do a commit, which takes the files as they are in the staging area and stores that snapshot permanently to the Git directory (the `.git`)


---

class: ubuntu
# Git getting started

Git comes with a tool called `git config` that lets you get and set configuration variables that control all aspects of how Git looks and operates


```
ntc@ntc:configs$ git config --global user.name "smith-ntc"
ntc@ntc:configs$ git config --global user.email "john.smith@networktocode.com"
git config --global core.editor vim
```

Use `git config --list` to verify the configuration

```
ntc@ntc:configs$ git config --list
user.name=smith-ntc
user.email=john.smith@networktocode.com
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
core.editor=vim
```

---

class: ubuntu
# Initializing

Initializing a repository

```
ntc@ntc:configs$ git init
Initialized empty Git repository in /home/ntc/configs/.git/
```


Checking the status

```
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
---

class: ubuntu
# Staging changes

Staging files
```
ntc@ntc:configs$ git add .
```

Checking status again:

```
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

---

class: ubuntu
# Committing the changes

Commits require a comment

```
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

Checking status:

```
ntc@ntc:configs (master)$ git status
On branch master
nothing to commit, working directory clean
```

---

class: ubuntu
# Making changes to a repository

If files are modified/added, running `git status` will highlight them as modified or not being tracked respectively


```
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

---
# The git status 

<img src="slides/media/git/lifecycle.png" alt="Pep8" style="align: left:;width:600px;height:400px;">



---

class: ubuntu
# Selective staging and commits

Individual files can be staged as separate commits.

```
ntc@ntc:configs (master)$ git add nyc-rtr01.cfg
ntc@ntc:configs (master)$ git commit -m "Adding loopback on nyc-rtr01"
[master f0bf1fd] Adding loopback on nyc-rtr01
 1 file changed, 2 insertions(+)
```


```
ntc@ntc:configs (master)$ git add atl-rtr01.cfg
ntc@ntc:configs (master)$ git commit -m "Adding loopback on atl-rtr01"          
Adding loopback on atl-rtr01
 1 file changed, 7 insertions(+)
```

---

class: ubuntu
# Ignoring files

- Often, you’ll have a class of files that you don’t want Git to automatically add or even show you as being untracked. These are generally automatically generated files such as log files

- In such cases, you can create a file listing patterns to match them named .gitignore. Here is an example `.gitignore` file:


```
$ cat .gitignore
*.pyc
*.retry
```

---


class: ubuntu
# Removing files

To remove a file from Git, you have to remove it from your tracked files (more accurately, remove it from your staging area) and then commit

```
ntc@ntc:configs (master)$ rm atl-rtr01.cfg
ntc@ntc:configs (master)$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    atl-rtr01.cfg

no changes added to commit (use "git add" and/or "git commit -a")
```

Use `git rm` to remove it from the staging area and then commit

```
ntc@ntc:configs (master)$ git rm atl-rtr01.cfg
rm 'atl-rtr01.cfg'
```

NOTE: If you need to remove a file/directory from the git repository but still keep it on the hard disk, use the `--cached` option.

```
$ git rm --cached README.md
```

---
class: ubuntu
# Reverting a previous commit

This is done using `git reset`

If you do `git reset --hard <SOME-COMMIT>` then Git will:

Make your current branch (typically master) back to point at <SOME-COMMIT>.
Then make the files in your working tree and the index ("staging area") the same as the versions committed in <SOME-COMMIT>

```
ntc@ntc:configs (master)$ git reset --hard HEAD~1
HEAD is now at d9b108c Add new configuration file
```

Potentially dangerous command, since it throws away all your uncommitted changes. For safety, you should always check that the output of git status is clean (that is, empty) before using it.


---
# Lab Time

Lab 27 - Getting started with git


---

class: ubuntu
# Navigating git history

Use `git log` to check the log trail and commit history.
By default, with no arguments, git log lists the commits made in that repository in reverse chronological order 

```
ntc@ntc:configs (master)$ git log
commit 2cf27e2a4082c3e79cc3c64a18282adbf3adafca
Author: smith-ntc <john.smith@networktocode.com>
Date:   Thu Jan 25 09:54:18 2018 -0500

    Add OSPF on nyc-rtr01.cfg

commit d9b108cbf72d1c8b193d5bb6981c7c669fcf9855
Author: smith-ntc <john.smith@networktocode.com>
Date:   Thu Jan 25 09:43:35 2018 -0500

    Add new configuration file

commit 424dbae8393702e99f4c1581935f59ec3e2ab0f4
Author: smith-ntc <john.smith@networktocode.com>
Date:   Thu Jan 25 09:35:00 2018 -0500

    Adding loopback on atl-rtr01

commit f0bf1fd844e95ebb95d17f1ad9a38cdb66c60e07
Author: smith-ntc <john.smith@networktocode.com>
Date:   Thu Jan 25 09:34:04 2018 -0500
```
---

# Git log options

- `$ git log --stat` :  Prints below each commit entry a list of modified files, how many files were changed, and how many lines in those files were added and removed. It also puts a summary of the information at the end.
- `$ git log --pretty=oneline` : The oneline option prints each commit on a single line, which is useful if you’re looking at a lot of commits
- `$ git log --graph`: Displays an ASCII graph of the branch and merge history beside the log output.

---

class: ubuntu
# Comparing commits

- The `git diff` command is used to compare commits
- Compare any 2 commits by picking their commit hash

```
ntc@ntc:configs (master)$ git diff 2cf27e2a4082c3e79cc3c64a18282adbf3adafca d9b108cbf72d1c8b193d5bb6981c7c669fcf9855
diff --git a/nyc-rtr01.cfg b/nyc-rtr01.cfg
index 1f55993..5fa7122 100644
--- a/nyc-rtr01.cfg
+++ b/nyc-rtr01.cfg
@@ -166,10 +166,6 @@ interface GigabitEthernet4
 interface Loopback101
  ip address 10.1.1.1 255.255.255.255
 !
-!
-router ospf 100
- network 10.1.100.0 0.0.0.255 area 0
-!
 virtual-service csr_mgmt
 !
 ip forward-protocol nd
```

---


class: ubuntu

# Reverting to an earlier commit

Use the `git checkout` command to revert to an earlier commit

An earlier commit is specified using the commit hash

```
ntc@ntc:configs (master)$ git checkout d9b108cbf72d1c8b193d5bb6981c7c669fcf9855
Note: checking out 'd9b108cbf72d1c8b193d5bb6981c7c669fcf9855'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>

HEAD is now at d9b108c... Add new configuration file
```


---


