## Lab - Vagrant CentOS

**Use your local machine for Vagrant labs.**

### Task 1 - Vagrant CentOS

##### Step 1

Add a CentOS box to you local vagrant, choosing virtualbox as provider.

```bash
ntc@ntc:~$ vagrant box add centos/7
==> box: Loading metadata for box 'centos/7'
    box: URL: https://vagrantcloud.com/centos/7
This box can work with multiple providers! The providers that it
can work with are listed below. Please review the list and choose
the provider you will be working with.

1) hyperv
2) libvirt
3) virtualbox
4) vmware_desktop

Enter your choice: 3
==> box: Adding box 'centos/7' (v1801.02) for provider: virtualbox
    box: Downloading: https://vagrantcloud.com/centos/boxes/7/versions/1801.02/providers/virtualbox.box
==> box: Successfully added box 'centos/7' (v1801.02) for 'virtualbox'!
```


##### Step 2

Verify the box has been imported.

```bash
ntc@ntc:~$ vagrant box list
centos/7                             (virtualbox, 1801.02)
juniper/ffp-12.1X47-D15.4-packetmode (virtualbox, 0.5.0)
```


##### Step 3

Create a new directory called `vagrant_centos`.

```bash
ntc@ntc:~$ mkdir vagrant_centos
ntc@ntc:~$ cd vagrant_centos
ntc@ntc:vagrant_centos
```

##### Step 4

Create a new file named `Vagrantfile` within your new `vagrant_centos` directory.  Use the following content within your new `Vagrantfile`.

**The new `Vagrantfile` must have a capital `V` at the beginning of the filename.**

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.provision :shell, path: "bootstrap.sh"
  config.vm.network "forwarded_port", guest: 80, host: 8080
end
```

- `config.vm.box = "centos/7"`: the box name.
- `config.vm.provision :shell, path: "bootstrap.sh"`: we're choosing to run the `bootstrap.sh` custom script during boot to provision our vagrant box.
- `config.vm.network "forwarded_port", guest: 80, host: 8080`: we're building a mapping between port 80 on local machine and port 8080 on the virtual machine.


##### Step 5
Create a file named `bootstrap.sh` within your new `vagrant_centos` directory.  This script is used by vagrant to install `netmiko` and `napalm` on your new `centos` machine.

```bash
# install pip
curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
sudo python get-pip.py

# install network libraries
sudo pip install netmiko
sudo pip install napalm

```


##### Step 6

Spin up the vagrant box using the `vagrant up` command within your new `vagrant_centos` directory.

```bash
ntc@ntc:vagrant_centos vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Importing base box 'centos/7'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'centos/7' is up to date...
==> default: Setting the name of the VM: vagrant_centos_default_1518647731109_7575
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 80 (guest) => 8080 (host) (adapter 1)
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default:
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default:
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
<---omitted--->
```

##### Step 7

Log into the box.

```bash
ntc@ntc:vagrant_centos$ vagrant ssh
[vagrant@localhost ~]$
```

##### Step 8

Verify that napalm and netmiko have been installed.

```bash
[vagrant@localhost ~]$ pip list
<---omitted--->
napalm (2.3.0)
<---omitted--->
netmiko (2.0.2)
<---omitted--->
```

##### Step 9

Get back to your local machine and stop the vagrant box.

```bash
[vagrant@localhost ~]$ exit
logout
Shared connection to 127.0.0.1 closed.
ntc@ntc:vagrant_centos$ vagrant halt
==> default: Attempting graceful shutdown of VM...
ntc@ntc:vagrant_centos$
```

##### Step 10

Destroy the VM with the `vagrant destroy` command.

```bash
ntc@ntc:vagrant_centos$ vagrant destroy
    default: Are you sure you want to destroy the 'default' VM? [y/N] y
==> default: Destroying VM and associated drives...
ntc@ntc:vagrant_centos$
```
