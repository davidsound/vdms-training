## Lab - Deal with conflicts

### Task 1 - Deal with conflicts



##### Step 1

Within the `~/backup_configs/` directory on you **jumphost**, create a new branch called `CHG002`.

```bash
ntc@ntc:backup_configs (master)$ git checkout -b CHG002
Switched to a new branch 'CHG002'
ntc@ntc:backup_configs (CHG002)$
```


##### Step 2

Open `nyc-rtr01.cfg` and add a description for Loopback 101 as `OSPF router id`.

```
<---omitted--->
!
interface Loopback101
 description OSPF router id
 ip address 10.1.1.1 255.255.255.255
!
<---omitted--->
```


##### Step 3

Add and commit your latest changes.

```bash
ntc@ntc:backup_configs (CHG002)$ git add nyc-rtr01.cfg
ntc@ntc:backup_configs (CHG002)$ git commit -m "Adding description for lo101"
[CHG002 a49a07a] Adding description for lo101
 1 file changed, 1 insertion(+)
ntc@ntc:backup_configs (CHG002)$
```


##### Step 4

Move back to the master branch

```bash
ntc@ntc:backup_configs (CHG002)$ git checkout  master
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
```


##### Step 5

Open again `nyc-rtr01.cfg` and add a different description for Loopback 101 then before: `Loopback interface for OSPF`.

```
<---omitted--->
!
interface Loopback101
 description Loopback interface for OSPF
 ip address 10.1.1.1 255.255.255.255
!
<---omitted--->
```


##### Step 6

Add and commit your latest changes.

```bash
ntc@ntc:backup_configs (master)$ git add nyc-rtr01.cfg
ntc@ntc:backup_configs (master)$ git commit -m "Adding loopback description"
[master ebea399] Adding loopback description
 1 file changed, 1 insertion(+)
ntc@ntc:backup_configs (master)$
```


##### Step 7

Try to merge `CHG002` into `master`.

> 'merge` ncorporates changes from the named commits (since the time their histories diverged from the current branch) into the current branch. This command is used by git pull to incorporate changes from another repository and can be used by hand to merge changes from one branch into another. [Git Documentation](https://git-scm.com/docs/git-merge)

```bash
ntc@ntc:backup_configs (master)$ git merge CHG002
Auto-merging nyc-rtr01.cfg
CONFLICT (content): Merge conflict in nyc-rtr01.cfg
Automatic merge failed; fix conflicts and then commit the result.
```

Since both branches have different versions of the same file, a merge conflict error has been raised requiring a manual change.


##### Step 8

Get specifics of the conflict by executing `git diff`

```bash
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


##### Step 9

In order to fix the conflict we can simply remove the bogus line on nyc-rt01.cfg.

```
!
interface Loopback101
 ip address 10.1.1.1 255.255.255.255
!
```


##### Step 10

Add and commit the changes.

```bash
ntc@ntc:backup_configs (master)$ git add nyc-rtr01.cfg
ntc@ntc:backup_configs (master)$ git commit -m "Resolve conflict"
[master 165c98b] Resolve conflict
```


##### Step 11

Use the `git log --graph` option, to visualize the branching

```bash
ntc@ntc:backup_configs (master)$ git log --graph
*   commit 165c98b4bd3a1e8b0e37a193507f514ce4ab6bd1
|\  Merge: ebea399 a49a07a
| | Author: smith-ntc <john.smith@networktocode.com>
| | Date:   Thu Feb 1 16:09:47 2018 -0500
| |
| |     Resolve conflict
| |
| * commit a49a07abf58c007343e6ad2977f13a8abc7c0ba3
| | Author: smith-ntc <john.smith@networktocode.com>
| | Date:   Thu Feb 1 15:59:15 2018 -0500
| |
| |     Adding description for lo101
| |
* | commit ebea399994937906f1dcea0f7e30becec180ad42
|/  Author: smith-ntc <john.smith@networktocode.com>
|   Date:   Thu Feb 1 16:03:55 2018 -0500
|
|       Adding loopback description
|
* commit afe84b3ea15094378a2faabf81de018833f8bf47
| Author: smith-ntc <john.smith@networktocode.com>
| Date:   Thu Feb 1 15:46:08 2018 -0500
|
|     Adding a new loopback and advertising it on OSPF
|
* commit b2c553b4f952c382ca16d0cf31367462c098e2ee
| Author: smith-ntc <john.smith@networktocode.com>
| Date:   Thu Jan 25 13:10:11 2018 -0500
|
|     Adding emea-rtr02.cfg
|
* commit fc348cdd22a9cfe1ba6ae3dc2b396ee28e052bbd
| Author: smith-ntc <john.smith@networktocode.com>
| Date:   Thu Jan 25 10:17:19 2018 -0500
|
|     Fix typo on OSPF configuration
|
* commit 2cf27e2a4082c3e79cc3c64a18282adbf3adafca
| Author: smith-ntc <john.smith@networktocode.com>
| Date:   Thu Jan 25 09:54:18 2018 -0500
|
|     Add OSPF on nyc-rtr01.cfg
|
* commit d9b108cbf72d1c8b193d5bb6981c7c669fcf9855
| Author: smith-ntc <john.smith@networktocode.com>
| Date:   Thu Jan 25 09:43:35 2018 -0500
|
|     Add new configuration file
|
* commit 424dbae8393702e99f4c1581935f59ec3e2ab0f4
| Author: smith-ntc <john.smith@networktocode.com>
| Date:   Thu Jan 25 09:35:00 2018 -0500
|
|     Adding loopback on atl-rtr01
|
* commit f0bf1fd844e95ebb95d17f1ad9a38cdb66c60e07
| Author: smith-ntc <john.smith@networktocode.com>
| Date:   Thu Jan 25 09:34:04 2018 -0500
|
|     Adding loopback on nyc-rtr01
|
* commit fa81740bcb2faaa724b17385ef9a56fa63140e30
  Author: smith-ntc <john.smith@networktocode.com>
  Date:   Thu Jan 25 09:31:43 2018 -0500

      Adding configuration files and README

```


##### Step 11

Delete the `CHG002` branch.

```bash
ntc@ntc:backup_configs (master)$ git branch -d CHG002
Deleted branch CHG002 (was a49a07a).
```


##### Step 12

Push all updates to the remote repository.

```bash
ntc@ntc:backup_configs (master)$ git push origin master
Username for 'https://github.com': <YOUR PUBLIC GITHUB USERNAME>
Password for 'https://<YOUR PUBLIC GITHUB USERNAME>@github.com':
Counting objects: 7, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (7/7), done.
Writing objects: 100% (7/7), 779 bytes | 0 bytes/s, done.
Total 7 (delta 5), reused 0 (delta 0)
remote: Resolving deltas: 100% (5/5), completed with 2 local objects.
To <GIT CLONE HTTPS LINK TO YOUR REPOSITORY>
   afe84b3..165c98b  master -> master

```
