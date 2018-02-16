layout: true

.footer-picture[![Network to Code Logo](slides/media/Footer2.PNG)]
.footnote-left[(C) 2015 Network to Code, LLC. All Rights Reserved. ]
.footnote-con[CONFIDENTIAL]

---

class: center, middle, title
.footer-picture[<img src="slides/media/Footer1.PNG" alt="Blue Logo" style="alight:middle;width:350px;height:60px;">]

# DevOps tools : VirtualBox & Vagrant


---

# Module Overview

- Introduction to VirtualBox
- Introduction to Vagrant
---

# VirtualBox

- VirtualBox is a cross-platform virtualization application, that can run multiple operating systems as virtual machines the same time. 

Typical use cases:
- Testing and disaster recovery
- Infrastructure consolidation
- Evaluating Software


---
# VirtualBox terminology

- **Host operating system (host OS)**: This is the operating system of the physical computer on which VirtualBox was installed. There are versions of VirtualBox for Windows, Mac OS X, Linux and Solaris hosts
- **Guest operating system (guest OS)**: This is the operating system that is running inside the virtual machine
- **Virtual machine (VM)**:This is the special environment that VirtualBox creates for the guest operating system. 
- **Guest Additions**: Special software packages which are shipped with VirtualBox but designed to be installed inside a VM to improve performance of the guest OS and to add extra features    


Examples of guest additions:
- Mouse pointer integration
- Shared folders
- Time Sync
- Shared clipboard
- Credential passing

---
# VirtualBox manager

When starting for the first time:


<img src="slides/media/vbox_vagrant/virtualbox-main-empty.png" alt="Pep8" style="align:middle:;width:750px;height:400px;">


---
# VirtualBox manager

After installing and configuring VirtualBox

<img src="slides/media/vbox_vagrant/virtualbox-main.png" alt="Pep8" style="align:middle:;width:750px;height:400px;">

---
# Network adapters

VirtualBox provides up to eight virtual PCI Ethernet cards for each virtual machine. For each such card, you can individually select

1. The hardware that will be virtualized as well as

2. The virtualization mode that the virtual card will be operating in with respect to your physical networking hardware on the host.


---
# Network adapter hardware 

For each card, you can individually select what kind of hardware will be presented to the virtual machine. VirtualBox can virtualize the following six types of networking hardware:

AMD PCNet PCI II (Am79C970A);

**AMD PCNet FAST III (Am79C973, the default);**

Intel PRO/1000 MT Desktop (82540EM);

Intel PRO/1000 T Server (82543GC);

Intel PRO/1000 MT Server (82545EM);

Paravirtualized network adapter (virtio-net).

---

# Network adapter modes


- **Not attached**: Network card present but not connected
- **Network Address Translation (NAT)**: Default mode, uses host's network settings for external connectivity
- **Host-only networking**: A virtual network interface (similar to a loopback interface) is created on the host, providing connectivity among virtual machines and the host.
- **Internal networking**: Software-based network which is visible to selected virtual machines, but not to applications running on the host or to the outside world.
- **Bridged networking**: When enabled, VirtualBox connects to one of your installed network cards and exchanges network packets directly, circumventing your host operating system's network stack.


---
# Demo/Guided Lab

- Demo



---
# Lab Time

- Lab 37 - Installing VirtualBox
- Lab 38 - Virualizing JUNOS

---


# Vagrant


