## Lab - Opensource Projects

### Task 1 - Clone remote repository

##### Step 1

Search for the following projects on GitHub and clone them inside your **jumphost** home directory:

- NAPALM
- Ansible
- netmiko
- ntc-templates

```bash
ntc@ntc:~$ git clone https://github.com/napalm-automation/napalm
Cloning into 'napalm'...
remote: Counting objects: 23931, done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 23931 (delta 4), reused 10 (delta 4), pack-reused 23921
Receiving objects: 100% (23931/23931), 5.10 MiB | 6.58 MiB/s, done.
Resolving deltas: 100% (12906/12906), done.
Checking connectivity... done.
ntc@ntc:~$
ntc@ntc:~$ git clone https://github.com/ansible/ansible
Cloning into 'ansible'...
remote: Counting objects: 292875, done.
remote: Compressing objects: 100% (8/8), done.
remote: Total 292875 (delta 1), reused 1 (delta 1), pack-reused 292866
Receiving objects: 100% (292875/292875), 99.90 MiB | 6.49 MiB/s, done.
Resolving deltas: 100% (185720/185720), done.
Checking connectivity... done.
ntc@ntc:~$
ntc@ntc:~$ git clone https://github.com/ktbyers/netmiko
Cloning into 'netmiko'...
remote: Counting objects: 7068, done.
remote: Compressing objects: 100% (15/15), done.
remote: Total 7068 (delta 5), reused 7 (delta 0), pack-reused 7053
Receiving objects: 100% (7068/7068), 2.06 MiB | 0 bytes/s, done.
Resolving deltas: 100% (4616/4616), done.
Checking connectivity... done.
ntc@ntc:~$
ntc@ntc:~$ git clone https://github.com/networktocode/ntc-templates
Cloning into 'ntc-templates'...
remote: Counting objects: 2803, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 2803 (delta 1), reused 0 (delta 0), pack-reused 2801
Receiving objects: 100% (2803/2803), 595.95 KiB | 0 bytes/s, done.
Resolving deltas: 100% (1446/1446), done.
Checking connectivity... done.
ntc@ntc:~$
```


##### Step 2

Move into the newly cloned `napalm` directory and check the origin.

```bash
ntc@ntc:~$ cd napalm/
ntc@ntc:napalm (develop)$
ntc@ntc:napalm (develop)$ git remote -v
origin  https://github.com/napalm-automation/napalm (fetch)
origin  https://github.com/napalm-automation/napalm (push)
```

Do the same for all other repositories.


##### Step 3

Move into the `ansible` directory and inspect its logs.

> The logs you see will likely be different from what is shown below.

```bash
ntc@ntc:napalm (develop)$ cd ../ansible/
ntc@ntc:ansible (devel)$
ntc@ntc:ansible (devel)$ git log
commit f849dc9cad93f512554823fe3f26f171b45dfcf9
Author: Peter Sprygada <privateip@users.noreply.github.com>
Date:   Fri Jan 26 06:33:44 2018 -0500

    add extattrs as an option when using nios lookup (#35371)

commit 0fb916c35e0105a754bf8afefb6269b8c60961ee
Author: Jordan Borean <jborean93@gmail.com>
Date:   Fri Jan 26 20:44:40 2018 +1000

    more Azure test fixes for Windows (#35386)

commit d2d825ccd3e3632891ff743f6597d82a37226bdd
Author: Christopher Brown <snecklifter@users.noreply.github.com>
Date:   Fri Jan 26 09:56:06 2018 +0000

    Add ManageIQ OpenStack provider (#34932)

    Adds the capability to create OpenStack providers. Because one
    of the parameters, keystone_v3_domain_id, is not currently
    exposed, it is added as an alias of azure_tenant_id which currently
    maps to uid_ems. Work is in progress to fix this.

    ManageIQ/manageiq#16802
    ManageIQ/manageiq-api#279
    ManageIQ/manageiq-providers-openstack#196

commit 1c8a872648dd274df00c587fc455e8d2a85f2e36
Author: Dag Wieers <dag@wieers.com>
Date:   Fri Jan 26 10:46:53 2018 +0100

    win_disk_facts: Prefix facts with ansible_ and use raw values (#35326)

    * win_disk_facts: Prefix facts with ansible_

    * Return raw values in bytes rather than formatted strings

    * Fix Fail-Json message

    * Improve examples

commit 466e1b289b85c0bfcbb669dd3afbe110282d0df6
Author: Reto Gantenbein <reto.gantenbein@linuxmonk.ch>
Date:   Fri Jan 26 09:49:51 2018 +0100

    Add thin pool / volume managment to lvol (#19312)

<---omitted--->
```


##### Step 4

Move into the `netmiko` directory and use log `--graph` option to visualize the branching.

> The logs you see will likely be different from what is shown below.

```bash
ntc@ntc:ansible (devel)$ cd ../netmiko/
ntc@ntc:netmiko (develop)$ git log --graph
*   commit af3be1db9a630116f191ed77b87c435a4c4d97b9
|\  Merge: fd46063 7370fdd
| | Author: Kirk Byers <ktbyers@twb-tech.com>
| | Date:   Tue Jan 23 20:22:31 2018 -0800
| |
| |     Merge pull request #685 from pathcl/develop
| |
| |     Dell s4048 ssh autodetect support
| |
| * commit 7370fdd88fe93e4d709de934502807e4059e39af
| | Author: Luis San Martin <luis@sanmartin.io>
| | Date:   Tue Jan 16 19:55:40 2018 -0300
| |
| |     S4048 SSH autodetect support
| |
* | commit fd46063a81b4efa1752650686e87441cd9112f4c
| | Author: Kirk Byers <ktbyers@twb-tech.com>
| | Date:   Tue Jan 23 19:30:59 2018 -0800
| |
| |     Restructure files
| |
* | commit 6d764bc22008fae393a25680615c0c049f2ca973
| | Author: Kirk Byers <ktbyers@twb-tech.com>
| | Date:   Tue Jan 23 19:26:15 2018 -0800
| |
| |     Restructuring some extreme items
| |
* |   commit 63341a7004b363855beb8c5665e4dde22d6ffef2
|\ \  Merge: a735a65 001d21d
| |/  Author: Kirk Byers <ktbyers@twb-tech.com>
|/|   Date:   Tue Jan 23 19:16:38 2018 -0800
| |
| |       Merge pull request #690 from fooelisa/develop
| |
| |       adding extreme telnet class
| |
| *   commit 001d21d615ced522a3de9a118353f4dceb028981
| |\  Merge: d3df024 a735a65
| |/  Author: Elisa Jasinska <elisa@jasinska.de>
|/|   Date:   Mon Jan 22 11:11:23 2018 -0400
| |
| |       merge upstream
| |
* |   commit a735a65a6aa26d4be56fae87bd4cf693941a1a46
|\ \  Merge: 1b8be9c 71a5f1b
| | | Author: Kirk Byers <ktbyers@twb-tech.com>
| | | Date:   Fri Jan 12 16:47:06 2018 -0800
<---omitted--->
```
