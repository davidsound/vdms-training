## Ping Scripting Lab 

In this lab we will build a `bash` script that will ping hosts and return their status. 

### Task 1 - `ping.sh`

#### Step 1

Perform a basic ping command from your `centos` machine to `192.168.0.51` with a count of `1`.

```bash
[ntc@ntc ~]$ ping -c 1 192.168.0.51
PING 192.168.0.51 (192.168.0.51) 56(84) bytes of data.
64 bytes from 192.168.0.51: icmp_seq=1 ttl=64 time=1.26 ms

--- 192.168.0.51 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 1.265/1.265/1.265/0.000 ms
```

This shows that we have a successful ping to `192.168.0.51`.

##### Step 2

Echo the `$?` variable.

> This is a variable that holds the exit status of the last command, in this case our ping.

```bash
[ntc@ntc ~]$ echo $?
0
```

`0` is the exit status for a successful command.

> For `ping` there are multiple exit statuses depending on the reply received from the host (block, rejected, etc).  `0` is a success and for this case anything else can be considered a failure.


##### Step 3

You can use this information to form a basic ping script on the `centos` host.

Build a basic script named `ping.sh` to run a ping command and return the output.

```bash
#! /usr/bin/env bash

ping -c 1 192.168.0.51 > /dev/null 2>&1

if [ $? -eq 0 ]
then
  echo "host is up"
else
  echo "host is down"
fi
```

> Don't forget that you will need to `chmod +x` the script to make it executable.

You will see that `> /dev/null 2>&1` has been added to the end of the `ping` command.  This will send the stdout to the null buffer instead of printing it to the screen.

After the ping is run, there is an `if` statement that looks to see if the exit status of the ping command was a `0` or not.  If it is a `0` is echos that the host is up, if not it is assumed down.

```bash
[ntc@ntc ~]$ ./ping.sh 
host is up
```


##### Step 4

Extend the `ping.sh` script to use a list of host IPs.  This will also show the use of a `for` loop within the shell script.

The IPs to use for the hosts list are `192.168.0.51`, `10.0.0.11`, `192.168.0.52`, `4.2.2.2`, `10.0.0.14`, and `192.168.0.54`.

```bash
#! /usr/bin/env bash
#
# A Script to ping list of IPs


hosts=(
    "192.168.0.51"
    "10.0.0.11"
    "192.168.0.52"
    "4.2.2.2"
    "10.0.0.14"
    "192.168.0.54"
    )

for host in ${hosts[@]}; do

  ping -c 1 "${host}" > /dev/null 2>&1

  if [ $? -eq 0 ]
  then
    echo "${host} is up"
  else
    echo "${host} is down"
  fi

done
```

Notice the structure of the `hosts` array. This was written for readability, but it could also be constructed on a single line useing spaces.  For example, `hosts=("192.168.0.51" "10.0.0.11" "192.168.0.52" "4.2.2.2" "10.0.0.14" "192.168.0.54")`.

The `for` loop goes through the `hosts` array and runs a ping on each IP, or `host`, in the array.

Once we pull each `host` out of the array it is now a variable available via `${host}` and can be used in the ping command where we would have used the IP address itself.

Also, since we are going through multiple hosts, there needs to be a feedback loop to show which host is up or down.  Again, we use the `${host}` variable in the `echo` command to specify which host is up or down.

Here is an example of the output once you complete the script.

```bash
[ntc@ntc ~]$ ./ping.sh 
192.168.0.51 is up
10.0.0.11 is up
192.168.0.52 is up
4.2.2.2 is up
10.0.0.14 is down
192.168.0.54 is down
```


##### Step 6

Extend `ping.sh` further by adding a function to handle the `ping` action.

A function is a section of a program that performs a specific task.  Functions often can be reused multiple times within a program or used in other programs to server a purpose without recreating code.

Here is the snippet of the function that will be created.

```bash
host_is_up() {
  # Send one ping (-c) and wait at most 1 second (-w)
  ping -c 1 -w 1 "${host}" > /dev/null 2>&1
}
```

The name of the function is `host_is_up()`.  We will pass the variable `host` to the function, and the function will return the exit code to the prgram for further processing.

```bash
#! /usr/bin/env bash
#
# A Script to ping list of IPs


hosts=(
    "192.168.0.51"
    "10.0.0.11"
    "192.168.0.52"
    "4.2.2.2"
    "10.0.0.14"
    "192.168.0.54"
    )

host_is_up() {
  # Send one ping (-c) and wait at most 1 second (-w)
  ping -c 1 -w 1 "${host}" > /dev/null 2>&1
}

for host in ${hosts[@]}; do

  ping -c 1 "${host}" > /dev/null 2>&1

  if host_is_up "${host}"
  then
    echo "${host} is up"
  else
    echo "${host} is down"
  fi

done
```

See the changes that are made to the structure of the script when adding and using functions.  In many cases functions can make code easier to extend and maintain.