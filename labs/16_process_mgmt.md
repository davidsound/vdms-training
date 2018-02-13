# Lab 16 - System Processes

In this lab we will look into Linux process management, and build upon the previous system lab(s) we completed.  This lab will focus on foreground & background processes. 

### Task 1 - Foreground Processes

##### Step 1 - Time App

Running a process/application in  the foreground is simply a matter of starting that process. For example, if we run the `date` command.  It runs in the foreground, printing immediately:

```
[ntc@ntc ~]$ date
Thu Feb  8 07:48:22 UTC 2018
```

Lets create the equivalent Python script.  Open a file called `now.py` in your text editor of choice and copy the following:

```
import time
print(time.asctime(time.localtime(time.time())))
```
We can run it in the foreground by typing `python now.py`

```
[ntc@ntc ~]$ python now.py
Thu Feb  8 07:56:51 2018
```

Lets extend this script to print the time, every 10 seconds, for a minute.  Change our script to look like this:

```
import time

counter = range(1,61,10)

for num in counter:
    print(time.asctime(time.localtime(time.time())))
    time.sleep(10)

print('ALL DONE')
```

This script will now print the the time every 10 seconds, and end by printing `ALL DONE`.  Run it by typing `python now.py`

```
[ntc@ntc ~]$ python now.py
Thu Feb  8 08:00:12 2018
Thu Feb  8 08:00:22 2018
Thu Feb  8 08:00:32 2018
Thu Feb  8 08:00:42 2018
Thu Feb  8 08:00:52 2018
Thu Feb  8 08:01:02 2018
ALL DONE
```

This is considered a foreground process.  Notice how when we ran our script, our cursor disappeared, and we were not able to type any commands.  Foreground processes are good for short-running processes, debugging, monitoring, etc... but, for a long-running process, we may not want to tie up our shell session.

### Task 2 - Background Processes

##### Step 1 - Background Time App

Using our same `now.py` program, lets configure it to run in the background.  

While there are many ways to run background processes, a siple way to accomplish this is to append an ampersand `&` to the end of the command when calling the application. Lets do that, and then type `ls` and then `tree /etc/`:

```
[ntc@ntc ~]$ python now.py &
[1] 17600
[ntc@ntc ~]$ Thu Feb  8 08:17:08 2018
ls
now.py
[ntc@ntc ~] Thu Feb  8 08:17:18 2018
[ntc@ntc ~]$ tree /etc
/etc
├── adjtime
├── aliases
[... snipped for brevity ...]

182 directories, 492 files
[ntc@ntc ~]$ Thu Feb  8 08:17:28 2018
Thu Feb  8 08:17:38 2018
Thu Feb  8 08:17:48 2018
Thu Feb  8 08:17:58 2018
ALL DONE
```

We noticed that the program still executed - printing the date/time every 10 seconds, but we were able to access the shell session.  

While this is a trivial example, and the programming printing every few seconds would still be annoying, a more in depth application, like a webserver, could run in the background while we monitored files, access logs, etc... 

What if an application is running in the background, and we want to manipulate it?

### Task 3 - Process Manipulation

##### Step 1 - Background to Foreground

Lets relaunch our `now.py` application in the background:

```
[ntc@ntc ~]$ python now.py &
[1] 18264
[ntc@ntc ~]$ Thu Feb  8 08:30:54 2018
```

Imagine you wanted to monitor this application - perhaps to debug a problem, you could do this bringing it to the foreground.  To do this, while the application is running, type `fg`.

```
[ntc@ntc ~]$ python now.py &
[1] 18264
[ntc@ntc ~]$ Thu Feb  8 08:30:54 2018
Thu Feb  8 08:31:04 2018
fg
python now.py
Thu Feb  8 08:31:14 2018
Thu Feb  8 08:31:24 2018
Thu Feb  8 08:31:34 2018
Thu Feb  8 08:31:44 2018
ALL DONE
```

You'll notice that after typing `fg` the process name prints to screen, and takes over the shell session completely.  This can be useful for watching an application run

##### Step 2 - Job Monitoring

To take this a step further, consider that we have multiple processes running in the background.

We can view these by typing `jobs`

```
[ntc@ntc ~]$ python now.py &
[1] 19364
[ntc@ntc ~]$ Thu Feb  8 08:53:52 2018
jobs
[1]+  Running                 python now.py &
[ntc@ntc ~]$ Thu Feb  8 08:54:02 2018
```

##### Step 3 - Process ID

You may have noticed a number that is immediately displayed after we launch a process in the background.  This is the process id or `pid`

```
[ntc@ntc ~]$ python now.py &
[1] 19364 <-- Process ID
```
To view running processes run the `ps` command:

```
[ntc@ntc ~]$ ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 Feb07 ?        00:00:09 /usr/lib/systemd/systemd --switched-root --system --deseri
root         2     0  0 Feb07 ?        00:00:00 [kthreadd]
root         3     2  0 Feb07 ?        00:00:00 [ksoftirqd/0]
root         5     2  0 Feb07 ?        00:00:00 [kworker/0:0H]
root         6     2  0 Feb07 ?        00:00:00 [kworker/u4:0]
root         7     2  0 Feb07 ?        00:00:00 [migration/0]
root         8     2  0 Feb07 ?        00:00:00 [rcu_bh]
root         9     2  0 Feb07 ?        00:00:01 [rcu_sched]
root        10     2  0 Feb07 ?        00:00:00 [watchdog/0]
root        11     2  0 Feb07 ?        00:00:00 [watchdog/1]
root        12     2  0 Feb07 ?        00:00:00 [migration/1]
root        13     2  0 Feb07 ?        00:00:00 [ksoftirqd/1]
root        15     2  0 Feb07 ?        00:00:00 [kworker/1:0H]
root        17     2  0 Feb07 ?        00:00:00 [kdevtmpfs]
root        18     2  0 Feb07 ?        00:00:00 [netns]
root        19     2  0 Feb07 ?        00:00:00 [khungtaskd]
root        20     2  0 Feb07 ?        00:00:00 [writeback]
root        21     2  0 Feb07 ?        00:00:00 [kintegrityd]
ntc      20547  3586  0 09:17 pts/0    00:00:00 ps -ef
```

The 1st column indicates the user running the process, and the 2nd, `PID` indicates the process or job id.

#### Step 4 - Kill Processes

To stop a running process, we can use the `kill` or `pkill` commands.

Lets change our `now.py` application to run longer:

```
[ntc@ntc ~]$ cat now.py 
import time

counter = range(1,3601,10)

for num in counter:
    print(time.asctime(time.localtime(time.time())))
    time.sleep(10)

print('ALL DONE')
```

It will now run for an hour.

Start it by issuing the `python now.py &` command. 

```
[ntc@ntc ~]$ python now.py &
[1] 20827
[ntc@ntc ~]$ Thu Feb  8 09:23:25 2018
```

We get a PID of 20827.  If we want to cancel this job, we can use the `kill` command, followed by the PID:

```
[ntc@ntc ~]$ Thu Feb  8 09:23:35 2018
Thu Feb  8 09:23:45 2018
Thu Feb  8 09:23:55 2018
Thu Feb  8 09:24:05 2018
Thu Feb  8 09:24:15 2018
Thu Feb  8 09:24:25 2018
Thu Feb  8 09:24:35 2018
Thu Feb  8 09:24:45 2018
kill 20827

[ntc@ntc ~]$
```

The process exits immediately


##### Step 4 - Pkill

`pkill` works similar to `kill` but can be used to search for a process, in case you don't know the PID.

```
[ntc@ntc ~]$ pkill -h

Usage:
 pkill [options] <pattern>

Options:
 -<sig>, --signal <sig>    signal to send (either number or name)
 -e, --echo                display what is killed
 -c, --count               count of matching processes
 -f, --full                use full process name to match
 -g, --pgroup <PGID,...>   match listed process group IDs
 -G, --group <GID,...>     match real group IDs
 -n, --newest              select most recently started
 -o, --oldest              select least recently started
 -P, --parent <PPID,...>   match only child processes of the given parent
 -s, --session <SID,...>   match session IDs
 -t, --terminal <tty,...>  match by controlling terminal
 -u, --euid <ID,...>       match by effective IDs
 -U, --uid <ID,...>        match by real IDs
 -x, --exact               match exactly with the command name
 -F, --pidfile <file>      read PIDs from file
 -L, --logpidfile          fail if PID file is not locked
 --ns <PID>                match the processes that belong to the same
                           namespace as <pid>
 --nslist <ns,...>         list which namespaces will be considered for
                           the --ns option.
                           Available namespaces: ipc, mnt, net, pid, user, uts

 -h, --help     display this help and exit
 -V, --version  output version information and exit

For more details see pgrep(1).
```

With `pkill`, you can kill processes running under a certain user id, running for a certain amount of time, or other factors
