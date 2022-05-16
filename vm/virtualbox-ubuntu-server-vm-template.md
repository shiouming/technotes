# Creating an Ubuntu Server Guest VM as Template

**Benefit of creating VM Template (or Base)**  
If we need to create more VM instances of the same OS and configuration baseline, we can get it done in few minutes by using Clone feature, to create one from the VM template.

My environment:

- Guest: Ubuntu Server 20.04
- Host: Mac OS 10.14.6 Mojave
- VirtualBox: 6.1.34
- Virtual networks: `NAT Network` and `Host-only Network`

Table of contents:
1. [Creating VM Template](#creating-vm-template)
   - [VM configuration and OS installation](#vm-configuration-and-os-installation)
   - [Post OS installation](#post-os-installation)
1. [Cloning VM Template into New Instance](#cloning-vm-template-into-new-instance)


## Creating VM Template

### VM configuration and OS installation

1. Download the Ubuntu Server 20.04 installation disk image from Ubuntu website:
   online installer 20.04.4: https://releases.ubuntu.com/20.04.4/ubuntu-20.04.4-live-server-amd64.iso (~1.33GB)
1. Create `NAT Network` and `Host-only Network` on host, if not available. Below page contain the instructions:  
   https://github.com/shiouming/technotes/blob/main/vm/virtualbox-ubuntu-guest-ssh-setup.md#ssh-over-nat-network  
   NOTE: If we don't need VM-VM communication, we can use the simpler `NAT` instead of `NAT Network`.  
1. Create a new VM (as template) with below configuration:
   - VM Name: Ubuntu_20_04_amd64_tmpl.
   - Memory: 1GB
   - Storage Disk: VDI type, 15GB (dynamic allocation)
   - Network Adapters:  
      - Change the default `NAT` mode NIC to use `NAT Network` mode instead, and attach it to the previously created NAT Network.
      - Add second NIC of `Host-only` mode, to enable direct access from host (e.g. SSH), so that no port-forwarding required.
1. Under the VM's Storage setting, mount the IDE CD drive with the previously downloaded Ubuntu installation image file.
1. Start the VM, it should boot from the Ubuntu installation CD drive.
1. Go through the installation steps. Pay attention to following settings:  
   - **Storage disk**: even if we selected the option to use whole disk's space, on next screen the partition plan shows that the installer will only create root partition (`/`) of 10GB space, leaving remaining space unallocated. Please manually edit the partition plan to make full use of the disk space (~13.6GB, as `/boot` partition takes ~1.3GB).
   - **Machine profile setup**:  
      - Server name: `ubuntu-20-04-amd64-tmpl`
      - Username (default sudoer user): `ubuntu`
      - Password and Confirm password: _<choosen_password>_
   - Ubuntu Advantage subscription: leave it blank
   - Server Snaps: install none of them    
1. Reboot once installation is completed. If getting CD unmount error during reboot, eject the installer disk image from the VM's setting, then press `ENTER` key to continue.

### Post OS installation

**IMPORTANT**: Since this is a VM template, do **_NOT_** set up static IP on it.

1. On first login (as user ubuntu), run below commands to refresh OS baseline:
   ```bash
   $ sudo apt update 
   $ sudo apt upgrade; sudo apt full-upgrade
   $ sudo apt autoremove; sudo apt autoclean
   ```
1. Verify storage disk space: `$ df -h`. 
   the root partition (`/`) should be roughly ~13+GB, used ~4.2GB, available ~8.5GB.
1. If the server name wasn't entered correctly during OS installation, run below command to correct it:
   ```bash
   $ sudo hostnamectl set-hostname ubuntu-20-04-amd64-tmpl
   ```
   and manually fix the hostname in IP to host mapping file:
   ```bash
   $ sudo vim /etc/hosts
   ```
1. Check the network interfaces and assigned IP addresses with command `$ ip a`. There should be 3 entries in output:  
   - local loopback - 127.0.0.1
   - enp0s3 - 10.0.2.x (assigned by the NAT Network DHCP server)
   - enp0s8 - 192.168.56.x (assigned by the Host-Only Network DHCP server)
1. Test SSH by logging in as user `ubuntu`, from host command line to guest VM, via IP of Host-Only NIC:
   ```bash
   $ ssh ubuntu@192.168.56.x
   ```
1. Revisit the previous [screen of `NAT Network` setting on host](https://github.com/shiouming/technotes/blob/main/vm/virtualbox-ubuntu-guest-ssh-setup.md#ssh-over-nat-network):  
   - Add a port-forwarding rule to this new VM
   - Test SSH through the port-forwarding.
1. Follow below page to zero fill and shrink the virtuak disk VDI file. In my setup, this step made the template ~1GB smaller:  
   https://github.com/shiouming/technotes/blob/main/vm/virtualbox-shrink-vdi-disk-file.md
1. By default, there's an issue if we use certain versions of Ubuntu as VM template.  
   For Ubuntu 18.10 and newer versions, Ubuntu uses machine-id instead of MAC address in DHCP requests. Since all VM clones will have the same machine-id, this causes all instances to be assigned with the same IP address, even if they have different MAC addresses.  
   Follow below page to apply the workaround. This fix must be performed as LAST STEP:  
   TODO
1. Shut the VM down. From VirtualBox UI, create a snapshot for this new VM template.  
   In case we accidentally launch the tempate and mess it up in the future, we can still restore the template to this stable state.


## Cloning VM Template into New Instance

1. In VirtualBox UI, select the template VM and click "Clone" button. Enter the new VM settings:
   - Name: desired new VM name
   - Path: use the default one, or change it per your need
   - MAC Address Policy: `Generate new MAC addresses for all network adapters`
   - Clone type: `Full clone`
   - Snapshots: `Everything`
1. It takes few minutes to create the new VM.
1. Once clone operation is complete, log in as user `ububtu`.
1. Set a new hostname for this new VM:
   ```bash
   $ sudo hostnamectl set-hostname ubuntu-20-04-amd64-dev-01
   ```
   and manually update the hostname in IP to host mapping file:
   ```bash
   $ sudo vim /etc/hosts
   ```
1. Run command `$ ip a` to verify all network adapter's IP addresses are in order.
1. If IP addresses look good, set static IP by following below instruction:  
   https://github.com/shiouming/technotes/blob/main/linux/configure-network-interface.md#ubuntu
3. Add new port forwarding rule in `NAT Network` setting on host, pointing to the new VM's IP address (for the adapter connected to `NAT Network`).
