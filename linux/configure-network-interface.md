# Configure network interface

Creation date: 2008-5-22

Tags: *linux, fedora, redhat*

---

## Fedora

Network interface config file is located at:
```
/etc/sysconfig/network-scripts/ifcfg-<eth0>
```

*\<eth0\>* has to be substituted with corresponding network interface name.

Below is an example of configuration file, *ifcfg-eth1*, that sets NIC *eth1* to use a static IP address permanently:
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
