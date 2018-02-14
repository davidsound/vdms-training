## Lab - Managing `iptables` rules using `firewalld`

### Task 1 - Query and Add rules.

This task will take you through the process of using `firewalld` to query rule state and add a rule.


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
5     1703  148K ACCEPT     all  --  any    any     anywhere             anywhere             ctstate RELATED,ESTABLISHED
6      816 54676 ACCEPT     all  --  lo     any     anywhere             anywhere            
7       45  6836 INPUT_direct  all  --  any    any     anywhere             anywhere            
8       45  6836 INPUT_ZONES_SOURCE  all  --  any    any     anywhere             anywhere            
9       45  6836 INPUT_ZONES  all  --  any    any     anywhere             anywhere            
10       0     0 DROP       all  --  any    any     anywhere             anywhere             ctstate INVALID
11      39  6452 REJECT     all  --  any    any     anywhere             anywhere             reject-with icmp-host-prohibited
```


##### Step 3

Use `firewalld` to query the current rules to see if any rules affect `icmp` `echo-request`. 

```bash
[ntc@ntc ~]$ sudo firewall-cmd --query-icmp-block=echo-request
no
```

> Look at `sudo firewall-cmd --help` or `man firewall-cmd` to see all available query commands.

##### Step 4

Add a rule to block `icmp` requests to the `centos` host.

```bash
[ntc@ntc ~]$ sudo firewall-cmd --add-icmp-block=echo-request
success
```

> This tells IP tables to append to the `INPUT` chain a rule to `DROP` icmp traffic coming into any port.


##### Step 5

Use `firewalld` to query the current rules to see if any rules affect `icmp` `echo-request`. 

```bash
[ntc@ntc ~]$ sudo firewall-cmd --query-icmp-block=echo-request
yes
```


##### Step 6

Run the ping test again from the **jumphost** to `centos`.

```bash
ntc@ntc:~$ ping -c 5 centos
PING centos (192.168.0.52) 56(84) bytes of data.
From centos (192.168.0.52) icmp_seq=1 Destination Host Prohibited
From centos (192.168.0.52) icmp_seq=2 Destination Host Prohibited
From centos (192.168.0.52) icmp_seq=3 Destination Host Prohibited
From centos (192.168.0.52) icmp_seq=4 Destination Host Prohibited
From centos (192.168.0.52) icmp_seq=5 Destination Host Prohibited

--- centos ping statistics ---
5 packets transmitted, 0 received, +5 errors, 100% packet loss, time 4006ms
```

> The ping is now being blocked!


##### Step 7

Check the `centos` `iptables` `IN_public_deny` chain rules to see where the `REJECT` rule was placed.

```bash
[ntc@ntc ~]$ sudo iptables -vL IN_public_deny --line-numbers
Chain IN_public_deny (1 references)
num   pkts bytes target     prot opt in     out     source               destination         
1        5   420 REJECT     icmp --  any    any     anywhere             anywhere             icmp echo-request reject-with icmp-host-prohibited
```

> As you can see, the rule has rejected the 5 ping requests that were sent form the jumphost.









### Task 2 - Removes rules and verify.

This task will take you through the process of using `firewalld` to remove a rule and verify the rule has been removed.


##### Step 1

Test to ensure pings to `centos` form the jumphost are still being rejected.

```bash
ntc@ntc:~$ ping -c 5 centos
PING centos (192.168.0.52) 56(84) bytes of data.
From centos (192.168.0.52) icmp_seq=1 Destination Host Prohibited
From centos (192.168.0.52) icmp_seq=2 Destination Host Prohibited
From centos (192.168.0.52) icmp_seq=3 Destination Host Prohibited
From centos (192.168.0.52) icmp_seq=4 Destination Host Prohibited
From centos (192.168.0.52) icmp_seq=5 Destination Host Prohibited

--- centos ping statistics ---
5 packets transmitted, 0 received, +5 errors, 100% packet loss, time 4004ms
```


##### Step 2

Remove the previously added rule to block `echo-request`.

```bash
[ntc@ntc ~]$ sudo firewall-cmd --remove-icmp-block=echo-request
success
```


##### Step 3

Use `firewalld` to query the rule set for `echo-requests`.

```bash
[ntc@ntc ~]$ sudo firewall-cmd --query-icmp-block=echo-request 
no
```

> Notice the similarity between the commands to add, remove, and query a rule.


##### Step 4

Run the ping test again from the **jumphost** to `centos`.

```bash
ntc@ntc:~$ ping -c 5 centos
PING centos (192.168.0.52) 56(84) bytes of data.
64 bytes from centos (192.168.0.52): icmp_seq=1 ttl=64 time=1.27 ms
64 bytes from centos (192.168.0.52): icmp_seq=2 ttl=64 time=0.843 ms
64 bytes from centos (192.168.0.52): icmp_seq=3 ttl=64 time=1.05 ms
64 bytes from centos (192.168.0.52): icmp_seq=4 ttl=64 time=0.848 ms
64 bytes from centos (192.168.0.52): icmp_seq=5 ttl=64 time=0.836 ms

--- centos ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4007ms
rtt min/avg/max/mdev = 0.836/0.971/1.276/0.174 ms
```

> The ping is now unblocked!


##### Step 5

Check the `centos` `iptables` `IN_public_deny` chain rules to see that  the `REJECT` rule was removed.

```bash
[ntc@ntc ~]$ sudo iptables -vL IN_public_deny --line-numbers
[ntc@ntc ~]$ sudo iptables -vL IN_public_deny --line-numbers
Chain IN_public_deny (1 references)
num   pkts bytes target     prot opt in     out     source               destination 
```

