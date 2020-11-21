# Quick notes - system administration commands

Creation date: 2008-6-12

Tags: *bash, linux, fedora, redhat*

---


To display or to set the host name for current session.
```
hostname <desired-host-name>

# To change hostname permanently:
# - modify 'hostname' variable in /etc/sysconfig/network,
# - edit/add entry for the selected host name in /etc/hosts.
```


To get system date and time in specific format, or to set system's date and time.
```
date
date "+%Y-%m-%d %T"
date -s "yyyymmdd hh:MM:ss"
```


To synchronise system date and time.
```
# recommended to perform an initial full sync (when machine not in production time):
ntpdate
# then edit /etc/ntp.conf to add NTP server, and run the daemon
service ntpd restart
```


To list running processes with details, or to list the running processes with their resource usage.
```
ps -ef
top  # press Ctrl+C to exit from 'top' command
```


To run program at a lower system priority, or to allow your program running even you are logged off.
```
nice [-n increment] <program> [argument ...]

<program> [argument ...]&
# if putting '&' at the end of command does not work, try 'nohup' instead.
nohup <program> [argument ...]
```


To start/stop a particular service, or to check the service status.

by using **systemctl**:
```
systemctl start <srvice-name>
systemctl stop <srvice-name>
systemctl restart <srvice-name>
systemctl reload <srvice-name>
systemctl status <srvice-name>
```

by using **service** (for older versions of Redhat/Centos/Fedora):
```
# The <service-name> is essentially init script stored in /etc/init.d/ written in Bash.
# All scripts should support at least 'start' and 'stop' command.
# If supported by the script, additional parameter can be supply after command. Refer to other init 
#   scripts as sample. Take note that there is no or minimum user environment only, thus the script might need to handle it manually.

service <service-name> start
service <service-name> stop
service <service-name> restart
service <service-name> reload
service <service-name> status
```


To list, edit, or remove cron jobs (within crontab) for the user.
```
crontab [-l | -e | -ri]
```
