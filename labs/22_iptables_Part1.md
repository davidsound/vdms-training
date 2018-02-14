## Lab - Using `iptables` in \*nux

Iptables  are used to set up, maintain, and inspect the tables of packet filter rules in the Linux kernel.  Several different tables may be defined.  Each table contains a number of built-in chains and may also contain user-defined chains.

** Note: Centos7 uses a firewall mangement interface named `firewalld`.  This uses `iptables` behind the scenes to control access.  You will be able to investigate rules through standard `iptables` commands, but must use `firewalld` to add/move/change rules. **



### Task 1 - Investigate chains and rules using `iptables` commands

The default table is the `filter` table and it contains the built-in chains `INPUT`, `FORWARD`, and `OUTPUT`. The `INPUT` chain is used for packets destined to local sockets, the `FORWARD` chain is used for packets being routed through the box while the `OUTPUT` chain is used for locally-generated packets.

This section takes place on your `centos` host.

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










### Task 2 - Investigate rules using `firewalld` commands

This section takes place on your `centos` host.


##### Step 1

Check the status of `firewalld`.

```bash
[ntc@ntc ~]$ sudo firewall-cmd  --state
running
```


##### Step 2

`Firewalld` uses the concept of `zones` for managing rules.  This is then translated into `iptables`.

Check for active zones and interfaces in those zones.

```bash
[ntc@ntc ~]$ sudo firewall-cmd --get-active-zones
public
  interfaces: ens4
```

> The `centos` host only has one zone, `public`, with the `ens4` interface in that zone.


##### Step 3

Now check which rules are currently added to the `public` zone.

```bash
[ntc@ntc ~]$ sudo firewall-cmd --zone=public --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: ens4
  sources: 
  services: ssh dhcpv6-client
  ports: 
  protocols: 
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 
```

From the above output you will see that there are currently no rules in place.


##### Step 4

List all services that are available for configuration within `firewalld`.

> `Firewalld` can create rules based on services or direct port entry.  Service profiles allow for a standard format for standard services as well as custom services that may be used across an organization.

```bash
[ntc@ntc ~]$ sudo  firewall-cmd --get-services
[sudo] password for ntc: 
RH-Satellite-6 amanda-client amanda-k5-client bacula bacula-client bitcoin bitcoin-rpc bitcoin-testnet bitcoin-testnet-rpc ceph ceph-mon cfengine condor-collector ctdb dhcp dhcpv6 dhcpv6-client dns docker-registry dropbox-lansync elasticsearch freeipa-ldap freeipa-ldaps freeipa-replication freeipa-trust ftp ganglia-client ganglia-master high-availability http https imap imaps ipp ipp-client ipsec iscsi-target kadmin kerberos kibana klogin kpasswd kshell ldap ldaps libvirt libvirt-tls managesieve mdns mosh mountd ms-wbt mssql mysql nfs nrpe ntp openvpn ovirt-imageio ovirt-storageconsole ovirt-vmconsole pmcd pmproxy pmwebapi pmwebapis pop3 pop3s postgresql privoxy proxy-dhcp ptp pulseaudio puppetmaster quassel radius rpc-bind rsh rsyncd samba samba-client sane sip sips smtp smtp-submission smtps snmp snmptrap spideroak-lansync squid ssh synergy syslog syslog-tls telnet tftp tftp-client tinc tor-socks transmission-client vdsm vnc-server wbem-https xmpp-bosh xmpp-client xmpp-local xmpp-server
```

> **Tip:** The service definition `xml` files can be found in `/usr/lib/firewalld/services/`
