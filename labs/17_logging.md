# Lab 17 - Log files

In this lab we will examine log files in more detail.  Log files provide critical information about Linux applications and systems.  They can prove invaluable when troubleshooting and debugging.

### Task 1 - Syslog

##### Step 1 - View System Logs

The `/var/log/syslog` (Ubuntu/Debian) and `/var/log/messages` (RedHat/CentOS) files provides an aggregate log of many applications & processes.  Let's have a look:

```
[ntc@ntc ~]$ sudo cat /var/log/messages
Feb  7 15:22:31 localhost journal: Runtime journal is using 8.0M (max allowed 189.5M, trying to leave 284.3M free of 1.8G available → current limit 189.5M).
Feb  7 15:22:31 localhost kernel: Initializing cgroup subsys cpuset
Feb  7 15:22:31 localhost kernel: Initializing cgroup subsys cpu
Feb  7 15:22:31 localhost kernel: Initializing cgroup subsys cpuacct
Feb  7 15:22:31 localhost kernel: Linux version 3.10.0-693.11.6.el7.x86_64 (builder@kbuilder.dev.centos.org) (gcc version 4.8.5 20150623 (Red Hat 4.8.5-16) (GCC) ) #1 SMP Thu Jan 4 01:06:37 UTC 2018
Feb  7 15:22:31 localhost kernel: Command line: BOOT_IMAGE=/vmlinuz-3.10.0-693.11.6.el7.x86_64 root=/dev/mapper/VolGroup00-LogVol00 ro no_timer_check console=tty0 console=ttyS0,115200n8 net.ifnames=0 biosdevname=0 crashkernel=auto rd.lvm.lv=VolGroup00/LogVol00 rd.lvm.lv=VolGroup00/LogVol01 rhgb quiet
Feb  7 15:22:31 localhost kernel: e820: BIOS-provided physical RAM map:
[... snipped for brevity ...]
```

##### Step 2 - Tail

Viewing static logfiles may work for historical information, but at times we may want to view log entries as they happen.  To do this, we can use the `tail` command.

```
[ntc@ntc ~]$ sudo tail -f /var/log/messages
Feb  8 17:01:01 localhost systemd: Started Session 32 of user root.
Feb  8 17:01:01 localhost systemd: Starting Session 32 of user root.
Feb  8 17:01:01 localhost systemd: Removed slice User Slice of root.
Feb  8 17:01:01 localhost systemd: Stopping User Slice of root.
Feb  8 18:01:01 localhost systemd: Created slice User Slice of root.
Feb  8 18:01:01 localhost systemd: Starting User Slice of root.
Feb  8 18:01:01 localhost systemd: Started Session 33 of user root.
Feb  8 18:01:01 localhost systemd: Starting Session 33 of user root.
Feb  8 18:01:01 localhost systemd: Removed slice User Slice of root.
Feb  8 18:01:01 localhost systemd: Stopping User Slice of root.
```

##### Step 3 - Logger Facility

Let's use the `now.py` script created earlier to  print the time/date to a log file.  The `logger` command can be used to do this.  Let's print the time to the `/var/log/maillog` file.

The `logger` command has the following options:

```
[ntc@ntc ~]$ logger -h

Usage:
 logger [options] [message]

Options:
 -T, --tcp             use TCP only
 -d, --udp             use UDP only
 -i, --id              log the process ID too
 -f, --file <file>     log the contents of this file
 -h, --help            display this help text and exit
 -S, --size <num>      maximum size for a single message (default 1024)
 -n, --server <name>   write to this remote syslog server
 -P, --port <port>     use this port for UDP or TCP connection
 -p, --priority <prio> mark given message with this priority
 -s, --stderr          output message to standard error as well
 -t, --tag <tag>       mark every line with this tag
 -u, --socket <socket> write to this Unix socket
 -V, --version         output version information and exit

[ntc@ntc ~]$
```

Let's change our `now.py` program to use the logger:

```
import time
import os

counter = range(1,3601,10)

for num in counter:
    right_now = time.asctime(time.localtime(time.time()))
    os.system("logger -i -p mail.err %s" % right_now)
    time.sleep(10)

print('ALL DONE')
```

Let's run this in the background, and monitor the `/var/log/maillog` for info:

```
[ntc@ntc ~]$ python now.py &
[1] 18503
[ntc@ntc ~]$ sudo tail -f /var/log/maillog 
Feb  7 15:22:37 localhost postfix/postfix-script[1048]: starting the Postfix mail system
Feb  7 15:22:37 localhost postfix/master[1050]: daemon started -- version 2.10.1, configuration /etc/postfix
Feb  8 18:46:43 localhost vagrant[15722]: Thu Feb 8 18:46:43 2018
Feb  8 19:43:08 localhost vagrant[18504]: Thu Feb 8 19:43:08 2018
Feb  8 19:43:18 localhost vagrant[18513]: Thu Feb 8 19:43:18 2018
Feb  8 19:43:28 localhost vagrant[18525]: Thu Feb 8 19:43:28 2018
Feb  8 19:43:39 localhost vagrant[18536]: Thu Feb 8 19:43:39 2018
Feb  8 19:43:49 localhost vagrant[18545]: Thu Feb 8 19:43:49 2018
```

As we can see, the `/var/log/maillog` file would continue to populate for the duration of the program. 

Try using the `tail` command on the following files:

* `/var/log/secure`
* `/var/log/cron`
* `/var/log/yum.log`
