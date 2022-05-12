# Setting up SSH Access from VirtualBox Host to Ubuntu Guest VM

My environment:
- Guest: Ubuntu 20.04
- Host: Mac OS 10.14.6 Mojave
- VirtualBox: 6.1.34



## OpenSSH Server

During installation of Ubuntu OS, choose to install OpenSSH Server when prompted. If it was't done, we can still install it afterwards with command:
```bash
$ sudo apt install openssh-server
```

Check if OpenSSH server is running:
```bash
$ sudo systemctl status ssh
```

Basic control commands for OpenSSH server:
```bash
$ sudo systemctl [start/stop/restart] ssh
```



## VirtualBox virtual networking - quick overview

VirtualBox supports few modes of virtual networking. Pay attention to the mode comparison table:
https://www.virtualbox.org/manual/ch06.html

p/s: **_NAT Network_** mode is being labeled as **_NATservice_** in the mode comparison table. I have no clue about the reason.

By default, newly created VM will have one network adapter using **NAT** mode.

During my research, I came across many online Q&A/articles/blogs/videos with **NAT Network** in title, but the contents are actually for **_NAT_** mode. It seems that many people confused with those two modes, understandably, due to their similar names.

One key difference between the default **_NAT_** mode and **_NAT Network_** mode is that, **_NAT Network_** mode allows VM to VM communication, but **_NAT_** mode does not.

Since NAT is simpler than NAT Network, and the former has rich coverage by online resources, in this document I'll focus on my current project's needs: **_NAT Network_** mode and **_Host-Only_** mode. For more info about other modes, refer the above document link from VirtualBox website.



## SSH over NAT Network

To access guest VM from host using this mode, port forwarding is required.

**IMPORTANT**:  
both **_NAT Network_** and **_NAT_** have port forwarding feature, can be configured through GUI as well as command. However, these two modes use totally different command and different GUI screen. DON'T get confused.

**Instructions**:  
(skip to step #3 if NAT Network has already been setup and guest VM's network adapter is connected to it)

1. Create a VirtualBox NAT Network on host, by using menu:
   ```
   VirtualBox > Preferences > Network
   ```
   The default Network Name will be `NatNetwork`. Notice there's a Port Forwarding button? we'll come back to that soon.
   
2. Open guest VM's Network settings, change the adapter from NAT mode to NAT Network mode. Select the newly created `NatNetwork` as attached network.

3. Start the guest VM, run `ip a` command to view the list of network interfaces and assigned IP. Note down the one assigned to the NAT Network interface. Example from my VM:
   ```bash
   $ ip a

   <... first NIC>
   2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:11:da:fc brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.4/24 brd 10.0.2.255 scope global dynamic enp0s3
       valid_lft 465sec preferred_lft 465sec
    inet6 fe80::a00:27ff:fe11:dafc/64 scope link 
       valid_lft forever preferred_lft forever
   <... third NIC>
   ```
   
4. On the NAT Network setting screen (refer step #1), click the Port Forwarding button, create a port forwarding rule:
   ```
   Name: SSH port forwarding <your choice of rule name>
   Protocol: TCP
   Host IP: <leave it blank>
   Host Port: 22221 <your desired port>
   Guest IP: 10.0.2.4 <the guest VM IP noted down in step #3>
   Guest Port: 22 <the default port SSH server listening to>
   ```
   Click OKs to close all dialogs and apply the new setting.  
   The NAT Network on the host will be listening at selected host port (`22221`), and forward received `TCP` traffic to guest VM's port `22`.
   
5. Test: from host command line, try SSH by using the newly set-up port forwarding:
   ```bash
   $ ssh -p 22221 ubuntu@127.0.0.1
   ```
   This sends a SSH request to the host (`127.0.0.1`) at port `22221` (where the NAT Network is listening to), as user `ubuntu` (the guest VM's default user created during OS installation).  
   This request will be forwarded to guest VM as per rule configured in step #4.



## SSH over Host-Only network

Host-Only network is a virtual network with host-only virtual adapters.
This is my choice of SSH setup for local development environment because:
- It is more secure, as no communication can happen between VMs and Internet through this network.
- It is more straight forward, no port forwarding is required.

We can still give VM access to Internet through additional network interface, e.g. of NAT mode or NAT Network mode.

**Instructions**:  
(skip to step #3 if Host-Only Network has already been setup and guest VM's network adapter is connected to it)

1. Add a virtual adapter to the VirtualBox Host-Only network on host, by using menu:
   ```
   File > Host Network Manager
   ```
   The default adapter Name will be `vboxnet0`. Once created, tick the checkbox to enable DHCP server.  
   In certain older versions of VirtualBox, the menu for accessing this UI is different:
   ```
   VirtualBox > Settings > Network
   ```
   
2. Open guest VM's Network settings, add a network adapter and connect it to a `Host-only Adapter`. Select the newly created `vboxnet0` adapter as target.

3. Start the guest VM, run `ip a` command to view the list of network interfaces and assigned IP. Note down the IP assigned to the interface connected to Host-Only adapter. Example from my VM:
   ```bash
   $ ip a

   <... first NIC>
   <... second NIC>
   3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
       link/ether 08:00:27:7b:08:93 brd ff:ff:ff:ff:ff:ff
       inet 192.168.56.102/24 brd 192.168.56.255 scope global dynamic enp0s8
          valid_lft 464sec preferred_lft 464sec
       inet6 fe80::a00:27ff:fe7b:893/64 scope link 
          valid_lft forever preferred_lft forever
   ```
   
4. Test: from host command line, try SSH directly to the IP identified in step #3:
   ```bash
   $ ssh ubuntu@192.168.56.102
   ```
   where `ubuntu` is the guest VM's default user created during OS installation.



## Guest VM static IP setup

Static IP is **NOT** required for SSH. However, it is still highly recommended to use static IP if:
- the guest VM has web server running, e.g. hosting website or serving web APIs, or
- the guest VM has need to support access through port forwarding.

We don't want the hassle of keep checking VM IP, then keep updating website URL or port forwarding rule whenever the dynamic IP address changed.

Refer this document for configuring static IP on Ubuntu 17.10 or later:  
https://github.com/shiouming/technotes/blob/main/linux/configure-network-interface.md#ubuntu

**IMPORTANT**:  
If we choose a static IP different from what we configured in SSH setup (e.g. port forwarding rule), we need to update those settings as well.

