## Lab - Vagrant

**Use your local machine for Vagrant labs.**

### Task 1 - Build multiple boxes

##### Step 1

Download the EOS vagrant box from [this](https://drive.google.com/drive/folders/1OQvC7qCFygTRXuR76xqt2fBovcgbOy_7) link.

##### Step 2

Add to vagrant the box you've just downloaded and rename it as `vEOS-lab-4.18.5M`.

```bash
ntc@ntc:~$ vagrant box add --name vEOS-lab-4.18.5M ~/Downloads/vEOS-lab-4.18.5M-virtualbox.box
==> box: Box file was not detected as metadata. Adding it directly...
==> box: Adding box 'vEOS-lab-4.18.5M' (v0) for provider:
    box: Unpacking necessary files from: file:///Users/ggerbino/Downloads/vEOS-lab-4.18.5M-virtualbox.box
==> box: Successfully added box 'vEOS-lab-4.18.5M' (v0) for 'virtualbox'!
```


##### Step 2

Verify the box has been imported.

```bash
ntc@ntc:~$ vagrant box list
vagrant box list
centos/7                             (virtualbox, 1801.02)
juniper/ffp-12.1X47-D15.4-packetmode (virtualbox, 0.5.0)
vEOS-lab-4.18.5M                     (virtualbox, 0)
```


##### Step 3

Create a new directory called `vagrant_multiple` and move into it.

```bash
ntc@ntc:~$ mkdir vagrant_multiple
ntc@ntc:~$ cd vagrant_multiple
ntc@ntc:vagrant_multiple
```

##### Step 4

Create a new Vagrantfile with a CentOS box. Map port 22 into port 12200 and add a private network interface.

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.define "centos" do |centos|
    centos.vm.box = "centos/7"
    centos.vm.network :forwarded_port, guest: 22, host: 12200, id: 'ssh'
    centos.vm.network "private_network", virtualbox__intnet: "link_1",
                                       ip: "169.254.1.10"
  end
```

##### Step 5

Append a JunOS box to the Vagrantfile. Map port 22 into port 12203 and add a private network interface.

```ruby
  config.vm.define "junos" do |junos|
    junos.vm.box = "juniper/ffp-12.1X47-D15.4-packetmode"
    junos.vm.network :forwarded_port, guest: 22, host: 12203, id: 'ssh'
    junos.vm.network "private_network", virtualbox__intnet: "link_1",
                                        ip: "169.254.1.12", auto_config: false
  end
```


##### Step 6

Append a JunOS box to the Vagrantfile. Map port 22 into port 12201 and port 443 into port 12443. Also add a private network interface.

```ruby
  config.vm.define "eos" do |eos|
    eos.vm.box = "vEOS-lab-4.18.5M"

    eos.vm.network :forwarded_port, guest: 22, host: 12201, id: 'ssh'
    eos.vm.network :forwarded_port, guest: 443, host: 12443, id: 'https'

    eos.vm.network "private_network", virtualbox__intnet: "link_1",
                                      ip: "169.254.1.13", auto_config: false
  end
end
```

##### Step 7

Run `vagrant up` to spin up the topology.

```bash
ntc@ntc:vagrant_multiple$ vagrant up
Bringing machine 'centos' up with 'virtualbox' provider...
Bringing machine 'eos' up with 'virtualbox' provider...
Bringing machine 'junos' up with 'virtualbox' provider...
==> centos: Importing base box 'centos/7'...
==> centos: Matching MAC address for NAT networking...
==> centos: Checking if box 'centos/7' is up to date...
==> centos: Setting the name of the VM: vagrant_centos_centos_1518769980522_52766
==> centos: Clearing any previously set network interfaces...
==> centos: Preparing network interfaces based on configuration...
    centos: Adapter 1: nat
    centos: Adapter 2: intnet
    centos: Adapter 3: intnet
==> centos: Forwarding ports...
    centos: 22 (guest) => 12200 (host) (adapter 1)
==> centos: Booting VM...
==> centos: Waiting for machine to boot. This may take a few minutes...
    centos: SSH address: 127.0.0.1:12200
    centos: SSH username: vagrant
    centos: SSH auth method: private key
    centos:
    centos: Vagrant insecure key detected. Vagrant will automatically replace
    centos: this with a newly generated keypair for better security.
    centos:
    centos: Inserting generated public key within guest...
    centos: Removing insecure key from the guest if it's present...
    centos: Key inserted! Disconnecting and reconnecting using new SSH key...
==> centos: Machine booted and ready!
<---omitted--->
```

The process may take a while and it may end up with a warning for the EOS box, but it should still be up and running.

##### Step 8

Try to ssh into each box.

```bash
ntc@ntc:vagrant_multiple$  vagrant ssh eos

Arista Networks EOS shell

-bash-4.3#
```

```bash
ntc@ntc:vagrant_multiple$  vagrant ssh centos
[vagrant@localhost ~]$
```

```bash
ntc@ntc:vagrant_multiple$ vagrant ssh junos
--- JUNOS 12.1X47-D15.4 built 2014-11-12 02:13:59 UTC
root@vsrx%
```

##### Final Vagrantfile

Your final `Vagrantfile` should look like this.

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.define "centos" do |centos|
    centos.vm.box = "centos/7"
    centos.vm.network :forwarded_port, guest: 22, host: 12200, id: 'ssh'
    centos.vm.network "private_network", virtualbox__intnet: "link_1", ip: "169.254.1.10"
  end
  config.vm.define "junos" do |junos|
    junos.vm.box = "juniper/ffp-12.1X47-D15.4-packetmode"
    junos.vm.network :forwarded_port, guest: 22, host: 12203, id: 'ssh'
    junos.vm.network "private_network", virtualbox__intnet: "link_1", ip: "169.254.1.12", auto_config: false
  end
  config.vm.define "eos" do |eos|
    eos.vm.box = "vEOS-lab-4.18.5M"
    eos.vm.network :forwarded_port, guest: 22, host: 12201, id: 'ssh'
    eos.vm.network :forwarded_port, guest: 443, host: 12443, id: 'https'
    eos.vm.network "private_network", virtualbox__intnet: "link_1", ip: "169.254.1.13", auto_config: false
  end
end
```