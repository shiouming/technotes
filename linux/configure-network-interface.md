# Configure network interface

Creation date: 2008-5-22

Tags: *linux, ubuntu, fedora, redhat*

---

## Ubuntu

Ubuntu >= 17.10 uses **_netplan_**

```
/etc/netplan/<yaml_config_file>
```

Ubuntu < 17.10 uses **_ifconfig_**

```
/etc/network/interfaces
```

### Netplan - setting static IP addresses

Create a custom Netplan config file `/etc/netplan/config.yaml`. Example:
```bash
$ sudo vim /etc/netplan/config.yaml

network:
    version: 2
    renderer: networkd
    ethernets:
        enp0s3:
            addresses:
                - 10.0.2.4/24
            dhcp4: true
        enp0s8:
            addresses:
                - 192.168.56.102/24
            dhcp4: true
```
The `enp0s3` and `enp0s8` are network interface name, can be obtained from command:
```bash
$ ip a
```

Test the new netplan config:
```bash
# While the "try" sub-command is counting down, we should check if the new config works per expectation, e.g. run the "ip" command through a separate SSH session to see the outcome. If it works fine, press ENTER key to accept the file change.
$ sudo netplan try

# Apply the config change.
$ sudo netplan apply

# Config change has been applied. Let's verify the final output again.
$ ip a
```

Reboot the machine, then verify the result:
1. Try to SSH into the machine through the static IP.
1. Run `ip a` command to check if IP addresses are still in order after reboot.


## Fedora

Network interface config file is located at:
```
/etc/sysconfig/network-scripts/ifcfg-<eth0>
```

`<eth0>` has to be substituted with corresponding network interface name.

Below is an example of configuration file, `ifcfg-eth1`, that sets NIC `eth1` to use a static IP address permanently:
```
# Advanced Micro Devices [AMD] 79c970 [PCnet32 LANCE]
DEVICE=eth1
IPADDR="192.168.19.134"
NETMASK="255.255.255.0"
BROADCAST="192.168.19.255"
ONBOOT=yes
BOOTPROTO=static
HWADDR=00:0c:29:25:f9:db
```
