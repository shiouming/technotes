# VirtualBox Shared Folder between Ubuntu Guest VM and Mac OS Host

My environment:
- Guest: Ubuntu Server 20.04
- Host: Mac OS 10.14.6 Mojave
- VirtualBox: 6.1.34

Reference and Credit:
- [Setup guide for Ubuntu guest and Windows host](https://gist.github.com/estorgio/0c76e29c0439e683caca694f338d4003)


**Instructions**

1. In VirtualBox software, open the **Shared Folders** setting of the selected VM.
1. Add a new Shared Folder:
   - Folder Path: `/Users/<username>/workspace/vm_shared_dir/linux`  _# desired shared directory on host_
   - Folder Name: `vbox-host-shared` _# desired name_
   - Mount point: `/mnt/vbox-host-shared`
   - Auto-mount: checked
1. Start that VM.
1. With that VM window active, select menu `Devices > Insert Guest Additions CD Image`.
1. From VM's command line, mount the CD in read-only mode:
   ```bash
   $ sudo mkdir /media/cdrom
   $ sudo mount -t iso9660 /dev/cdrom /media/cdrom
   ```
1. Install `build-essential` package, needed by VirtualBox Guest Additions installation (kernel building step):
   ```bash
   $ sudo apt-get install -y build-essential
   ```
1. Install the VirtualBox Guest Additions:
   ```bash
   $ sudo /media/cdrom/VBoxLinuxAdditions.run
   ```
1. Reboot the VM:
   ```bash
   $ sudo shutdown -r now
   ```
1. By default, the shared directory `/mnt/vbox-host-shared/` specified in step #2 is accessible by user group `vboxsf`.  
   Let's give the guest VM user the access permission, by adding the user to group `vboxsf`:
   ```bash
   $ ll -d /mnt/vbox-host-shared
   drwxrwx--- 1 root vboxsf 96 May 13 08:36 /mnt/vbox-host-shared/
   
   # assume 'ubuntu' is the user
   $ sudo usermod -aG vboxsf ubuntu
   ```
1. Logout and re-login again. Verify if the change to user group association is effective:
   ```bash
   $ groups
   ubuntu adm cdrom sudo dip plugdev lxd vboxsf
   ```
1. Test: from VM, create a file in the shared directory, then open it on host.
   ```bash
   $ vim /mnt/vbox-host-shared/testfile.txt
   ```
   Do the same test in reverse direction, it should work.
