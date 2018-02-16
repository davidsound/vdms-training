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

---
# Removing files

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
# Lab Time

Lab 28 - Navigate git history

---


class: ubuntu
# Using GitHub

GitHub is the single largest host for Git repositories, and is the central point of collaboration for millions of developers and projects. A large percentage of all Git repositories are hosted on GitHub, and many open-source projects use it for Git hosting, issue tracking, code review, and other things.

---
# Creating a GitHub repository


You can create a repository by clicking on the option

<img src="slides/media/git/new_repo.png" alt="Pep8" style="align:middle:;width:800px;height:550px;">


---

# Creating a GitHub repository(Contd.)

Give the repo a new name

<img src="slides/media/git/repo_name.png" alt="Pep8" style="align:middle:;width:800px;height:550px;">


---

class: ubuntu

# Connecting the local and remote repositories

"Remotes" tell the local git repo, which remote repo to connect with.

```
ntc@ntc:configs (master)$ git remote add origin https://github.com/smith-ntc/JS_configs
ntc@ntc:configs (master)$
```

Note: The word `origin` is just a string and used by convention as the default `remote`

(When you clone a repo from github, by default, that repo's "origin" is mapped to this name)

---

class: ubuntu
# Viewing the remote

By default, the same origin is used for `pushing to` and `fetching from` 


```
ntc@ntc:configs (master)$ git remote -v
origin  https://github.com/smith-ntc/JS_configs (fetch)
origin  https://github.com/smith-ntc/JS_configs (push)
```

---

class: ubuntu
# Pushing the local repository to remote

After ensuring that all local commits are good to be pushed to the remote server, use the `git push origin master` command 

```
ntc@ntc:configs (master)$ git push origin master
Username for 'https://github.com': smith-ntc
Password for 'https://smith-ntc@github.com':
Counting objects: 22, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (22/22), done.
Writing objects: 100% (22/22), 4.45 KiB | 0 bytes/s, done.
Total 22 (delta 13), reused 0 (delta 0)
remote: Resolving deltas: 100% (13/13), done.
To https://github.com/smith-ntc/JS_configs
 * [new branch]      master -> master
```


Here, the "master" is referencing the local repository.


---
# Lab Time

Lab 29 - Getting started with GitHub

---

class: ubuntu
# Cloning from remote repositories

Many open source projects are available as github repositories.

Use the `git clone` command

```
ntc@ntc:~$ git clone https://github.com/ansible/ansible
Cloning into 'ansible'...
remote: Counting objects: 298586, done.
remote: Compressing objects: 100% (52/52), done.
remote: Total 298586 (delta 46), reused 33 (delta 25), pack-reused 298505
Receiving objects: 100% (298586/298586), 103.44 MiB | 13.48 MiB/s, done.
Resolving deltas: 100% (190396/190396), done.
Checking connectivity... done.
Checking out files: 100% (8546/8546), done.
ntc@ntc:~$ 

```

By default adds the project to a directory

```
ntc@ntc:~$ ls -lp | grep ans
drwxrwxr-x 14 ntc ntc    4096 Feb 15 10:36 ansible/
ntc@ntc:~$ 
```

---

class: ubuntu
# Cloning (Contd.)

Optionally clone into another directory

```
ntc@ntc:~$ git clone https://github.com/napalm-automation/napalm.git my_project
Cloning into 'my_project'...
remote: Counting objects: 24022, done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 24022 (delta 1), reused 4 (delta 0), pack-reused 24015
Receiving objects: 100% (24022/24022), 5.12 MiB | 8.05 MiB/s, done.
Resolving deltas: 100% (12966/12966), done.
Checking connectivity... done.
ntc@ntc:~$ 


```

Check the remote to see the path for the push and pull repositories

```
ntc@ntc:~$ cd my_project/
ntc@ntc:my_project (develop)$ git remote -v
origin	https://github.com/napalm-automation/napalm.git (fetch)
origin	https://github.com/napalm-automation/napalm.git (push)
ntc@ntc:my_project (develop)$ 

```


---
# Lab Time

- Lab 30 - Using GitHub to restore backup
- Lab 31 - Working with Open Source Projects

---

# Branches in git

Branching means you diverge from the main line of development and continue to do work without messing with that main line


<img src="slides/media/git/branch1.png" alt="Pep8" style="align:middle:;width:500px;height:350px;">

---
# How does git keep track of the current branch?

A special pointer called "HEAD" is used to keep track of the current branch

<img src="slides/media/git/branch2.png" alt="Pep8" style="align:middle:;width:700px;height:350px;">

---

# Checking out a branch

Checking out a branch means making "HEAD" point to the new branch

<img src="slides/media/git/branch3.png" alt="Pep8" style="align:middle:;width:700px;height:500px;">


---

# Advancing the branch

<img src="slides/media/git/branch4.png" alt="Pep8" style="align:middle:;width:700px;height:500px;">

---
# Checking out master

"Master" is simply a name used by convention. The "HEAD" moves back to that commit

<img src="slides/media/git/branch5.png" alt="Pep8" style="align:middle:;width:700px;height:500px;">

---
# Advancing the "master" branch
Advancing master causes a split

<img src="slides/media/git/branch6.png" alt="Pep8" style="align:middle:;width:700px;height:500px;">



---
class: center, middle, segue


# Using a scenario to understand branching

---

# Branching master

Create a new branch "iss53" to work on "Issue 53"

<img src="slides/media/git/branch7.png" alt="Pep8" style="align:middle:;width:700px;height:450px;">

 
---
# Update branch 

Update "iss53"

<img src="slides/media/git/branch8.png" alt="Pep8" style="align:middle:;width:700px;height:450px;">


---
# Request for a fix in master

While still working on "iss53" quickly revert back to fix master

<img src="slides/media/git/branch9.png" alt="Pep8" style="align:middle:;width:700px;height:450px;">


---
# Advance master

New master is old master + the fix applied to it (Fast Forward)

<img src="slides/media/git/branch10.png" alt="Pep8" style="align:middle:;width:700px;height:450px;">


---

# Continue working on branch

Complete work on "iss53" (which was branched of the old master - C2)

<img src="slides/media/git/branch11.png" alt="Pep8" style="align:middle:;width:700px;height:450px;">

---

# Merge commit

Git automatically figures out the current master and creates a new commit ( rather than just move the pointer) 
<img src="slides/media/git/branch12.png" alt="Pep8" style="align:middle:;width:700px;height:450px;">

---


class: ubuntu
# Handling conflicts

If you changed the same part of the same file differently in the two branches you’re merging together, Git won’t be able to merge them cleanly

```
ntc@ntc:backup_configs (master)$ git merge CHG002
Auto-merging nyc-rtr01.cfg
CONFLICT (content): Merge conflict in nyc-rtr01.cfg
Automatic merge failed; fix conflicts and then commit the result.
```
---

class: ubuntu
# Handling conflicts(Contd.)

Use `git status` and `git diff` to identify the confilict

```
ntc@ntc:backup_configs (master)$ git diff master CHG002
diff --git a/nyc-rtr01.cfg b/nyc-rtr01.cfg
index c8dee07..c7032da 100644
--- a/nyc-rtr01.cfg
+++ b/nyc-rtr01.cfg
@@ -164,7 +164,7 @@ interface GigabitEthernet4
  negotiation auto
 !
 interface Loopback101
- description Loopback interface for OSPF
+ description OSPF router id
  ip address 10.1.1.1 255.255.255.255
 !
 interface Loopback102
```


---
# Fixing conflicts

When a merge conflict happens, git will highlight the conflict within the file causing the conflict.

Resolution is manual: 
    - Edit the file to resolve the conflict
    - Use `git add` to stage the changes
    - Finally commit the changes


The `git log --graph`  is a great tool to visualize the branching.


---
# Deleting a branch

Typically branches used to make "fixes" are deleted upon completing the task

```
ntc@ntc:backup_configs (master)$ git branch -d CHG002
Deleted branch CHG002 (was a49a07a).
```

---

class: ubuntu
# Some branching stratergies
<img src="slides/media/git/branch-strategy.png" alt="Pep8" style="align:middle:;width:750px;height:400px;">


---

# Lab Time

- Lab 32 - Git Branches
- Lab 33 - Merge Conflits

---


class: ubuntu
# Forking and pull requests


A fork is a copy of a repository. Forking a repository allows you to freely experiment with changes without affecting the original project.

<img src="slides/media/git/fork1.png" alt="Pep8" style="align:middle:;width:350px;height:100px;">


---

# Workflow

1. Fork a public repo
2. Clone the "forked" version locally
3. Make updates
4. Push upstream
5. Submit a pull request

---

class: ubuntu
# Pull request

Pull requests are a way to request changes to a repository

- Can be made between branches from within a repo
- Submitting PRs from a forked copy of a public repo is how the maintainer gets an opportunity to review the request
- Typically used to build CI pipelines as a trigger to kick off automated testing

---

# Working with a shared repo

Best practices

- Add the original repo as a remote called `upstream`
- Always `fetch` from upstream but push to `origin`

---
# Lab Time

- Lab 34 - Pull requests
- Lab 35 - Working with a shared repo
- Lab 36 - Maintaining a forked repo
---









