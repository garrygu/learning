
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
- Check hostname   
`hostname`  
`hostname -f`

- Update system  
`sudo yum clean all`  
`sudo yum -y update`

- Make backup of a file  
`cp /etc/httpd/conf/httpd.conf ~/httpd.conf.backup`

- Find all socket files on your system
`sudo find / -type s`


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
yum -y update
yum -y upgrade
```

### RPM
- Install a package  
`rpm -i package-1.2.3.rpm`

- Remove a package-1  
`rpm -e package-1.2.3.rpm`

- Find the full path to the file if not known, such as for an executable in $PATH  
`type -path awk`

- Find the name, including version, of the package containing the file  
`rpm -qf /usr/bin/awk`

- Query for info from that package  
`rpm -qi gawk`


### GUI Software Manager
#### yumex (epel repository)
```
yum --enablerepo=epel info yumex
yum --enablerepo=epel install yumex
```

### Installing Alien and Dependencies
https://www.tecmint.com/convert-from-rpm-to-deb-and-deb-to-rpm-package-using-alien/  
```
# yum install epel-release
# rpm --import http://li.nux.ro/download/nux/RPM-GPG-KEY-nux.ro
# rpm -Uvh http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-5.el7.nux.noarch.rpm
# yum update && yum install alien
```

#### Converting from .deb to .rpm Package
```
# alien --to-rpm --scripts dateutils_0.3.1-1.1_amd64.deb
```

- Try to install
```
# rpm -Uvh dateutils-0.3.1-2.1.x86_64.rpm 
```

- Solve Errors
```
# yum --enablerepo=epel-testing install rpmrebuild
# rpmrebuild -pe dateutils-0.3.1-2.1.x86_64.rpm
```
Go to the %files section and delete the lines that refer to the directories mentioned in the error message, then save the file and exit.   

- Install the package
```
# rpm -Uvh /root/rpmbuild/RPMS/x86_64/dateutils-0.3.1-2.1.x86_64.rpm
# rpm -qa | grep dateutils
```


### Install Google Chrome
https://www.tecmint.com/install-google-chrome-on-redhat-centos-fedora-linux/  
- Enable Google YUM repository  
Create a file called /etc/yum.repos.d/google-chrome.repo and add the following lines of code to it.  
```
[google-chrome]
name=google-chrome
baseurl=http://dl.google.com/linux/chrome/rpm/stable/$basearch
enabled=1
gpgcheck=1
gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub
```

- Installing Chrome Web Browser  
```
# yum info google-chrome-stable
# yum install google-chrome-stable
```

- Automatically download and install latest Google Chrome browser  
The Google Chrome browser no longer supports RHEL 6.x and its free clones such as CentOS and Scientific Linux.
```
# wget http://chrome.richardlloyd.org.uk/install_chrome.sh
# chmod u+x install_chrome.sh
# ./install_chrome.sh
```

- Starting Google Web browser
```
# google-chrome &
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


## Frequently Used Tools
### Text Editor
- Atom  
```
wget https://github.com/atom/atom/releases/download/v1.6.0/atom.x86_64.rpm
# Install dependency package
yum install lsb-core-noarch
sudo rpm -ivh atom.x86_64.rpm
```

- Nano
`nano /etc/httpd/conf/httpd.conf`


## Security
- sudo
```
su -
vi /etc/sudoers
# Add: username        ALL=(ALL)      ALL
```

### User and Group permissions
#### chmod  
`chmod -flags permissions /path/to/dir/or/file`

- permissions
```
  u = user
  g = group
  o = other  

  + will add permissions
  - will remove permissions

  r = read
  w = write
  x = execute
```
