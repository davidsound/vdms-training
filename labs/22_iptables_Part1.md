## Lab - Using `iptables` in \*nux

Iptables  are used to set up, maintain, and inspect the tables of packet filter rules in the Linux kernel.  Several different tables may be defined.  Each table contains a number of built-in chains and may also contain user-defined chains.

** Note: The hosts in the lab are running straight `iptables` without any of the management packages such as `firewalld` or `ufw`.  These commands are standard for `iptables` as an application.  `iptables` is available on multiple platforms including `centos` and `ubuntu`.**



### Task 1 - Investigate chains and rules using `iptables` commands

The default table is the `filter` table and it contains the built-in chains `INPUT`, `FORWARD`, and `OUTPUT`. The `INPUT` chain is used for packets destined to local sockets, the `FORWARD` chain is used for packets being routed through the box while the `OUTPUT` chain is used for locally-generated packets.

**This section takes place on your `centos` host.**

##### Step 1

List the rules for each chain (`INPUT`, `FORWARD`, `OUTPUT`) using the the command `sudo iptables -L` followed by the name of the chain.

> If you do not specify a chain, all will be listed.

```bash
[ntc@ntc ~]$ sudo iptables -L INPUT
[sudo] password for ntc: 
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         
ACCEPT     udp  --  anywhere             anywhere             udp dpt:domain
ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:domain
ACCEPT     udp  --  anywhere             anywhere             udp dpt:bootps
ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:bootps
ACCEPT     all  --  anywhere             anywhere             ctstate RELATED,ESTABLISHED
ACCEPT     all  --  anywhere             anywhere            
INPUT_direct  all  --  anywhere             anywhere            
INPUT_ZONES_SOURCE  all  --  anywhere             anywhere            
INPUT_ZONES  all  --  anywhere             anywhere            
DROP       all  --  anywhere             anywhere             ctstate INVALID
REJECT     all  --  anywhere             anywhere             reject-with icmp-host-prohibited
```

```bash
[ntc@ntc ~]$ sudo iptables -L FORWARD
Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         
ACCEPT     all  --  anywhere             192.168.122.0/24     ctstate RELATED,ESTABLISHED
ACCEPT     all  --  192.168.122.0/24     anywhere            
ACCEPT     all  --  anywhere             anywhere            
REJECT     all  --  anywhere             anywhere             reject-with icmp-port-unreachable
REJECT     all  --  anywhere             anywhere             reject-with icmp-port-unreachable
ACCEPT     all  --  anywhere             anywhere             ctstate RELATED,ESTABLISHED
ACCEPT     all  --  anywhere             anywhere            
FORWARD_direct  all  --  anywhere             anywhere            
FORWARD_IN_ZONES_SOURCE  all  --  anywhere             anywhere            
FORWARD_IN_ZONES  all  --  anywhere             anywhere            
FORWARD_OUT_ZONES_SOURCE  all  --  anywhere             anywhere            
FORWARD_OUT_ZONES  all  --  anywhere             anywhere            
DROP       all  --  anywhere             anywhere             ctstate INVALID
REJECT     all  --  anywhere             anywhere             reject-with icmp-host-prohibited
```

> The `REJECT` rule at the bottom denies anything that is not previously matched by an `ACCEPT` rule.

```bash
[ntc@ntc ~]$ sudo iptables -L OUTPUT
Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination         
ACCEPT     udp  --  anywhere             anywhere             udp dpt:bootpc
OUTPUT_direct  all  --  anywhere             anywhere
```


##### Step 2

Now focus on the `INPUT` chain.  To be able to more easily manage the chain, list the rules with line numbers using the `--line-numbers` option.

```bash
[ntc@ntc ~]$ sudo iptables -L INPUT --line-numbers
Chain INPUT (policy ACCEPT)
num  target     prot opt source               destination         
1    ACCEPT     udp  --  anywhere             anywhere             udp dpt:domain
2    ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:domain
3    ACCEPT     udp  --  anywhere             anywhere             udp dpt:bootps
4    ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:bootps
5    ACCEPT     all  --  anywhere             anywhere             ctstate RELATED,ESTABLISHED
6    ACCEPT     all  --  anywhere             anywhere            
7    INPUT_direct  all  --  anywhere             anywhere            
8    INPUT_ZONES_SOURCE  all  --  anywhere             anywhere            
9    INPUT_ZONES  all  --  anywhere             anywhere            
10   DROP       all  --  anywhere             anywhere             ctstate INVALID
11   REJECT     all  --  anywhere             anywhere             reject-with icmp-host-prohibited
```

This command shows a reference number for each rule in the chain.  This number can be used to insert/delete rules more precisely.


##### Step 3

It can also be helpful to see the counters for a chain.  Again, focusing on the `INPUT` chain, show the chain in `verbose` mode using the `-v` option.

```bash
[ntc@ntc ~]$ sudo iptables -vL INPUT --line-numbers
Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
num   pkts bytes target     prot opt in     out     source               destination         
1        0     0 ACCEPT     udp  --  virbr0 any     anywhere             anywhere             udp dpt:domain
2        0     0 ACCEPT     tcp  --  virbr0 any     anywhere             anywhere             tcp dpt:domain
3        0     0 ACCEPT     udp  --  virbr0 any     anywhere             anywhere             udp dpt:bootps
4        0     0 ACCEPT     tcp  --  virbr0 any     anywhere             anywhere             tcp dpt:bootps
5     5519  433K ACCEPT     all  --  any    any     anywhere             anywhere             ctstate RELATED,ESTABLISHED
6      596 39940 ACCEPT     all  --  lo     any     anywhere             anywhere            
7       46  7638 INPUT_direct  all  --  any    any     anywhere             anywhere            
8       46  7638 INPUT_ZONES_SOURCE  all  --  any    any     anywhere             anywhere            
9       46  7638 INPUT_ZONES  all  --  any    any     anywhere             anywhere            
10       0     0 DROP       all  --  any    any     anywhere             anywhere             ctstate INVALID
11      35  6978 REJECT     all  --  any    any     anywhere             anywhere             reject-with icmp-host-prohibited
```

Verbose mode adds the `pkts` and `bytes` counters for each rule.
