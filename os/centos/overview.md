
## Frequently Used Commands
- Change user  
`su {username}`

- Change screen resolution [[source link]](http://www.netometer.com/blog/?p=1663)
```
# Use "grubby" to update all current and future kernels passing the video argument
# for the Microsoft Hyper-V Synthetic Video Frame Buffer driver
grubby --update-kernel=ALL --args="video=hyperv_fb:1024x768"
# A reboot is required to apply the changes:
reboot
```

## Repository
### EPEL
> EPEL (Extra Packages for Enterprise Linux) is open source and free community based repository project from Fedora team which provides 100% high quality add-on software packages for Linux distribution including RHEL (Red Hat Enterprise Linux), CentOS, and Scientific Linux. [[source link]](http://www.tecmint.com/how-to-enable-epel-repository-for-rhel-centos-6-5/)

**How to enable EPEL**
```
## RHEL/CentOS 7 64-Bit ##
# wget http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-9.noarch.rpm
# rpm -ivh epel-release-7-9.noarch.rpm

## verify repository
# yum repolist

## The epel configuration file is located under /etc/yum.repos.d/epel.repo
```

**Search and install packages using epel repo**  
```
yum --enablerepo=epel info zabbix
yum --enablerepo=epel install zabbix
```


## Package Manager
### YUM
```
yum check-update
yum update
yum repolist
yum search httpd
yum info httpd
```

### RPM
### GUI Software Manager
#### yumex (epel repository)
```
yum --enablerepo=epel info yumex
yum --enablerepo=epel install yumex
```


## Network
### Network Configuration Files
- /etc/hosts
- /etc/resolv.conf
- /etc/sysconfig/network
- /etc/sysconfig/network-scripts/ifcfg-<interface-name>

### Commands
- nmcli  
Control NetworkManager and report network status  
```
nmcli [OPTIONS...] {help | general | networking | radio | connection | device | agent | monitor} [COMMAND] [ARGS...]  
nmcli general {status | hostname | permissions | logging} [ARGS...]  
nmcli networking {on | off | connectivity} [ARGS...]  
nmcli radio {all | wifi | wwan} [ARGS]  
nmcli monitor  
nmcli connection {show | up | down | modify | add | clone | delete | monitor | reload | load | import | export} [ARGS]  
nmcli device {status | show | set | connect | reapply | modify | disconnect | delete | monitor | wifi | lldp} [ASGS...]
```

Examples:  
```
nmcli d
nmcli -t -f RUNNING general  
nmcli -t -f STATE general  
nmcli radio wifi off  
nmcli connection show  
nmcli -p -m multiline -f all con show  
nmcli connection show --active  
nmcmi -f name,autoconnect c s  
nmcli -p connection show "My default em1"  
nmcli device status
nmcli connection  add type ethernet autoconnect no ifname eth0
...
```

- service network restart
- systemctl
```
systemctl status network.service  
```
- journalctl
```
journalctl -xe
```
