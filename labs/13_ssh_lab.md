## Lab 11 - SSH access, keys, and configuration

In this lab we will learn about SSH access in Linux, as well as advanced configuration parameters for using SSH effectively.

### Task 1 - Generating SSH Keys

##### Step 1 - Generate Key

Create a new ssh key pair using the `ssh-keygen` command.  We will create an [RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) key in this lab

```
[ntc@ntc ~]$ ssh-keygen -t rsa -b 4096
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ntc/.ssh/id_rsa):
Created directory '/home/ntc/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/ntc/.ssh/id_rsa.
Your public key has been saved in /home/ntc/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:VKxJehWj6PjCDdZBeAlddX5205ivgoKIHBS41yKbZ9k ntc@ntc
The key's randomart image is:
+---[RSA 4096]----+
| .. .+.o.o=..    |
|.  ...+..oo+   o.|
| ... .ooo+  . =.o|
|o.o .+.o+    o o.|
| =.++ o.S       .|
|o.+=E= .   .   . |
| oo + + . . . .  |
|     .   .   .   |
|                 |
+----[SHA256]-----+

```

In the example above we supplied two command line flags:
* `-t` or "type" - The type of key we are generating `rsa`
* `-b` or "bits" - The number of bits in the key to create `4096`

These are generally the minimum amount of flags necessary to create a key pair

The next few prompts determine the name and location of the key pair, and if it should be secured with a passphrase (additional security measure). Pressing <ENTER> at the prompts accepts the defaults in parenthesis

_Alternatively, you can supply answers to the prompts via command line flags_

##### Step 2 - View `~/.ssh` Directory

During the "keygen" process, a hidden directory called `~/.ssh` is created (if none exist). Use the `tree` command to explore this directory.

```
[ntc@ntc ~]$ tree ~/.ssh
/home/ntc/.ssh
├── id_rsa
└── id_rsa.pub

0 directories, 2 files
[ntc@ntc ~]$ cd ~/.ssh
[ntc@ntc .ssh]$ tree
.
├── id_rsa
└── id_rsa.pub

0 directories, 2 files
   
```

These files will facilitate a secure connection between our client, and a target device.  Essentially these keys will provide a cryptographic strength that is stronger than even the longest, most complicated passwords can offer.

Each key pair includes a public & private key.  In the example above, the public key is `id_rsa.pub`, and the private key is `id_rsa`. _It is a common practice to ensure public keys end in `.pub`_

> Public Key

The 'public key' `id_rsa.pub` is considered 'public', and is intended to be shared with target devices.  It will be used by the target device to authenticate the client device.  Let's view the contents:

```
[ntc@ntc ~]$ cat ~/.ssh/id_rsa.pub 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDR5J7Y8u5hizWwTloogRXK743fq16Tbr1C1mKBVNB/J0PwhyudoLFPsOK5YzyyZRT5Dny4wREFdS1zD0KE8bbP67SIO9Ary8HAtu/eINUmCgHv227uqRuAn5D1NYMwa93WY7AOTJIOZ3mwHMfRsssoJktIX6e1/69nApHesSdhSqgEbVu3RPc+egWnOlN3GK0ar7IdcadiZ9EL+Jj/L3Is+hQ5s2LnfTecoVnvJjxuX2aZHPeTnuwDWuAIUAOpDFTEfXy3FclJIcx79pofhJuGwiV34S1EBHzGaumKjtj+sF/uq0925I0K6nxT8CleM5dK+jMGBCdqMMT0JGAzvxZxjse3o29lchcPsRu8OV2UTO5lCXohP+mEJZJKFLiz9bN6jOWqQYPSoBGfoBttzQCmZSVf5z2fHJvmx9+Ltp7uAz6RxFbceFFreTzPiS/r5vA2khcR+YeRx393Pk1F6zp0F0y1HNlkhYQJ9H0079u7Kc8UNy40XqyNKiyZVbdweOW86PlUxpTYaPgKNbpDkhIct2ZnZ/S/HJYZTBuE7+wr8XAizji5zESOiKcJIpWguvQk4dp+jtVm9w+9p+McnPJBJPrawfYts9dUK2B9eXNHSk1pK+knoozd59NB0vXP3h5DkR8Uh2HjFmdZ+rFVUh0RCRxcS8w8j6EuJy3b4iS6Qw== ntc@ntc
```

> Private Key

The 'private key' `id_rsa` should remain only with the client device. It is considered highly secure, and proves the client's identity.  Private keys *should not* be shared or distributed.  Lets view the contents:

```
[ntc@ntc ~]$ cat ~/.ssh/id_rsa
-----BEGIN RSA PRIVATE KEY-----
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
-----END RSA PRIVATE KEY-----
```

> More information on public/private keys and ssh authentication can be found [here](https://www.ssh.com/ssh/public-key-authentication)


##### Step 3 - SSH to Proxy


SSH to `proxy.ntc.com`

```
[ntc@ntc .ssh]$ ssh ntc@proxy.ntc.com
```

In order to proceed, you will need to accept the public key of `proxy.ntc.com`:

```
The authenticity of host 'proxy.ntc.com (192.168.0.51)' can't be established.
ECDSA key fingerprint is SHA256:anly9GgDgacLCUh24DgfoQp9I/dnOpLgzEhkUaOAmHU.
ECDSA key fingerprint is MD5:e0:c5:83:19:3f:68:1c:8a:9b:fe:46:74:d2:06:e3:cb.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'proxy.ntc.com,192.168.0.51' (ECDSA) to the list of known hosts.
```

_note the last line mentions "known hosts"_

You are then prompted for the password:
```
ntc@proxy.ntc.com's password:
```

Once entered, the login is successful:

```
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-59-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

320 packages can be updated.
175 updates are security updates.

Last login: Wed Feb  7 10:22:56 2018 from 8.8.8.8
```

##### Step 4 - Revisit `~/.ssh` Directory 

Navigate to the `~/.ssh` directory and issue the `tree` command again.

```
ntc@ntc:~$ exit
logout
Connection to proxy.ntc.com closed.
[ntc@ntc .ssh]$ tree
.
├── id_rsa
├── id_rsa.pub
└── known_hosts

0 directories, 3 files
```

There is now a `known_hosts` file with an entry for the proxy and its public key:
```
[ntc@ntc .ssh]$ cat known_hosts 
proxy.ntc.com,192.168.0.51 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBJXdpRSZOWwBI1p2YdMnuJEnlAUw5pRH27y7xqvitLUTJXA42v+Y/nsoAnH1qxDNGuMLlRjvcW24fTPsEyIowk0=
```

##### Step 5 - SSH Without Password

SSH access can be accomplished without requiring a password using _Public Key Authentication_.  To do this, you will need to copy the public key - `id_rsa.pub` - we created in an earlier step.

This can be done easily using the `ssh-copy-id` utility:

```
[ntc@ntc .ssh]$ cat known_hosts
proxy.ntc.com,192.168.0.51 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBJXdpRSZOWwBI1p2YdMnuJEnlAUw5pRH27y7xqvitLUTJXA42v+Y/nsoAnH1qxDNGuMLlRjvcW24fTPsEyIowk0=
[ntc@ntc .ssh]$ clear

[ntc@ntc .ssh]$ ssh-copy-id ntc@proxy.ntc.com
/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/ntc/.ssh/id_rsa.pub"
/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
ntc@proxy.ntc.com's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'ntc@proxy.ntc.com'"
and check to make sure that only the key(s) you wanted were added.

[ntc@ntc .ssh]$
```

Once the public key is copied successfully, we can connect to our proxy without password authentication:

```
[ntc@ntc .ssh]$ ssh ntc@proxy.ntc.com
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-59-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

320 packages can be updated.
175 updates are security updates.

Last login: Wed Feb  7 11:30:45 2018 from 8.8.8.8
ntc@ntc:~$
```

### Task 2 - SSH Configuration

##### Step 1 - SSH Config file

Create a `config` file within the `~/.ssh` directory

```
[ntc@ntc .ssh]$ touch ~/.ssh/config && chmod 0600 ~/.ssh/config
```

We can use this file to manipulate our SSH configuration.  Add the following to the file:

```
Host bastion.ntc.com
  Hostname proxy.ntc.com
  User ntc
  ForwardAgent yes
```

We should now be able to ssh to `bastion.ntc.com`:

```
[ntc@ntc .ssh]$ ssh bastion.ntc.com
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-59-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

320 packages can be updated.
175 updates are security updates.

Last login: Wed Feb  7 11:38:15 2018 from 8.8.8.8

```

Unlike other aliases, this only impacts SSH.  It also enables us to adjust ssh parameters on a per-host basis

### Task 3 - SSH Proxying

##### Step 1 - ProxyCommand: cli

At times, we will need to connect to network devices via an intermediary device - a proxy or bastion host.  To do this, we can use several methods.  The most widely used (and simple) is the `ProxyCommand` option in ssh to `bounce` through an intermediate host

`ProxyCommand` can be issued via the command line or within the `~/.ssh/config` file:

Command line:

```
ssh -o ProxyCommand="ssh -W %h:%p bastion.ntc.com" veos1
```

> Explanation:
* `-o` or 'option' - allows us to invoke `ProxyCommand` option
* `-W %h:%p` - instructs ssh client to 'bounce' connection through the `%h host` and `%p port` of the intermediate host
* `bastion.ntc.com` - intermediate host to use for access to destination
* `veos1` - ultimate destination of ssh command _must be accessible by the intermediate_

> Output:

```
[ntc@ntc .ssh]$ ssh -o ProxyCommand="ssh -W %h:%p bastion.ntc.com" veos1
The authenticity of host 'eos-spine1 (<no hostip for proxy command>)' can't be established.
RSA key fingerprint is SHA256:vujvX1sLqwQpyN7ESYtndV/KeLn5xoZ/j+XEm1JHWmY.
RSA key fingerprint is MD5:f0:b0:5d:b6:6a:ee:9d:80:20:2e:6e:eb:04:c5:36:43.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'eos-spine1' (RSA) to the list of known hosts.
Password: 
eos-spine1#
```

As you can see in the output above, the connection finally lands at the `eos-spine1`

##### Step 2 - ProxyCommand: config file

Command line flags are perfect for one-time tasks, or troubleshooting.  In practice, a more permanent solution can yield better results.  We can permanently ensure the use of an SSH Proxy by adding to our `~/.ssh/config` file.

Config file:

Add the following to the `~/.ssh/config` file:

```
Host veostest
  Hostname eos-spine1
  User ntc
  ForwardAgent yes
  ProxyCommand ssh -W %h:%p bastion.ntc.com veos1
```

Now attempt to ssh to the alias `veostest`:

```
[ntc@ntc .ssh]$ ssh veostest
```
  
> Output:

```
[ntc@ntc .ssh]$ ssh veostest
Password: 
Last login: Wed Feb  7 17:16:24 2018 from 10.0.0.3
eos-spine1#

```

### Task 4 - Advanced SSH

##### Step 1 - ASA Config Entry

Building from the last example, lets add an entry in our config for `asa1`:

```
Host asa1
  Hostname asa1
  User ntc
  ForwardAgent yes
  ProxyCommand ssh -W %h:%p bastion.ntc.com asa1
```

##### Step 2 - ASA SSH connection

Attempt to connect to a our `asa1` via SSH:

```
[ntc@ntc ~]$ ssh ntc@asa1
Unable to negotiate with 192.168.33.96 port 22: no matching key exchange method found. Their offer: diffie-hellman-group1-sha1
```

The error is showing that we cannot connect with the ASA due to a bad key exchange.  By default older ASAs (and other devices) use Key Exchange Algorithms that are less secure than current Linux SSH standards.  Ideally, we would want to upgrade the security on the target device.  However, when that is not possible, we can elect to adjust the security on the client.

As this is a _potential_ security risk, it is not desirable to make this change globally.  Instead, we can use the `~/.ssh/config` to adjust this per host.  To troubleshoot further, increase the ssh verbosity by adding `-v` to the ssh command:

```
[ntc@ntc ~]$ ssh ntc@asa1 -v
OpenSSH_7.4p1, OpenSSL 1.0.2k-fips  26 Jan 2017
debug1: Reading configuration data /home/ntc/.ssh/config
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 59: Applying options for *
debug1: Connecting to asa1 [192.168.33.96] port 22.
debug1: Connection established.
### cut for brevity ###
debug1: Authenticating to asa1:22 as 'ntc'
debug1: SSH2_MSG_KEXINIT sent
debug1: SSH2_MSG_KEXINIT received
debug1: kex: algorithm: (no match)
```

Continue to increase the verbosity by appending additional `v` to the end of the command.  Eventually, you will see a mismatch between what the target is offering, and what the client accepts:

```
debug2: KEX algorithms: diffie-hellman-group-exchange-sha256,curve25519-sha256@libssh.org,ext-info-c # client acceptance
debug2: KEX algorithms: diffie-hellman-group1-sha1 # target offer
```

##### Step 3 - Edit Algorithms

To resolve this, we can edit the `~/.ssh/config` file for the ASA to match their offer:

```
Host asa1
  User ntc
  ForwardAgent yes
  ProxyCommand ssh -W %h:%p bastion.ntc.com asa1
  KexAlgorithms +diffie-hellman-group1-sha1
```

Attempt to to ssh to the ASA again:

```
[ntc@ntc ~]$ ssh ntc@asa1
asa1>
```
