# Issue: All VM Clones from Ubuntu 18.10 and Newer Versions Having Same IP Addrss

## Cause
From version 18.10 onwards, Ubuntu uses machine-id instead of MAC address in DHCP request. All clones from the same template will have the same machine-id, so they will all be assigned with the same IP address, even though they have different MAC address.

## Workaround 1
As last step of VM template creation, delete the machine-id, shut the VM down. When we create new VM out of this template and boot it up, since machine-id was deleted, a new one will be generated.
```bash
$ sudo rm /etc/machine-id /var/lib/dbus/machine-id
$ sudo touch /etc/machine-id; sudo chmod 444 /etc/machine-id
$ sudo shutdown -h now
```

## Workaround 2
Configure networkd to use MAC address instead in DHCP request. [**NOT TESTED**]
```bash
$ sudo vim /etc/systemd/network/default.network

# then set the following config
ClientIdentifier=mac
```


## References

- https://superuser.com/questions/1470219/virtualbox-dhcp-server-is-leasing-the-same-ip-to-all-guest-vms-connected-to-a-v
- https://kb.vmware.com/s/article/82229
