## Lab - Work with branches

### Task 1 - Work with branches



##### Step 1

Back on your **jumphost** `backup_configs` local repository, create a new branch called `CHG001`.

```bash
ntc@ntc:backup_configs (master)$ git checkout -b CHG001
Switched to a new branch 'CHG001'
ntc@ntc:backup_configs (CHG001)$
```

> The `-b` option tells git to checkout and create new branch.


##### Step 2

Update `nyc-rtr01.cfg` with a new loopback interface and advertise it in OSPF process as you already did on previous labs.

```
<---omitted--->
!
interface Loopback102
 ip address 10.2.2.2 255.255.255.255
!
!
router ospf 100
 network 10.1.1.0 0.0.0.255 area 0
 network 10.2.2.0 0.0.0.255 area 0
!
<---omitted--->
```

##### Step 3

Stage your latest changes.

```
ntc@ntc:backup_configs (CHG001)$ git add nyc-rtr01.cfg
ntc@ntc:backup_configs (CHG001)$
```


##### Step 4

Commit your changes with a message.

```bash
ntc@ntc:backup_configs (CHG001)$ git commit -m "Adding a new loopback and advertising it on OSPF"
[CHG001 afe84b3] Adding a new loopback and advertising it on OSPF
 1 file changed, 4 insertions(+)
```


##### Step 5

Push your local branch upstream on GitHub. This will create a new branch on the remote repository.

```bash
ntc@ntc:backup_configs (CHG001)$ git push origin CHG001
Username for 'https://github.com': <YOUR PUBLIC GITHUB USERNAME>
Password for 'https://<YOUR PUBLIC GITHUB USERNAME>@github.com':
Counting objects: 3, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 380 bytes | 0 bytes/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To <GIT CLONE HTTPS LINK TO YOUR REPOSITORY>
 * [new branch]      CHG001 -> CHG001
```

Go you GitHub and verify that the new `CHG001` branch has been pushed.


##### Step 6

Move back to the master branch.

```bash
ntc@ntc:backup_configs (CHG001)$ git checkout master
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
```


##### Step 7

Now merge the `CHG001` branch into master.

```bash
ntc@ntc:backup_configs (master)$ git merge CHG001
Updating b2c553b..afe84b3
Fast-forward
 nyc-rtr01.cfg | 4 ++++
 1 file changed, 4 insertions(+)
```


##### Step 8

Now that `CHG001` has been merged, you can delete the branch.

```bash
ntc@ntc:backup_configs (master)$ git branch -d CHG001
Deleted branch CHG001 (was afe84b3).
```


##### Step 9

Push changes upstream into master.

```bash
ntc@ntc:backup_configs (master)$ git push origin master
Username for 'https://github.com': <YOUR PUBLIC GITHUB USERNAME>
Password for 'https://<YOUR PUBLIC GITHUB USERNAME>@github.com':
Total 0 (delta 0), reused 0 (delta 0)
To <GIT CLONE HTTPS LINK TO YOUR REPOSITORY>
   b2c553b..afe84b3  master -> master
```