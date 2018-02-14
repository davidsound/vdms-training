## Lab - Networking on Linux

In this lab you will learn how to configure network interfaces, shut down and bring them back up. You will also learn how to ensure that these settings can be persisted across reboots.

You will learn how to configure static routes on a multi-homed system and persist the routes across boots using configuration files.



### Task 1 - DHCP and DNS

In this task you will use the `dhclient` program to dynamically assign IP address to an interface, set up DNS resolution and a single default gateway.




##### Step 1

Check the route on the device:

```
[ntc@ntc ~]$ ip route
192.168.0.0/24 dev ens4 proto kernel scope link src 192.168.0.52 
192.168.122.0/24 dev virbr0 proto kernel scope link src 192.168.122.1 
[ntc@ntc ~]$ 

```

As you can tell, there is no default route on the server.


##### Step 2

Try and ping `yahoo.com`.

```
[ntc@ntc ~]$ ping yahoo.com
^C
[ntc@ntc ~]$ 

```

This will fail for 2 reasons: 

- The current default route has no path to the internet
- The DNS resolution does not work with the existing configuration.



##### Step 2

List the current interfaces on the device:

```
[ntc@ntc ~]$ ip link 
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: ens3: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT qlen 1000
    link/ether 2c:c2:60:3e:dd:80 brd ff:ff:ff:ff:ff:ff
3: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT qlen 1000
    link/ether 2c:c2:60:38:11:af brd ff:ff:ff:ff:ff:ff
4: virbr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DEFAULT qlen 1000
    link/ether 52:54:00:ed:da:3f brd ff:ff:ff:ff:ff:ff
5: virbr0-nic: <BROADCAST,MULTICAST> mtu 1500 qdisc pfifo_fast master virbr0 state DOWN mode DEFAULT qlen 1000
    link/ether 52:54:00:ed:da:3f brd ff:ff:ff:ff:ff:ff
[ntc@ntc ~]$ 

```

> The only enabled interface is `ens4` which is an internal network path.



##### Step 3

List the current ip addresses on the device:


```
[ntc@ntc ~]$ ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens3: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN qlen 1000
    link/ether 2c:c2:60:3e:dd:80 brd ff:ff:ff:ff:ff:ff
3: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether 2c:c2:60:38:11:af brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.52/24 brd 192.168.0.255 scope global dynamic ens4
       valid_lft 359415sec preferred_lft 359415sec
    inet6 fe80::7070:148a:e15b:b2fb/64 scope link 
       valid_lft forever preferred_lft forever
4: virbr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN qlen 1000
    link/ether 52:54:00:ed:da:3f brd ff:ff:ff:ff:ff:ff
    inet 192.168.122.1/24 brd 192.168.122.255 scope global virbr0
       valid_lft forever preferred_lft forever
5: virbr0-nic: <BROADCAST,MULTICAST> mtu 1500 qdisc pfifo_fast master virbr0 state DOWN qlen 1000
    link/ether 52:54:00:ed:da:3f brd ff:ff:ff:ff:ff:ff
[ntc@ntc ~]$ 

```

> Note the virbr0 interface is an internal bridge and is enabled due to the libvirt package.


##### Step 4

In the previous labs, we configured a proxy server in order for the CentOS machine to have external reachablity. This works for connections that depend on the HTTP protocol. In order to enable direct (over a NAT) connectivity to the internet, bring up the interface `ens3`. Use the `ip link` command for this.


```
[ntc@ntc ~]$ sudo ip link set ens3 up
[sudo] password for ntc: 
[ntc@ntc ~]$ 

```


##### Step 5

Re-check the status of the link:

```
[ntc@ntc ~]$ ip link show ens3
2: ens3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT qlen 1000
    link/ether 2c:c2:60:3e:dd:80 brd ff:ff:ff:ff:ff:ff

```

Re-check the ip addresses of the interfaces:

```
[ntc@ntc ~]$ ip addr show ens3
2: ens3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether 2c:c2:60:3e:dd:80 brd ff:ff:ff:ff:ff:ff
    inet6 fe80::2ec2:60ff:fe3e:dd80/64 scope link 
       valid_lft forever preferred_lft forever
[ntc@ntc ~]$ 

```

> Even though the interface is up, an IP has not been assigned.



##### Step 6

The `ens3` interface has been configured to get an IP from a DHCP server. This requires the dhcp client to be invoked. Run the `dhclient` command.

```
[ntc@ntc ~]$ sudo dhclient
[sudo] password for ntc: 
[ntc@ntc ~]$ 

```

Re-check the IP address of ens3

```

[ntc@ntc ~]$ ip addr show ens3
2: ens3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether 2c:c2:60:3e:dd:80 brd ff:ff:ff:ff:ff:ff
    inet 10.0.0.4/16 brd 10.0.255.255 scope global dynamic ens3
       valid_lft 360000sec preferred_lft 360000sec
    inet6 fe80::2ec2:60ff:fe3e:dd80/64 scope link 
       valid_lft forever preferred_lft forever
[ntc@ntc ~]$ 

```


##### Step 7

In reality, interfaces should only be brought up/down using the `ifup and ifdown ` commands. Using these commands negate the extra step of having to invoke dhclient. In the lab, dhclient is shown as an extra step to understand the process to bring up the interface.

Shut down the ens3 interface using the `ifdown` command:

```
[ntc@ntc ~]$ sudo ifdown ens3
[ntc@ntc ~]$ 

```

```
[ntc@ntc ~]$ ip addr show ens3
2: ens3: <BROADCAST,MULTICAST> mtu 1500 qdisc pfifo_fast state DOWN qlen 1000
    link/ether 2c:c2:60:3e:dd:80 brd ff:ff:ff:ff:ff:ff
[ntc@ntc ~]$ 

```


##### Step 8

Use the `ifup` command to bring up the interface and automatically assign it an IP address:

```
[ntc@ntc ~]$ sudo ifup ens3

Determining IP information for ens3... done.
RTNETLINK answers: File exists

```

```
[ntc@ntc ~]$ ip addr show ens3
2: ens3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether 2c:c2:60:3e:dd:80 brd ff:ff:ff:ff:ff:ff
    inet 10.0.0.4/16 brd 10.0.255.255 scope global dynamic ens3
       valid_lft 359995sec preferred_lft 359995sec
    inet6 fe80::2ec2:60ff:fe3e:dd80/64 scope link 
       valid_lft forever preferred_lft forever
[ntc@ntc ~]$ 

```


##### Step 9

This command also inserts a new default route on the device (as defined in the DHCP server). Check the routing table:


```
[ntc@ntc ~]$ ip route
default via 10.0.0.2 dev ens3 
10.0.0.0/16 dev ens3 proto kernel scope link src 10.0.0.4 
192.168.0.0/24 dev ens4 proto kernel scope link src 192.168.0.52 
192.168.122.0/24 dev virbr0 proto kernel scope link src 192.168.122.1 

```




##### Step 10

Now retry our original test to ping `yahoo.com`.

```
[ntc@ntc ~]$ ping yahoo.com
PING yahoo.com (98.138.252.38) 56(84) bytes of data.
64 bytes from media-router-fp1.prod.media.vip.ne1.yahoo.com (98.138.252.38): icmp_seq=1 ttl=54 time=33.2 ms
64 bytes from media-router-fp1.prod.media.vip.ne1.yahoo.com (98.138.252.38): icmp_seq=2 ttl=54 time=32.8 ms
64 bytes from media-router-fp1.prod.media.vip.ne1.yahoo.com (98.138.252.38): icmp_seq=3 ttl=54 time=32.5 ms
64 bytes from media-router-fp1.prod.media.vip.ne1.yahoo.com (98.138.252.38): icmp_seq=4 ttl=54 time=32.6 ms
64 bytes from media-router-fp1.prod.media.vip.ne1.yahoo.com (98.138.252.38): icmp_seq=5 ttl=54 time=32.8 ms
64 bytes from media-router-fp1.prod.media.vip.ne1.yahoo.com (98.138.252.38): icmp_seq=6 ttl=54 time=32.8 ms
^C
--- yahoo.com ping statistics ---
6 packets transmitted, 6 received, 0% packet loss, time 5008ms
rtt min/avg/max/mdev = 32.591/32.848/33.248/0.321 ms
[ntc@ntc ~]$ 

```




### Task 2 - Network configuration files

##### Step 1 


The network configurations are controlled by the files in `/etc/sysconfig/network-scripts`. Look at the contents of `/etc/sysconfig/network-scripts/ifcfg-ens3` using the `more` command.

```
[ntc@ntc ~]$ more /etc/sysconfig/network-scripts/ifcfg-ens3 
DEVICE=ens3
ONBOOT=no
NM_CONTROLLED=no
BOOTPROTO=dhcp
[ntc@ntc ~]$ 

```
This file specifies that the interface should not be enabled on boot per the line `ONBOOT=no`.


##### Step 2

The `systemctl` command is used to stop and start services on the device. Use the `systemctl` command to restart networking. This will reinforce the settings as defined in the network-scripts directory.


```
[ntc@ntc ~]$ sudo systemctl restart network
[sudo] password for ntc: 
[ntc@ntc ~]$ 

```

Check the status of the interface.
```

[ntc@ntc ~]$ ip link show ens3
2: ens3: <BROADCAST,MULTICAST> mtu 1500 qdisc pfifo_fast state DOWN mode DEFAULT qlen 1000
    link/ether 2c:c2:60:3e:dd:80 brd ff:ff:ff:ff:ff:ff
[ntc@ntc ~]$ 
[ntc@ntc ~]$ 

```
Check the ip address of the interface.

```

[ntc@ntc ~]$ ip addr show ens3
2: ens3: <BROADCAST,MULTICAST> mtu 1500 qdisc pfifo_fast state DOWN qlen 1000
    link/ether 2c:c2:60:3e:dd:80 brd ff:ff:ff:ff:ff:ff
[ntc@ntc ~]$ 
[ntc@ntc ~]$ 

```


##### Step 3


Modify the `/etc/sysconfig/network-scripts/ifcfg-ens3` to always come up on boot.


```
DEVICE=ens3
ONBOOT=yes
NM_CONTROLLED=no
BOOTPROTO=dhcp

```



##### Step 4

Now restart the network services:

```
[ntc@ntc ~]$ sudo systemctl restart network
[ntc@ntc ~]$ 

```



##### Step 5

Re-check the link status and IP address

```
[ntc@ntc ~]$ ip link show ens3
2: ens3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT qlen 1000
    link/ether 2c:c2:60:3e:dd:80 brd ff:ff:ff:ff:ff:ff
[ntc@ntc ~]$ 

```


```
[ntc@ntc ~]$ ip addr show ens3
2: ens3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether 2c:c2:60:3e:dd:80 brd ff:ff:ff:ff:ff:ff
    inet 10.0.0.4/16 brd 10.0.255.255 scope global dynamic ens3
       valid_lft 359929sec preferred_lft 359929sec
    inet6 fe80::2ec2:60ff:fe3e:dd80/64 scope link 
       valid_lft forever preferred_lft forever
[ntc@ntc ~]$ 

```

### Task 3 - Network name resolution

By default, Linux servers first check the `/etc/hosts` file for name resolution of internet names. Failing this, it then queries the nameserver as defined in `/etc/resolv.conf`.

##### Step 1 

View the contents of the `/etc/hosts` file.

```
[ntc@ntc ~]$ cat /etc/hosts
129.213.99.91 vasa1

10.0.0.11   veos1
10.0.0.12   veos2

10.0.0.37   vmx1
10.0.0.38   vmx2


127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

```

> This is a simple text file formatted with <IP>  <HOSTNAME>. 


    
##### Step 2
Add a new entry in this file for the google dns server (8.8.8.8). Call it `google.dns`


> Use vim invoked with `sudo` to edit the `/etc/hosts` file.

```
[ntc@ntc ~]$ cat /etc/hosts
8.8.8.8 google.dns

129.213.99.91 vasa1

10.0.0.11   veos1
10.0.0.12   veos2

10.0.0.37   vmx1
10.0.0.38   vmx2


127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6



```


##### Step 3

Now try to ping `google.dns`.

```
[ntc@ntc ~]$ ping google.dns
PING google.dns (8.8.8.8) 56(84) bytes of data.
64 bytes from google.dns (8.8.8.8): icmp_seq=1 ttl=60 time=2.86 ms
64 bytes from google.dns (8.8.8.8): icmp_seq=2 ttl=60 time=2.32 ms
64 bytes from google.dns (8.8.8.8): icmp_seq=3 ttl=60 time=2.33 ms
^C
--- google.dns ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2004ms
rtt min/avg/max/mdev = 2.325/2.508/2.863/0.254 ms
[ntc@ntc ~]$ 

```


##### Step 4

The `/etc/resolv.conf` is used to identify the nameserver closest to the host for DNS resolution. This is typically automatically populated via dhcp. Print the contents of this file onto the screen:

```
[ntc@ntc ~]$ cat /etc/resolv.conf 
; generated by /usr/sbin/dhclient-script
search localdomain
nameserver 10.0.0.1
[ntc@ntc ~]$ 


```


##### Step 5

Remove the line that starts with `nameserver` and then try to ping google.com


```
[ntc@ntc ~]$ cat /etc/resolv.conf 
; generated by /usr/sbin/dhclient-script
search localdomain
[ntc@ntc ~]$ 

```

```

[ntc@ntc ~]$ ping google.com
ping: google.com: Name or service not known
[ntc@ntc ~]$ 

```


##### Step 6

Add the public google nameserver into `/etc/resolv.conf`.

```
[ntc@ntc ~]$ cat /etc/resolv.conf 
; generated by /usr/sbin/dhclient-script
search localdomain
nameserver 8.8.8.8
[ntc@ntc ~]$ 

```

```

[ntc@ntc ~]$ ping google.com
PING google.com (172.217.15.78) 56(84) bytes of data.
64 bytes from iad23s63-in-f14.1e100.net (172.217.15.78): icmp_seq=1 ttl=57 time=2.29 ms
64 bytes from iad23s63-in-f14.1e100.net (172.217.15.78): icmp_seq=2 ttl=57 time=2.34 ms
64 bytes from iad23s63-in-f14.1e100.net (172.217.15.78): icmp_seq=3 ttl=57 time=2.22 ms

```




### Task 4 - DNS Tools

The 2 tools used to troubleshoot DNS are `nslookup` and `dig`. In this task we will understand how to use these tools

##### Step 1 

Use the `nslookup` command to find the IP address of `yahoo.com`.

```
[ntc@ntc ~]$ nslookup yahoo.com
Server:		8.8.8.8
Address:	8.8.8.8#53

Non-authoritative answer:
Name:	yahoo.com
Address: 98.138.252.38
Name:	yahoo.com
Address: 206.190.39.42
Name:	yahoo.com
Address: 98.139.180.180

[ntc@ntc ~]$ 

```


##### Step 2

Use nslookup interactively to point the DNS queries to different DNS servers and re-execute the name lookup:

```
[ntc@ntc ~]$ nslookup
> server
Default server: 8.8.8.8
Address: 8.8.8.8#53

```

```
> yahoo.com
Server:		8.8.8.8
Address:	8.8.8.8#53

Non-authoritative answer:
Name:	yahoo.com
Address: 98.138.252.38
Name:	yahoo.com
Address: 206.190.39.42
Name:	yahoo.com
Address: 98.139.180.180
> 

```

```

> server 10.0.0.1
Default server: 10.0.0.1
Address: 10.0.0.1#53
> yahoo.com
Server:		10.0.0.1
Address:	10.0.0.1#53

Non-authoritative answer:
Name:	yahoo.com
Address: 98.138.252.38
Name:	yahoo.com
Address: 206.190.39.42
Name:	yahoo.com
Address: 98.139.180.180
> 

```

##### Step 3

Use `dig` to lookup the `A Record` or address record for `juniper.com`.

```
[ntc@ntc ~]$ dig juniper.com

; <<>> DiG 9.9.4-RedHat-9.9.4-51.el7_4.2 <<>> juniper.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 4121
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;juniper.com.			IN	A

;; ANSWER SECTION:
juniper.com.		42	IN	A	192.107.16.40

;; Query time: 12 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Fri Feb 09 11:16:09 EST 2018
;; MSG SIZE  rcvd: 56

[ntc@ntc ~]$ 

```

> Pay attention to the output particularly to the following fields: `status`, `ANSWER SECTION`, `Query time` and `SERVER`.



##### Step 4

Use `dig` to lookup a non-existing internet name. Use dig to query the address of `juuuniper.com`


```
[ntc@ntc ~]$ dig juuuniper.com

; <<>> DiG 9.9.4-RedHat-9.9.4-51.el7_4.2 <<>> juuuniper.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 629
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;juuuniper.com.			IN	A

;; AUTHORITY SECTION:
com.			899	IN	SOA	a.gtld-servers.net. nstld.verisign-grs.com. 1518193089 1800 900 604800 86400

;; Query time: 30 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Fri Feb 09 11:18:30 EST 2018
;; MSG SIZE  rcvd: 115

[ntc@ntc ~]$ 

```

> Compare the output from the previous step, paying attention to the same fields from before. What is the value now for `status`. 




##### Step 5

Force the dns query to a particular DNS server. Use the dig command to query `10.0.0.1` for `juniper.com`

```
[ntc@ntc ~]$ dig @10.0.0.1 juniper.com

; <<>> DiG 9.9.4-RedHat-9.9.4-51.el7_4.2 <<>> @10.0.0.1 juniper.com
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 56496
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;juniper.com.			IN	A

;; ANSWER SECTION:
juniper.com.		600	IN	A	192.107.16.40

;; Query time: 43 msec
;; SERVER: 10.0.0.1#53(10.0.0.1)
;; WHEN: Fri Feb 09 11:20:54 EST 2018
;; MSG SIZE  rcvd: 56

[ntc@ntc ~]$ 


```

> Compare the query time between the first dig command and this one.



##### Step 6

Use dig to check the path of recursive DNS queries. Use the `+trace` option with dig to trace the dns query for `eos.arista.com`

```
[ntc@ntc ~]$ dig @8.8.8.8 eos.arista.com +trace

; <<>> DiG 9.9.4-RedHat-9.9.4-51.el7_4.2 <<>> @8.8.8.8 eos.arista.com +trace
; (1 server found)
;; global options: +cmd
.			214773	IN	NS	a.root-servers.net.
.			214773	IN	NS	b.root-servers.net.
.			214773	IN	NS	c.root-servers.net.
.			214773	IN	NS	d.root-servers.net.
.			214773	IN	NS	e.root-servers.net.
.			214773	IN	NS	f.root-servers.net.
.			214773	IN	NS	g.root-servers.net.
.			214773	IN	NS	h.root-servers.net.
.			214773	IN	NS	i.root-servers.net.
.			214773	IN	NS	j.root-servers.net.
.			214773	IN	NS	k.root-servers.net.
.			214773	IN	NS	l.root-servers.net.
.			214773	IN	NS	m.root-servers.net.
.			214773	IN	RRSIG	NS 8 0 518400 20180220170000 20180207160000 41824 . BGeBbOoh4t1ntVG6P4EhNbowYIlpoMqYfPQ7zLoj2xdImQ8erNYr/zK9 U48/iotHVEg4wM9p1I1CroUKsZHt1VLPixcXw7b4pTT6jCaLoqyFv2wq 8xeSQLZZ0eVrsnqPDlfxQ5/S+/UmjTWQQNsnWahP+ahwtmKiwOlkWHrs f1Ak+Un+4jEsO9BA8/bIgCOobTb523J6cu+rh7uHFF8gyqeALC1QtplB XVz5B9qmBRsuHSfVBxvjjcrDw+o2JoTJJjbwukD6XZxgI/8VO3mzvZoc Wlrzr1Rbz+RWh5z9rzRPjdIMsKGI/VILq5AK/Mn9ChluvlxJx/LqyNPk F2bLsg==
;; Received 525 bytes from 8.8.8.8#53(8.8.8.8) in 120 ms

com.			172800	IN	NS	a.gtld-servers.net.
com.			172800	IN	NS	b.gtld-servers.net.
com.			172800	IN	NS	c.gtld-servers.net.
com.			172800	IN	NS	d.gtld-servers.net.
com.			172800	IN	NS	e.gtld-servers.net.
com.			172800	IN	NS	f.gtld-servers.net.
com.			172800	IN	NS	g.gtld-servers.net.
com.			172800	IN	NS	h.gtld-servers.net.
com.			172800	IN	NS	i.gtld-servers.net.
com.			172800	IN	NS	j.gtld-servers.net.
com.			172800	IN	NS	k.gtld-servers.net.
com.			172800	IN	NS	l.gtld-servers.net.
com.			172800	IN	NS	m.gtld-servers.net.
com.			86400	IN	DS	30909 8 2 E2D3C916F6DEEAC73294E8268FB5885044A833FC5459588F4A9184CF C41A5766
com.			86400	IN	RRSIG	DS 8 1 86400 20180222050000 20180209040000 41824 . mRJy+0OrIFRxFthU628BYM5y8t0fEsgflaa7Kk8sE+cZQUteVutja+uY +xsWGCpRVkodHpcFU9aDGB9W0BAZq3LdrzDtDGA3HMAeDEFmknf3LhQB OPTLSJ47w4wppwhbxueWf34DDKbZY8D/DKm46KzL67k6MA1SlKSDm0fF L1hTnRNNACrCCREZVdOPzpskfzI/xIBm9Ck5nLszF3hPf+/r/3kSXVoo Hd/yte78a1SelcoC+DTIjJLChhVaKslvSTRvB6Y6NuteadQxOG3PAOCI HgUaFrqgoZnz5FoBhMKmL9YKKEB/FMBRLR1Ps4e+JJrfVHBBeREZxNWM IsT5ew==
;; Received 1174 bytes from 198.97.190.53#53(h.root-servers.net) in 97 ms

arista.com.		172800	IN	NS	dns3.easydns.org.
arista.com.		172800	IN	NS	dns1.easydns.com.
arista.com.		172800	IN	NS	dns2.easydns.net.
arista.com.		172800	IN	NS	dns4.easydns.info.
CK0POJMG874LJREF7EFN8430QVIT8BSM.com. 86400 IN NSEC3 1 1 0 - CK0Q1GIN43N1ARRC9OSM6QPQR81H5M9A NS SOA RRSIG DNSKEY NSEC3PARAM
CK0POJMG874LJREF7EFN8430QVIT8BSM.com. 86400 IN RRSIG NSEC3 8 2 86400 20180213054812 20180206043812 46967 com. oI+Za4j7wVjn4fZpxeFKERuQbdVTo5d+5oF4ZH6JCRPV6z0dqg3LK7Tc AkiuXsE0gOKqf8Zxzv1201DZwewSn0ySescZ4CunqhG03Ozith7EbNu9 sa8ErJt9C+dyIwhiGUGQkArIQOF3zPUt+JgVh3gwtR+xICga/0UHtYl1 /ec=
04IRGPMTN8SCHQRC60GPNEC2L7GA0CQ1.com. 86400 IN NSEC3 1 1 0 - 04ISMH844HBMD169K676HPE26LE83P2I NS DS RRSIG
04IRGPMTN8SCHQRC60GPNEC2L7GA0CQ1.com. 86400 IN RRSIG NSEC3 8 2 86400 20180214060426 20180207045426 46967 com. j1z9h5VQLRl7NB+9xWF7vW7gM2klFOmFeCzKQvgw6mZ5z71b3hMeEHqQ j79ZIclA9CscVmc9p5gPI6XPNqoVRFDL94Gymvsp2LqgaNK/jY3aXhC5 AJTbuWgOlcG7C/lKuUhB251KYtxBhWmbw2+XDWhYvjyGbBGfDmP7fwCW UHA=
;; Received 734 bytes from 192.31.80.30#53(d.gtld-servers.net) in 103 ms

eos.arista.com.		1200	IN	CNAME	ipv6.arista.com.edgekey.net.
;; Received 84 bytes from 198.41.222.254#53(dns2.easydns.net) in 31 ms

[ntc@ntc ~]$ 

```


##### Step 7

Use the `+short` option with dig to query `eos.arista.com` and `yahoo.com`


```
[ntc@ntc ~]$ dig @8.8.8.8 eos.arista.com  +short
ipv6.arista.com.edgekey.net.
e10346.dscb.akamaiedge.net.
96.6.55.176
[ntc@ntc ~]$ 

```

```
[ntc@ntc ~]$ dig @8.8.8.8 yahoo.com  +short
98.138.252.38
98.139.180.180
206.190.39.42
[ntc@ntc ~]$ 

```


### Task 5 - Daemons

In this lab we will look at the [ntp daemon](http://doc.ntp.org/4.1.0/ntpd.htm), or `ntpd`.  This daemon sets & maintains system time on our Linux systems.  It sync's the time of our local system with an authorized ntp server.  

Before we get further into NTPD, let's examine daemons.

##### Step 1 - Daemons

A "daemon" in Linux is an application that runs unobtrusively in the background.  It is not under the direct control of a user.  Most often, these are reserved for system-level & long-running applications.

If you recall our 'Foreground vs Background' application lab, when we added an `&` to send our script to the background, we essentially created a daemon.  Most daemons read configuration files at startup, and then are only manipulated editing this file and resarting the application/process.

Daemons are _normally_ started at a certain point in the bootup process, and are triggered by certain `init` applications (`systemd`, `System V`, `init.d`).

More information can be found [here](http://www.linfo.org/daemon.html)

A list of common Unix daemons can be found [here](https://en.wikipedia.org/wiki/List_of_Unix_daemons)

##### Step 2 - NTPD Process

First, let's determine if NTPD is currently running:

```
[root@ntc ~]# systemctl status ntpd
● ntpd.service - Network Time Service
   Loaded: loaded (/usr/lib/systemd/system/ntpd.service; disabled; vendor preset: disabled)
   Active: inactive (dead)
```

Our server reports that ntpd is dead.  Let's use the systemd daemon to start our ntpd daemon.

```
[root@ntc ~]# systemctl start ntpd
```

Let's check the status:

```
[root@ntc ~]# systemctl status ntpd
● ntpd.service - Network Time Service
   Loaded: loaded (/usr/lib/systemd/system/ntpd.service; disabled; vendor preset: disabled)
   Active: active (running) since Fri 2018-02-09 15:09:06 EST; 4s ago
  Process: 6438 ExecStart=/usr/sbin/ntpd -u ntp:ntp $OPTIONS (code=exited, status=0/SUCCESS)
 Main PID: 6440 (ntpd)
   CGroup: /system.slice/ntpd.service
           └─6440 /usr/sbin/ntpd -u ntp:ntp -g

Feb 09 15:09:06 ntc ntpd[6440]: Listen and drop on 0 v4wildcard 0.0.0.0 UDP 123
Feb 09 15:09:06 ntc ntpd[6440]: Listen and drop on 1 v6wildcard :: UDP 123
Feb 09 15:09:06 ntc ntpd[6440]: Listen normally on 2 lo 127.0.0.1 UDP 123
Feb 09 15:09:06 ntc ntpd[6440]: Listen normally on 3 ens3 10.0.0.3 UDP 123
Feb 09 15:09:06 ntc ntpd[6440]: Listen normally on 4 ens4 192.168.0.52 UDP 123
Feb 09 15:09:06 ntc ntpd[6440]: Listen normally on 5 virbr0 192.168.122.1 UDP 123
Feb 09 15:09:06 ntc ntpd[6440]: Listen normally on 6 lo ::1 UDP 123
Feb 09 15:09:06 ntc ntpd[6440]: Listen normally on 7 ens3 fe80::2ec2:60ff:fe3e:dd80 UDP 123
Feb 09 15:09:06 ntc ntpd[6440]: Listen normally on 8 ens4 fe80::7070:148a:e15b:b2fb UDP 123
Feb 09 15:09:06 ntc ntpd[6440]: Listening on routing socket on fd #25 for interface updates
[root@ntc ~]# 
```

Now, let's stop the daemon & check the status again:

```
[root@ntc ~]# systemctl stop ntpd
[root@ntc ~]# systemctl status ntpd
● ntpd.service - Network Time Service
   Loaded: loaded (/usr/lib/systemd/system/ntpd.service; disabled; vendor preset: disabled)
   Active: inactive (dead) since Fri 2018-02-09 15:11:15 EST; 3s ago
  Process: 6438 ExecStart=/usr/sbin/ntpd -u ntp:ntp $OPTIONS (code=exited, status=0/SUCCESS)
 Main PID: 6440 (code=exited, status=0/SUCCESS)

Feb 09 15:09:06 ntc ntpd[6440]: Listen normally on 7 ens3 fe80::2ec2:60ff:fe3e:dd80 UDP 123
Feb 09 15:09:06 ntc ntpd[6440]: Listen normally on 8 ens4 fe80::7070:148a:e15b:b2fb UDP 123
Feb 09 15:09:06 ntc ntpd[6440]: Listening on routing socket on fd #25 for interface updates
Feb 09 15:09:11 ntc ntpd[6440]: 0.0.0.0 c016 06 restart
Feb 09 15:09:11 ntc ntpd[6440]: 0.0.0.0 c012 02 freq_set kernel 0.000 PPM
Feb 09 15:09:11 ntc ntpd[6440]: 0.0.0.0 c011 01 freq_not_set
Feb 09 15:09:17 ntc ntpd[6440]: 0.0.0.0 c614 04 freq_mode
Feb 09 15:11:15 ntc ntpd[6440]: ntpd exiting on signal 15
Feb 09 15:11:15 ntc systemd[1]: Stopping Network Time Service...
Feb 09 15:11:15 ntc systemd[1]: Stopped Network Time Service.
```

##### Step 3 - NTPD Status

Daemons can come with their own commands and flags, just like other processes/scripts.  Lets start ntpd and look at some of the options:

```
[root@ntc ~]# systemctl start ntpd
```

ntpd comes with a few built-in commands  to configure, verify, troubleshoot ntp connectivity. These are:

- ntpd
- ntpupdate
- ntpdc
- ntp-keygen
- ntpq
- ntpstat
- ntptime

Instead of checking the status of the daemon, using `systemctl status ntpd`, we can query the daemon itself using `ntpq`. `ntpq` has many options:

```
[root@ntc ~]# ntpq --help
ntpq - standard NTP query program - Ver. 4.2.6p5
USAGE:  ntpq [ -<flag> [<val>] | --<name>[{=| }<val>] ]... [ host ...]
  Flg Arg Option-Name    Description
   -4 no  ipv4           Force IPv4 DNS name resolution
				- prohibits the option 'ipv6'
   -6 no  ipv6           Force IPv6 DNS name resolution
				- prohibits the option 'ipv4'
   -c Str command        run a command and exit
				- may appear multiple times
   -d no  debug-level    Increase output debug message level
				- may appear multiple times
   -D Str set-debug-level Set the output debug message level
				- may appear multiple times
   -p no  peers          Print a list of the peers
				- prohibits the option 'interactive'
   -i no  interactive    Force ntpq to operate in interactive mode
				- prohibits these options:
				command
				peers
   -n no  numeric        numeric host addresses
      no  old-rv         Always output status line with readvar
      opt version        Output version information and exit
   -? no  help           Display extended usage information and exit
   -! no  more-help      Extended usage information passed thru pager
   -> opt save-opts      Save the option state to a config file
   -< Str load-opts      Load options from a config file
				- disabled as '--no-load-opts'
				- may appear multiple times
```

Most often we will use the `-p` flag to check the status of configured peers:

```
[root@ntc ~]# ntpq -p
     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
+ns1.infomir.com 243.50.127.182   2 u   14   64   77  125.862    0.662   3.259
-node54.ia64.org 144.76.96.7      3 u   18   64   77    6.497  -16.112   2.885
*owners.kjsl.com 216.218.254.202  2 u   13   64   77   63.591   -6.754   3.241
+linode.appus.or 216.218.254.202  2 u   17   64   77   65.555   -3.632   2.344
```

##### Step 4 - NTPD Configuration

To see how the peers above were setup, lets look at our configuration file for `ntpd`:

```
[root@ntc ~]# cat /etc/ntp.conf 
# For more information about this file, see the man pages
# ntp.conf(5), ntp_acc(5), ntp_auth(5), ntp_clock(5), ntp_misc(5), ntp_mon(5).

driftfile /var/lib/ntp/drift

# Permit time synchronization with our time source, but do not
# permit the source to query or modify the service on this system.
restrict default nomodify notrap nopeer noquery

# Permit all access over the loopback interface.  This could
# be tightened as well, but to do so would effect some of
# the administrative functions.
restrict 127.0.0.1 
restrict ::1

# Hosts on local network are less restricted.
#restrict 192.168.1.0 mask 255.255.255.0 nomodify notrap

# Use public servers from the pool.ntp.org project.
# Please consider joining the pool (http://www.pool.ntp.org/join.html).
server 0.centos.pool.ntp.org iburst
server 1.centos.pool.ntp.org iburst
server 2.centos.pool.ntp.org iburst
server 3.centos.pool.ntp.org iburst

#broadcast 192.168.1.255 autokey	# broadcast server
#broadcastclient			# broadcast client
#broadcast 224.0.1.1 autokey		# multicast server
#multicastclient 224.0.1.1		# multicast client
#manycastserver 239.255.254.254		# manycast server
#manycastclient 239.255.254.254 autokey # manycast client

# Enable public key cryptography.
#crypto

includefile /etc/ntp/crypto/pw

# Key file containing the keys and key identifiers used when operating
# with symmetric key cryptography. 
keys /etc/ntp/keys

# Specify the key identifiers which are trusted.
#trustedkey 4 8 42

# Specify the key identifier to use with the ntpdc utility.
#requestkey 8

# Specify the key identifier to use with the ntpq utility.
#controlkey 8

# Enable writing of statistics records.
#statistics clockstats cryptostats loopstats peerstats

# Disable the monitoring facility to prevent amplification attacks using ntpdc
# monlist command when default restrict does not include the noquery flag. See
# CVE-2013-5211 for more details.
# Note: Monitoring will not be disabled with the limited restriction flag.
disable monitor
```

The interesting (for our purposes) part of the file is in the middle, here:

```
server 0.centos.pool.ntp.org iburst
server 1.centos.pool.ntp.org iburst
server 2.centos.pool.ntp.org iburst
server 3.centos.pool.ntp.org iburst
```

These lines configure our ntp servers to use `[0..3].centos.pool.ntp.org` as the authorized servers for system time.

Let's make a change, and check the output of our peers.  Edit the lines above to remove the first and last entries, leaving only:

```
server 1.centos.pool.ntp.org iburst
server 2.centos.pool.ntp.org iburst
```

Then restart the daemon by running `systemctl restart ntpd`

Let's query our ntp peers again:

```
[root@ntc ~]# ntpq -p
     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
*tools.ninjaneer 127.67.113.92    2 u   29   64    1   63.767   -6.872   0.175
 resolver1.skyfi 216.218.192.202  2 u   29   64    1   70.836   -4.847   1.444
```

As we can see, editing the configuration file, and restarting the daemon caused two of the servers to disappear


