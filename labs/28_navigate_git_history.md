## Lab - Navigate Git history

This lab will help you inspect and move within Git logs and history.



### Task 1 - Checkout commits

##### Step 1

Use `git log` to check the log trail and commit history.

```bash
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

    Adding loopback on nyc-rtr01

commit fa81740bcb2faaa724b17385ef9a56fa63140e30
Author: smith-ntc <john.smith@networktocode.com>
Date:   Thu Jan 25 09:31:43 2018 -0500

    Adding configuration files and README

```


##### Step 2

Use git diff to compare the last 2 commits by picking their commit hash.

> You will need to use the commit hashes given in the previous steps.

```bash
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


##### Step 3

Use `git checkout` to revert changes to the commit before the OSPF changes.

> You will need to use the commit hashes used in the previous step.

```bash
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

Open the `nyc-rtr01.cfg` file and verify no OSPF configuration is present.


##### Step 4

Now move back to the earliest commit by picking the hash of the commit at the bottom of `git log` output.

```bash
ntc@ntc:configs ((HEAD detached at d9b108c))$ git checkout fa81740bcb2faaa724b17385ef9a56fa63140e30
Previous HEAD position was d9b108c... Add new configuration file
HEAD is now at fa81740... Adding configuration files and README
```

##### Step 5

Verify only the 6 initial files are present.

```bash
ntc@ntc:configs ((HEAD detached at fa81740))$ ls
atl-rtr01.cfg  atl-rtr02.cfg  atl-rtr03.cfg
nyc-rtr01.cfg  nyc-rtr02.cfg  nyc-rtr03.cfg
```


##### Step 6

Go back to our latest commit by checking out `master`.

```bash
ntc@ntc:configs ((HEAD detached at fa81740))$ git checkout master
Previous HEAD position was fa81740... Adding configuration files and README
Switched to branch 'master'
ntc@ntc:configs (master)$
```


##### Step 7

Open `nyc-rtr01.cfg` and fix a typo on the OSPF configuration by changing `10.1.100.0` to `10.1.1.0`.

```
<--omitted-->
!
router ospf 100
 network 10.1.1.0 0.0.0.255 area 0
!
<--omitted-->
```


##### Step 8

Check the diff for the last change.

```bash
ntc@ntc:configs (master)$ git diff
diff --git a/nyc-rtr01.cfg b/nyc-rtr01.cfg
index 1f55993..49f8d9b 100644
--- a/nyc-rtr01.cfg
+++ b/nyc-rtr01.cfg
@@ -168,7 +168,7 @@ interface Loopback101
 !
 !
 router ospf 100
- network 10.1.100.0 0.0.0.255 area 0
+ network 10.1.1.0 0.0.0.255 area 0
 !
 virtual-service csr_mgmt
 !
```


##### Step 9

Add and commit the changes.

```bash
ntc@ntc:configs (master)$ git add nyc-rtr01.cfg
ntc@ntc:configs (master)$ git commit -m "Fix typo on OSPF configuration"
[master fc348cd] Fix typo on OSPF configuration
 1 file changed, 1 insertion(+), 1 deletion(-)
```
