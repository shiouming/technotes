# How to Shrink VirtualBox Virtual Disk File (VDI)

At high level, it requires two major steps:
1. From guest: defrag and zero fill unutilised space of virtual disk.
2. From host: use VirtualBox command tool to shrink the VDI file to actual utilised disk size.

My environment:
- Guest: Ubuntu Server 20.04
- Host: Mac OS 10.14.6 Mojave
- VirtualBox: 6.1.34

Table of contents:
1. [Guest VM: Defrag and Zero Fill Virtual Disk](#openssh-server)
   - [Ubuntu Server Guest](#ubuntu-server-guest)
2. [Host: Shrink Virtual Disk VDI File](#ssh-over-nat-network)
   - [Mac OS Host](#mac-os-host)


## Guest VM: Defrag and Zero Fill Virtual Disk

### Ubuntu Server Guest

1. Start the guest VM.
1. Log in through the VM main window, then reboot with command `$ sudo shutdown -h now` and perform next step quickly.
1. While the VM is booting up, press `ESC` key to access **GRUB** boot menu.  
   If missed the timing to access **GRUB** menu, retry by rebooting again.
1. In GRUB boot menu, choose `Advanced options for Ubuntu`.
1. Choose to boot with kernel under `recovery mode`. If multiple kernel versions available in menu, choose the latest version.
1. Choose to access `root shell prompt`. Then press `ENTER` key for maintenance.
1. On root shell, run `df -hT` command to list the disks.
1. Note down the disk name and type of the interested drive:
   In this example, the 15GB VDI consists of:
   - `/` drive mounted from `ext4` disk `/dev/mapper/ubuntu--vg-ubuntu--lv`.
   - `/boot` drive mounted from `ext4` disk `/dev/sda2`.
1. We need to use `zerofree` utility to zero fill unutilised disk space. Install it if not available:
   ```bash
   $ sudo apt list --installed | grep zerofree
   
   # install it if not on our system.
   sudo apt-get install zerofree
   ```
1. Run below piped commands. Substitute the disk name and type with what we noted down in step #8.
   ```bash
   $ systemctl stop systemd-journal* && sudo swapoff -a && mount -n -o remount,ro -t ext4 /dev/mapper/ubuntu--vg-ubuntu--lv / && zerofree -v /dev/mapper/ubuntu--vg-ubuntu--lv
   $ systemctl stop systemd-journal* && sudo swapoff -a && mount -n -o remount,ro -t ext4 /dev/sda2 /boot && zerofree -v /dev/sda2
   ```
   Some of these commands must be run quickly right after the previous one, thus it is easier to chain them together.
1. Run `$ halt` command to shut the VM down. When the screen prints `System halted`, close the VM window, then choose `Power off the machine` when prompted with options.


## Host: Shrink Virtual Disk VDI File

### Mac OS Host
