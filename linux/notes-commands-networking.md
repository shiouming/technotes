# Quick notes - networking commands

Creation date: 2008-5-27

Tags: *bash, linux, fedora, redhat*

---


**ifconfig**

To get IP address of network interface, or to configure network interface, such as setting the IP address to a static one.

**Note**: IP address assigned with this command will be gone once reboot. To set a static IP address permanently, you will need to [edit the interface’s configuration file](configure-network-interface.md).

```
ifconfig
ifconfig up
ifconfig down
ifconfig <ip-address>
```

**ss**

```
# Check processes that listening to network ports (Ubuntu only?)
sudo ss -ltnp
```

**netstat**

```
# Display summary statistics for each protocol
netstat -s

# Display IP routing table.
netstat -r

# Display the selected information every second continuously,
# usually used in combination with other options.
netstat -c

# Display the selected information with PID and program name,
# usually used in combination with other options.
netstat -p

# Show listening sockets only
netstat -l

# Show both listening and non-listening sockets
netstat -a

# Display additional details
netstat -e

# Check the port a particular process is listening to
netstat -pnl | grep <process-name>
```

**nslookup**

To query Internet domain name servers
```
nslookup <server-name>
```

**ping**

To test if an address is reachable by sending an ICMP *ECHO_REQUEST* datagram to the specified network host. Take note that this will not work if destination has been configured to block the echo request. This command can be use to check packet loss and jitter error as well.
```
ping <destination-host>
traceroute
```

List the nodes along the route to specified network host.
```
traceroute <destination-host>
```

**scp**

Copy file to a directory in remote machine. Once executed this command a password prompt will be display.
```
scp [-i <key-file-path>] -C <src-file-path> <remote-user>@<remote-address>:<remote-destination-path>
```
