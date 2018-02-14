## Net Tools Lab (Continued)

In this lab we will build upon our earlier `Net Tools` lab.  We will need to ensure that our web server is still running & we have `netcat` and `nmap` installed

### Task 1 - Netcat

##### Step 1 - Netcat Basic

[Netcat](http://netcat.sourceforge.net/) can be used to read network data on a local server.  It can also be used to verify/validate connectivity to local & exernal network resources.

Like `tcpdump` `netcat` has *MANY* options:

```bash
[ntc@ntc web]$ nc -h
Ncat 6.40 ( http://nmap.org/ncat )
Usage: ncat [options] [hostname] [port]

Options taking a time assume seconds. Append 'ms' for milliseconds,
's' for seconds, 'm' for minutes, or 'h' for hours (e.g. 500ms).
  -4                         Use IPv4 only
  -6                         Use IPv6 only
  -U, --unixsock             Use Unix domain sockets only
  -C, --crlf                 Use CRLF for EOL sequence
  -c, --sh-exec <command>    Executes the given command via /bin/sh
  -e, --exec <command>       Executes the given command
      --lua-exec <filename>  Executes the given Lua script
  -g hop1[,hop2,...]         Loose source routing hop points (8 max)
  -G <n>                     Loose source routing hop pointer (4, 8, 12, ...)
  -m, --max-conns <n>        Maximum <n> simultaneous connections
  -h, --help                 Display this help screen
  -d, --delay <time>         Wait between read/writes
  -o, --output <filename>    Dump session data to a file
  -x, --hex-dump <filename>  Dump session data as hex to a file
  -i, --idle-timeout <time>  Idle read/write timeout
  -p, --source-port port     Specify source port to use
  -s, --source addr          Specify source address to use (doesn't affect -l)
  -l, --listen               Bind and listen for incoming connections
  -k, --keep-open            Accept multiple connections in listen mode
  -n, --nodns                Do not resolve hostnames via DNS
  -t, --telnet               Answer Telnet negotiations
  -u, --udp                  Use UDP instead of default TCP
      --sctp                 Use SCTP instead of default TCP
  -v, --verbose              Set verbosity level (can be used several times)
  -w, --wait <time>          Connect timeout
      --append-output        Append rather than clobber specified output files
      --send-only            Only send data, ignoring received; quit on EOF
      --recv-only            Only receive data, never send anything
      --allow                Allow only given hosts to connect to Ncat
      --allowfile            A file of hosts allowed to connect to Ncat
      --deny                 Deny given hosts from connecting to Ncat
      --denyfile             A file of hosts denied from connecting to Ncat
      --broker               Enable Ncat's connection brokering mode
      --chat                 Start a simple Ncat chat server
      --proxy <addr[:port]>  Specify address of host to proxy through
      --proxy-type <type>    Specify proxy type ("http" or "socks4")
      --proxy-auth <auth>    Authenticate with HTTP or SOCKS proxy server
      --ssl                  Connect or listen with SSL
      --ssl-cert             Specify SSL certificate file (PEM) for listening
      --ssl-key              Specify SSL private key (PEM) for listening
      --ssl-verify           Verify trust and domain name of certificates
      --ssl-trustfile        PEM file containing trusted SSL certificates
      --version              Display Ncat's version information and exit
```

For basic listening, we can use the `-l` flag.  However, if we try to listen on our server's port, we will throw an error:

```bash
[ntc@ntc web]$ nc -l -p 5000
Ncat: bind to 0.0.0.0:5000: Address already in use. QUITTING.
```

This is because netcat works a bit differently than tcpdump.  It does not listen passively.  The details are beyond the scope of this lab, but netcat is _normally_ used to test connectivity.  It can also act as a server, listening on a certain port.

##### Step 2 - Netcat connection

Let's imagine that we want to test that port 5000 is open on our host.  From our host we could run the following netcat command:

```bash
[ntc@ntc ~]$ nc -v 127.0.0.1 5000
Ncat: Version 7.60 ( https://nmap.org/ncat )
Ncat: Connected to 127.0.0.1:5000.
```

> You can stop netcat with <CTRL>+c

The `-v` flag is for verbose output.  The Netcat syntax is: `nc [options] host port`

We could also run the above command from a remote host to ensure connectivity

Netcat can also be used to _scan_ ports, looking for an open port.  Let's assume we don't know what port our web server is configured to use.  We can check by adding a `-z` flag:

```bash
nc -z -v 127.0.0.1 1-65535 #this will scan all ports
```

We can also use netcat to check external connectivity:

```bash
nc -v google.com 80
Ncat: Version 7.60 ( https://nmap.org/ncat )
Ncat: Connected to 172.217.1.46:80.
```

##### Step 3 - Netcat Listener

Let's revisit the netcat listener option.  It can be very useful in troubleshooting.  

Let's assume we are having a problem with our webserver, and we want to ensure that it's not a network issue.  All signs indicate that ports 5000-6000 should be open & available.  We can use netcat to serve our `index.html` file on a different port.  Let's use port 5001.

Ensure we are in the `~/web` directory we created earlier, and the `index.html` file is available.  Then enter the following command:

```bash
[ntc@ntc web]$ nc -l 5001 < index.html 
```

Opening a browser and navigating to: `host:5001`, we should be greeted with the following:

![](images/netcatserver.png)

> We suggest using Firefox, as Chrome _may_ have problems with this test

You'll also notice the following in your terminal:

```bash
[ntc@ntc web]$ nc -l 5000 < index.html 
GET / HTTP/1.1
Host: localhost:5000
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:58.0) Gecko/20100101 Firefox/58.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: keep-alive
Upgrade-Insecure-Requests: 1
```

Unlike a _true_ web server, netcat serves the file only once.  If you refresh the browser, you'll notice that the web page is unavailable. 

> CHALLENGE: Configure a `netcat` listener on your jumphost, to use port 9125, and transfer the `index.html` file via `netcat` on a different host

> CHALLENGE 2: Use `netcat` to query the `ntp` servers configured on your host

### Task 2 - NMAP

[Nmap](https://nmap.org/) is a utility for network discovery & security auditing.  It can be used to disover open ports, host & OS information, vulnerability detection, and much more.

In this lab we will use Nmap to discover ports, hosts, and details about our network


##### Step 1 - Nmap Options

Take a moment to get familiar with the options for nmap:

```bash
[ntc@ntc ~]$ nmap -h
Nmap 6.40 ( http://nmap.org )
Usage: nmap [Scan Type(s)] [Options] {target specification}
TARGET SPECIFICATION:
  Can pass hostnames, IP addresses, networks, etc.
  Ex: scanme.nmap.org, microsoft.com/24, 192.168.0.1; 10.0.0-255.1-254
  -iL <inputfilename>: Input from list of hosts/networks
  -iR <num hosts>: Choose random targets
  --exclude <host1[,host2][,host3],...>: Exclude hosts/networks
  --excludefile <exclude_file>: Exclude list from file
HOST DISCOVERY:
  -sL: List Scan - simply list targets to scan
  -sn: Ping Scan - disable port scan
  -Pn: Treat all hosts as online -- skip host discovery
  -PS/PA/PU/PY[portlist]: TCP SYN/ACK, UDP or SCTP discovery to given ports
  -PE/PP/PM: ICMP echo, timestamp, and netmask request discovery probes
  -PO[protocol list]: IP Protocol Ping
  -n/-R: Never do DNS resolution/Always resolve [default: sometimes]
  --dns-servers <serv1[,serv2],...>: Specify custom DNS servers
  --system-dns: Use OS's DNS resolver
  --traceroute: Trace hop path to each host
SCAN TECHNIQUES:
  -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans
  -sU: UDP Scan
  -sN/sF/sX: TCP Null, FIN, and Xmas scans
  --scanflags <flags>: Customize TCP scan flags
  -sI <zombie host[:probeport]>: Idle scan
  -sY/sZ: SCTP INIT/COOKIE-ECHO scans
  -sO: IP protocol scan
  -b <FTP relay host>: FTP bounce scan
PORT SPECIFICATION AND SCAN ORDER:
  -p <port ranges>: Only scan specified ports
    Ex: -p22; -p1-65535; -p U:53,111,137,T:21-25,80,139,8080,S:9
  -F: Fast mode - Scan fewer ports than the default scan
  -r: Scan ports consecutively - don't randomize
  --top-ports <number>: Scan <number> most common ports
  --port-ratio <ratio>: Scan ports more common than <ratio>
SERVICE/VERSION DETECTION:
  -sV: Probe open ports to determine service/version info
  --version-intensity <level>: Set from 0 (light) to 9 (try all probes)
  --version-light: Limit to most likely probes (intensity 2)
  --version-all: Try every single probe (intensity 9)
  --version-trace: Show detailed version scan activity (for debugging)
SCRIPT SCAN:
  -sC: equivalent to --script=default
  --script=<Lua scripts>: <Lua scripts> is a comma separated list of
           directories, script-files or script-categories
  --script-args=<n1=v1,[n2=v2,...]>: provide arguments to scripts
  --script-args-file=filename: provide NSE script args in a file
  --script-trace: Show all data sent and received
  --script-updatedb: Update the script database.
  --script-help=<Lua scripts>: Show help about scripts.
           <Lua scripts> is a comma separted list of script-files or
           script-categories.
OS DETECTION:
  -O: Enable OS detection
  --osscan-limit: Limit OS detection to promising targets
  --osscan-guess: Guess OS more aggressively
TIMING AND PERFORMANCE:
  Options which take <time> are in seconds, or append 'ms' (milliseconds),
  's' (seconds), 'm' (minutes), or 'h' (hours) to the value (e.g. 30m).
  -T<0-5>: Set timing template (higher is faster)
  --min-hostgroup/max-hostgroup <size>: Parallel host scan group sizes
  --min-parallelism/max-parallelism <numprobes>: Probe parallelization
  --min-rtt-timeout/max-rtt-timeout/initial-rtt-timeout <time>: Specifies
      probe round trip time.
  --max-retries <tries>: Caps number of port scan probe retransmissions.
  --host-timeout <time>: Give up on target after this long
  --scan-delay/--max-scan-delay <time>: Adjust delay between probes
  --min-rate <number>: Send packets no slower than <number> per second
  --max-rate <number>: Send packets no faster than <number> per second
FIREWALL/IDS EVASION AND SPOOFING:
  -f; --mtu <val>: fragment packets (optionally w/given MTU)
  -D <decoy1,decoy2[,ME],...>: Cloak a scan with decoys
  -S <IP_Address>: Spoof source address
  -e <iface>: Use specified interface
  -g/--source-port <portnum>: Use given port number
  --data-length <num>: Append random data to sent packets
  --ip-options <options>: Send packets with specified ip options
  --ttl <val>: Set IP time-to-live field
  --spoof-mac <mac address/prefix/vendor name>: Spoof your MAC address
  --badsum: Send packets with a bogus TCP/UDP/SCTP checksum
OUTPUT:
  -oN/-oX/-oS/-oG <file>: Output scan in normal, XML, s|<rIpt kIddi3,
     and Grepable format, respectively, to the given filename.
  -oA <basename>: Output in the three major formats at once
  -v: Increase verbosity level (use -vv or more for greater effect)
  -d: Increase debugging level (use -dd or more for greater effect)
  --reason: Display the reason a port is in a particular state
  --open: Only show open (or possibly open) ports
  --packet-trace: Show all packets sent and received
  --iflist: Print host interfaces and routes (for debugging)
  --log-errors: Log errors/warnings to the normal-format output file
  --append-output: Append to rather than clobber specified output files
  --resume <filename>: Resume an aborted scan
  --stylesheet <path/URL>: XSL stylesheet to transform XML output to HTML
  --webxml: Reference stylesheet from Nmap.Org for more portable XML
  --no-stylesheet: Prevent associating of XSL stylesheet w/XML output
MISC:
  -6: Enable IPv6 scanning
  -A: Enable OS detection, version detection, script scanning, and traceroute
  --datadir <dirname>: Specify custom Nmap data file location
  --send-eth/--send-ip: Send using raw ethernet frames or IP packets
  --privileged: Assume that the user is fully privileged
  --unprivileged: Assume the user lacks raw socket privileges
  -V: Print version number
  -h: Print this help summary page.
EXAMPLES:
  nmap -v -A scanme.nmap.org
  nmap -v -sn 192.168.0.0/16 10.0.0.0/8
  nmap -v -iR 10000 -Pn -p 80
SEE THE MAN PAGE (http://nmap.org/book/man.html) FOR MORE OPTIONS AND EXAMPLES
```

With so many options, lets identify some of the most used command line flags:

* `-v`: Increase verbosity level (use -vv or more for greater effect)
* `-p <port ranges>`: Only scan specified ports (Example: `-p1-1024`)
* `-sn`: Ping Scan - disable port scan
* `-Pn`: Treat all hosts as online -- skip host discovery
* `-n/-R`: Never do DNS resolution/Always resolve [default: sometimes]
* `-O`: Enable OS detection 

> These flags can be combined

##### Step 2 - Ping Sweep

We can use nmap to issue a ping to all hosts in a given subnet, and return information about those that respond:

```bash
[ntc@ntc ~]$ nmap -sn 192.168.0.0/24

Starting Nmap 6.40 ( http://nmap.org ) at 2018-01-11 11:20 EST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for proxy.ntc.com (192.168.0.51)
Host is up (0.0039s latency).
Nmap scan report for 192.168.0.52
Host is up (0.0035s latency).
Nmap done: 256 IP addresses (2 hosts up) scanned in 21.92 seconds
```

As indicated by the last line, nmap scanned 256 IP addresses, and found one host that responded.

To increase the verbosity, add a `-v` option:

```bash
[ntc@ntc ~]$ nmap -v -sn 192.168.0.0/24

Starting Nmap 6.40 ( http://nmap.org ) at 2018-01-11 11:23 EST
Initiating Ping Scan at 11:23
Scanning 256 hosts [2 ports/host]
Ping Scan Timing: About 21.29% done; ETC: 11:26 (0:01:55 remaining)
Completed Ping Scan at 11:24, 57.43s elapsed (256 total hosts)
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.0.0 [host down]
Nmap scan report for 192.168.0.1 [host down]
Nmap scan report for 192.168.0.2 [host down]
Nmap scan report for 192.168.0.3 [host down]

[...snipped for brevity...]

Nmap scan report for 192.168.0.44 [host down]
Nmap scan report for 192.168.0.45 [host down]
Nmap scan report for 192.168.0.46 [host down]
Nmap scan report for 192.168.0.47 [host down]
Nmap scan report for 192.168.0.48 [host down]
Nmap scan report for 192.168.0.49 [host down]
Nmap scan report for 192.168.0.50 [host down]
Nmap scan report for proxy.ntc.com (192.168.0.51)
Host is up (0.0061s latency).
Nmap scan report for 192.168.0.52
Host is up (0.0051s latency).
Nmap scan report for 192.168.0.53 [host down]
Nmap scan report for 192.168.0.54 [host down]
Nmap scan report for 192.168.0.55 [host down]

[...snipped for brevity...]

Read data files from: /usr/bin/../share/nmap
Nmap done: 256 IP addresses (2 hosts up) scanned in 57.46 seconds
```

With verbosity increased, we can see all of the hosts that were attempted, and their status.  We will use this information to look into 

> CHALLENGE: Use the command line flags to remove the `DNS Resolution` error above

##### Step 3 - Port Scanning

Ping sweeping is a good way to determine which hosts are responding on the network, but this has several limitations:

* Limited to hosts that respond to ping
* Does not provide information about open ports

Let's add port scanning to our nmap commands.  We will the proxy we configured in previous labs:

```bash
[ntc@ntc ~]$ sudo nmap -n -p 1-65535 -sV -sS -T4 192.168.0.51

Starting Nmap 6.40 ( http://nmap.org ) at 2018-01-11 11:45 EST
Nmap scan report for 192.168.0.51
Host is up (0.00065s latency).
Not shown: 65532 closed ports
PORT     STATE SERVICE       VERSION
22/tcp   open  ssh           (protocol 2.0)
3389/tcp open  ms-wbt-server xrdp
8080/tcp open  http-proxy    Tinyproxy 1.8.3
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at http://www.insecure.org/cgi-bin/servicefp-submit.cgi :
SF-Port22-TCP:V=6.40%I=7%D=2/14%Time=5A8467E4%P=x86_64-redhat-linux-gnu%r(
SF:NULL,29,"SSH-2\.0-OpenSSH_7\.2p2\x20Ubuntu-4ubuntu2\.1\r\n");
MAC Address: 2C:C2:60:44:9C:30 (Ravello Systems)

Service detection performed. Please report any incorrect results at http://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 28.64 seconds
```

Let's examine the flags we added to that command:
* `-n`: Disable DNS Resolution
* `-p 1-65535`: Ports to scan - all ports
* `-sV`: Service & Version Detection (returns SERVICE & VERSION )
* `-sS`: Scan Technique (returns MAC Address) 
* `-T4`: Timing type 4 (_safe balance of timing and accuracy_)

In `plain-human`: 
* Check host 192.168.0.51 to determine which ports are open and listening for a TCP connection.  
* Try to determine which service (application) is listening on these open ports, and which version of that service.

##### Step 4 - Service Scanning

The example above was more useful in identifying services available on a certain host.  At times, we may need to identify a _service_ on a network, or among a list of hosts.  As such our approach would be more targeted, but against a wider subset of hosts.

Let's say we wanted to search for available webservers on our network.  Nmap has a handy way of doing this:

```bash
[ntc@ntc ~]$ nmap -n -p80,443 192.168.0.0/24

Starting Nmap 6.40 ( http://nmap.org ) at 2018-01-12 12:05 EST
RTTVAR has grown to over 2.3 seconds, decreasing to 2.0
Nmap scan report for 192.168.0.51
Host is up (0.0049s latency).
PORT    STATE  SERVICE
80/tcp  closed http
443/tcp closed https

Nmap scan report for 192.168.0.52
Host is up (0.0032s latency).
PORT    STATE  SERVICE
80/tcp  closed http
443/tcp closed https

Nmap done: 256 IP addresses (2 hosts up) scanned in 21.75 seconds
```

In the previous equest, we are searching our network for open _known_ http/s ports.

Let's target our proxy configuration from an earlier lab:

```bash
[ntc@ntc ~]$ nmap -n -p8080 192.168.0.0/24

Starting Nmap 6.40 ( http://nmap.org ) at 2018-01-12 12:09 EST
Nmap scan report for 192.168.0.51
Host is up (0.0030s latency).
PORT     STATE  SERVICE
8080/tcp open   http-proxy

Nmap scan report for 192.168.0.52
Host is up (0.0024s latency).
PORT     STATE  SERVICE
8080/tcp closed http-proxy

Nmap done: 256 IP addresses (2 hosts up) scanned in 28.16 seconds
```

Finally, let's add service discovery with `-sV`:

```bash
[ntc@ntc ~]$ sudo nmap -n -sV -p8080 192.168.0.0/24

Starting Nmap 6.40 ( http://nmap.org ) at 2018-01-12 12:16 EST
Nmap scan report for 192.168.0.1
Host is up (0.0011s latency).
PORT     STATE    SERVICE    VERSION
8080/tcp filtered http-proxy
MAC Address: 2C:C2:60:FF:00:2A (Ravello Systems)

Nmap scan report for 192.168.0.2
Host is up (0.00067s latency).
PORT     STATE    SERVICE    VERSION
8080/tcp filtered http-proxy
MAC Address: 2C:C2:60:FF:00:2F (Ravello Systems)

Nmap scan report for 192.168.0.51
Host is up (0.00089s latency).
PORT     STATE SERVICE    VERSION
8080/tcp open  http-proxy Tinyproxy 1.8.3
MAC Address: 2C:C2:60:44:9C:30 (Ravello Systems)

Nmap scan report for 192.168.0.52
Host is up (0.00038s latency).
PORT     STATE  SERVICE    VERSION
8080/tcp closed http-proxy

Service detection performed. Please report any incorrect results at http://nmap.org/submit/ .
Nmap done: 256 IP addresses (4 hosts up) scanned in 8.77 seconds
```

##### Step 5 - CHALLENGE!

Scenario: The application team is contending that the webserver (built in our previous lab) is down

> CHALLENGE: Use the flags & information above to scan the network & determine if the webserver is up on the network

Scenario 2: We are handed a set of hosts to scan - 192.168.0.51 & 192.168.0.52

> CHALLENGE: Use the flags and informaation above to scan for open ports on JUST these two hosts

> BONUS: Create a file called hosts.txt and use this to determine which hosts are to be used in the scan above

Scenario 3: We need to output our results to a report

> CHALLENGE: Create a report called `scan.txt` from the results of a verbose scan of all hosts/ports on the network.