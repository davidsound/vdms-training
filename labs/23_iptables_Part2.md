## Lab - Managing `iptables` rules 

### Task 1 - Add rules.

This task will take you through the process of using `iptables` to manage rules.

**NOTE:  These commands are standard for `iptables` as an application.  `iptables` is available on multiple platforms including `centos` and `ubuntu`.** 


##### Step 1

First test to ensure you can ping `centos` from your **jumphost**.

```bash
ntc@ntc:~$ ping centos
PING centos (192.168.0.52) 56(84) bytes of data.
64 bytes from centos (192.168.0.52): icmp_seq=1 ttl=64 time=1.65 ms
64 bytes from centos (192.168.0.52): icmp_seq=2 ttl=64 time=0.931 ms
64 bytes from centos (192.168.0.52): icmp_seq=3 ttl=64 time=0.863 ms

^C--- centos ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 0.863/1.149/1.654/0.359 ms
```


##### Step 2

Now, `ssh` into the `centos` host and check your `INPUT` chain rules.

```bash
[ntc@ntc ~]$ sudo iptables -vL INPUT --line-numbers 
Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
num   pkts bytes target     prot opt in     out     source               destination         
1        0     0 ACCEPT     udp  --  virbr0 any     anywhere             anywhere             udp dpt:domain
2        0     0 ACCEPT     tcp  --  virbr0 any     anywhere             anywhere             tcp dpt:domain
3        0     0 ACCEPT     udp  --  virbr0 any     anywhere             anywhere             udp dpt:bootps
4        0     0 ACCEPT     tcp  --  virbr0 any     anywhere             anywhere             tcp dpt:bootps
5        0     0 ACCEPT     udp  --  virbr0 any     anywhere             anywhere             udp dpt:domain
6        0     0 ACCEPT     tcp  --  virbr0 any     anywhere             anywhere             tcp dpt:domain
7        0     0 ACCEPT     udp  --  virbr0 any     anywhere             anywhere             udp dpt:bootps
8        0     0 ACCEPT     tcp  --  virbr0 any     anywhere             anywhere             tcp dpt:bootps
9        0     0 ACCEPT     udp  --  virbr0 any     anywhere             anywhere             udp dpt:domain
10       0     0 ACCEPT     tcp  --  virbr0 any     anywhere             anywhere             tcp dpt:domain
11       0     0 ACCEPT     udp  --  virbr0 any     anywhere             anywhere             udp dpt:bootps
12       0     0 ACCEPT     tcp  --  virbr0 any     anywhere             anywhere             tcp dpt:bootps
13       0     0 ACCEPT     tcp  --  any    any     anywhere             anywhere             tcp dpts:commplex-main:x11 ctstate NEW,ESTABLISHED
14     103  9085 ACCEPT     all  --  any    any     anywhere             anywhere             state RELATED,ESTABLISHED
15       0     0 ACCEPT     icmp --  any    any     anywhere             anywhere            
16       0     0 ACCEPT     all  --  lo     any     anywhere             anywhere            
17       1    60 ACCEPT     tcp  --  any    any     anywhere             anywhere             state NEW tcp dpt:ssh
18      41  7739 REJECT     all  --  any    any     anywhere             anywhere             reject-with icmp-host-prohibited
```


##### Step 3

Add a rule to block `icmp` requests to the `centos` host.

```bash
[ntc@ntc ~]$ sudo  iptables -I INPUT 15 -p icmp --icmp-type echo-request -j DROP
```

> This tells `iptables` to insert to the `INPUT` chain a rule to `DROP` icmp `echo-request` traffic coming into any port.  This new rule will be inserted before rule 15.  This is because, in this example, rule 15 allows all icmp traffic.


##### Step 4

Run the ping test again from the **jumphost** to `centos`.

```bash
ntc@ntc:~$ ping centos
PING centos (192.168.0.52) 56(84) bytes of data.

^C--- centos ping statistics ---
5 packets transmitted, 0 received, 100% packet loss, time 4032ms
```

> The ping is now being blocked!


##### Step 7

Check the `centos` `iptables` `INPUT` chain rules to see where the `DROP` rule was placed.

```bash
Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
num   pkts bytes target     prot opt in     out     source               destination         
1        0     0 ACCEPT     udp  --  virbr0 any     anywhere             anywhere             udp dpt:domain
2        0     0 ACCEPT     tcp  --  virbr0 any     anywhere             anywhere             tcp dpt:domain
3        0     0 ACCEPT     udp  --  virbr0 any     anywhere             anywhere             udp dpt:bootps
4        0     0 ACCEPT     tcp  --  virbr0 any     anywhere             anywhere             tcp dpt:bootps
5        0     0 ACCEPT     udp  --  virbr0 any     anywhere             anywhere             udp dpt:domain
6        0     0 ACCEPT     tcp  --  virbr0 any     anywhere             anywhere             tcp dpt:domain
7        0     0 ACCEPT     udp  --  virbr0 any     anywhere             anywhere             udp dpt:bootps
8        0     0 ACCEPT     tcp  --  virbr0 any     anywhere             anywhere             tcp dpt:bootps
9        0     0 ACCEPT     udp  --  virbr0 any     anywhere             anywhere             udp dpt:domain
10       0     0 ACCEPT     tcp  --  virbr0 any     anywhere             anywhere             tcp dpt:domain
11       0     0 ACCEPT     udp  --  virbr0 any     anywhere             anywhere             udp dpt:bootps
12       0     0 ACCEPT     tcp  --  virbr0 any     anywhere             anywhere             tcp dpt:bootps
13       0     0 ACCEPT     tcp  --  any    any     anywhere             anywhere             tcp dpts:commplex-main:x11 ctstate NEW,ESTABLISHED
14     365 27853 ACCEPT     all  --  any    any     anywhere             anywhere             state RELATED,ESTABLISHED
15       5   420 DROP       icmp --  any    any     anywhere             anywhere             icmp echo-request
16       0     0 ACCEPT     icmp --  any    any     anywhere             anywhere            
17       0     0 ACCEPT     all  --  lo     any     anywhere             anywhere            
18       1    60 ACCEPT     tcp  --  any    any     anywhere             anywhere             state NEW tcp dpt:ssh
19      41  7739 REJECT     all  --  any    any     anywhere             anywhere             reject-with icmp-host-prohibited
```

> As you can see, the rule has rejected the 5 ping requests that were sent form the jumphost (Rule 15).









### Task 2 - Remove rules and verify.

This task will take you through the process of using `iptables` to remove a rule and verify the rule has been removed.


##### Step 1

Test to ensure pings to `centos` form the **jumphost** are still being rejected.

```bash
ntc@ntc:~$ ping centos
PING centos (192.168.0.52) 56(84) bytes of data.

^C--- centos ping statistics ---
5 packets transmitted, 0 received, 100% packet loss, time 4032ms
```


##### Step 2

Remove the previously added rule to block `echo-request` on the `centos` host.

```bash
[ntc@ntc ~]$ sudo iptables -D INPUT 15
```


##### Step 3

Run the ping test again from the **jumphost** to `centos`.

```bash
ntc@ntc:~$ ping centos
PING centos (192.168.0.52) 56(84) bytes of data.
64 bytes from centos (192.168.0.52): icmp_seq=1 ttl=64 time=0.983 ms
64 bytes from centos (192.168.0.52): icmp_seq=2 ttl=64 time=0.710 ms
64 bytes from centos (192.168.0.52): icmp_seq=3 ttl=64 time=0.609 ms
64 bytes from centos (192.168.0.52): icmp_seq=4 ttl=64 time=0.624 ms
64 bytes from centos (192.168.0.52): icmp_seq=5 ttl=64 time=0.567 ms
^C
--- centos ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4002ms
rtt min/avg/max/mdev = 0.567/0.698/0.983/0.152 ms
```

> The ping is now unblocked!


##### Step 5

Check the `centos` `iptables` `INPUT` chain rules to see that  the `DROP` rule was removed.

```bash
Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
num   pkts bytes target     prot opt in     out     source               destination         
1        0     0 ACCEPT     udp  --  virbr0 any     anywhere             anywhere             udp dpt:domain
2        0     0 ACCEPT     tcp  --  virbr0 any     anywhere             anywhere             tcp dpt:domain
3        0     0 ACCEPT     udp  --  virbr0 any     anywhere             anywhere             udp dpt:bootps
4        0     0 ACCEPT     tcp  --  virbr0 any     anywhere             anywhere             tcp dpt:bootps
5        0     0 ACCEPT     udp  --  virbr0 any     anywhere             anywhere             udp dpt:domain
6        0     0 ACCEPT     tcp  --  virbr0 any     anywhere             anywhere             tcp dpt:domain
7        0     0 ACCEPT     udp  --  virbr0 any     anywhere             anywhere             udp dpt:bootps
8        0     0 ACCEPT     tcp  --  virbr0 any     anywhere             anywhere             tcp dpt:bootps
9        0     0 ACCEPT     udp  --  virbr0 any     anywhere             anywhere             udp dpt:domain
10       0     0 ACCEPT     tcp  --  virbr0 any     anywhere             anywhere             tcp dpt:domain
11       0     0 ACCEPT     udp  --  virbr0 any     anywhere             anywhere             udp dpt:bootps
12       0     0 ACCEPT     tcp  --  virbr0 any     anywhere             anywhere             tcp dpt:bootps
13       0     0 ACCEPT     tcp  --  any    any     anywhere             anywhere             tcp dpts:commplex-main:x11 ctstate NEW,ESTABLISHED
14     461 34553 ACCEPT     all  --  any    any     anywhere             anywhere             state RELATED,ESTABLISHED
15       2   168 ACCEPT     icmp --  any    any     anywhere             anywhere            
16       0     0 ACCEPT     all  --  lo     any     anywhere             anywhere            
17       1    60 ACCEPT     tcp  --  any    any     anywhere             anywhere             state NEW tcp dpt:ssh
18      41  7739 REJECT     all  --  any    any     anywhere             anywhere             reject-with icmp-host-prohibited
```

### Task 3 - Save and make rules persistent


Rules placed into `iptables` are not automatically persistent and will be erased upon reboot.

##### Step 1

On the `centos` host save your rules and reload the service.

```bash
[ntc@ntc ~]$ sudo service iptables save   
iptables: Saving firewall rules to /etc/sysconfig/iptables:[  OK  ]
[ntc@ntc ~]$ sudo service iptables reload
Redirecting to /bin/systemctl reload iptables.service
```

Now the rules will be persistent across reboots.

**Bonus**

To make rules persistant on an `ubuntu` host you would use the following commands.

```bash
ntc@ubuntu:~$ sudo netfilter-persistent save
```