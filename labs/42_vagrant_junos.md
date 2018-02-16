## Lab - Vagrant Junos

### Task 1 - Vagrant Junos

In this lab we will get some practice with Vagrant and we will create a lab with Junos boxes.

##### Step 1

As first step, let's explore the Vagrant command line by getting the `help` output.

```
ntc@ntc:~$ vagrant --help
Usage: vagrant [options] <command> [<args>]

    -v, --version                    Print the version and exit.
    -h, --help                       Print this help.

Common commands:
     box             manages boxes: installation, removal, etc.
     connect         connect to a remotely shared Vagrant environment
     destroy         stops and deletes all traces of the vagrant machine
     global-status   outputs status Vagrant environments for this user
     halt            stops the vagrant machine
     help            shows the help for a subcommand
     init            initializes a new Vagrant environment by creating a Vagrantfile
     login           log in to HashiCorp's Vagrant Cloud
     package         packages a running vagrant environment into a box
     plugin          manages plugins: install, uninstall, update, etc.
     port            displays information about guest port mappings
     powershell      connects to machine via powershell remoting
     provision       provisions the vagrant machine
     push            deploys code in this environment to a configured destination
     rdp             connects to machine via RDP
     reload          restarts vagrant machine, loads new Vagrantfile configuration
     resume          resume a suspended vagrant machine
     share           share your Vagrant environment with anyone in the world
     snapshot        manages snapshots: saving, restoring, etc.
     ssh             connects to machine via SSH
     ssh-config      outputs OpenSSH valid configuration to connect to the machine
     status          outputs status of the vagrant machine
     suspend         suspends the machine
     up              starts and provisions the vagrant environment
     validate        validates the Vagrantfile
     vbguest         plugin: vagrant-vbguest: install VirtualBox Guest Additions to the machine
     version         prints current and latest Vagrant version

For help on any individual command run `vagrant COMMAND -h`

Additional subcommands are available, but are either more advanced
or not commonly used. To see all subcommands, run the command
`vagrant list-commands`.
```

As you can see lot of commands exist, but we are actually interested on only few of them.

##### Step 2

Before importing a Junos box, we need to install a couple of plugins. First, install the `vagrant-junos` using the `plugin` command.

Note: Windows user may fail to install this plugin.

```
ntc@ntc:~$ vagrant plugin install vagrant-junos
Installing the 'vagrant-junos' plugin. This can take a few minutes...
Fetching: vagrant-junos-0.2.1.gem (100%)
Installed the plugin 'vagrant-junos (0.2.1)'!
```


##### Step 3

Now install the `vagrant-host-shell` plugin.

Note: Windows user may fail to install this plugin.

```
$ntc@ntc:~$ vagrant plugin install vagrant-host-shell
Installing the 'vagrant-host-shell' plugin. This can take a few minutes...
Fetching: vagrant-host-shell-0.0.4.gem (100%)
Installed the plugin 'vagrant-host-shell (0.0.4)'!
```

##### Step 4

Now we're ready to import the Junos box using the `box` command. Choose `virtualbox` when prompted with the provider choice.
```
ntc@ntc:~$ vagrant box add juniper/ffp-12.1X47-D15.4-packetmode
==> box: Loading metadata for box 'juniper/ffp-12.1X47-D15.4-packetmode'
    box: URL: https://vagrantcloud.com/juniper/ffp-12.1X47-D15.4-packetmode
This box can work with multiple providers! The providers that it
can work with are listed below. Please review the list and choose
the provider you will be working with.

1) virtualbox
2) vmware_desktop

Enter your choice: 1
==> box: Adding box 'juniper/ffp-12.1X47-D15.4-packetmode' (v0.5.0) for provider: virtualbox
    box: Downloading: https://vagrantcloud.com/juniper/boxes/ffp-12.1X47-D15.4-packetmode/versions/0.5.0/providers/virtualbox.box
==> box: Successfully added box 'juniper/ffp-12.1X47-D15.4-packetmode' (v0.5.0) for 'virtualbox'!
```

##### Step 5

Verify we have correctly imported the box with the `vagrant box list` command.

```
$ntc@ntc:~$ vagrant box list
juniper/ffp-12.1X47-D15.4-packetmode (virtualbox, 0.5.0)
```

##### Step 6

Create a new directory called `vagrant_junos`.

```
ntc@ntc:~$ mkdir vagrant_junos
ntc@ntc:~$ cd vagrant_junos
ntc@ntc:vagrant_junos$
```

##### Step 7

Initialize a Vagrant file with the newly imported box.

```
ntc@ntc:vagrant_junos$ vagrant init juniper/ffp-12.1X47-D15.4-packetmode
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.
```

The command creates a new Vagrant file with the following content.

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "juniper/ffp-12.1X47-D15.4-packetmode"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end
```


##### Step 8

Spin up the vagrant box using the `vagrant up` command.

Note: depending on your OS you may receive a warning regarding guest additions. Despite this, the box will boot up correctly.

```
ntc@ntc:vagrant_junos$ vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Importing base box 'juniper/ffp-12.1X47-D15.4-packetmode'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'juniper/ffp-12.1X47-D15.4-packetmode' is up to date...
==> default: Setting the name of the VM: vagrant_junos_default_1518645235836_60641
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: root
    default: SSH auth method: private key
    default:
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default:
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
Sorry, don't know how to check guest version of Virtualbox Guest Additions on this platform. Stopping installation.
==> default: Checking for guest additions in VM...
    default: No guest additions were detected on the base box for this VM! Guest
    default: additions are required for forwarded ports, shared folders, host only
    default: networking, and more. If SSH fails on this machine, please install
    default: the guest additions and repackage the box to continue.
    default:
    default: This is not an error message; everything may continue to work properly,
    default: in which case you may ignore this message.
```

##### Step 9

Use `vagrant status` to verify the box state.

```
ntc@ntc:vagrant_junos$ vagrant status
Current machine states:

default                   running (virtualbox)

The VM is running. To stop this VM, you can run `vagrant halt` to
shut it down forcefully, or you can run `vagrant suspend` to simply
suspend the virtual machine. In either case, to restart it again,
simply run `vagrant up`.
```

##### Step 10

Log into the box with `vagrant ssh`.

```
ntc@ntc:vagrant_junos$ vagrant ssh
--- JUNOS 12.1X47-D15.4 built 2014-11-12 02:13:59 UTC
root@vsrx%
```

##### Step 11

Get back to your local machine and stop the vagrant box.

```
root@vsrx% exit
ntc@ntc:vagrant_junos$ vagrant halt
==> default: Attempting graceful shutdown of VM...
ntc@ntc:vagrant_junos$
```
