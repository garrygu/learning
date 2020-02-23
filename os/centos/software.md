
## Repositories
### [IUS](https://ius.io/)
> IUS is a yum repository that provides newer versions of select software for RHEL and CentOS.

- CentOS 7
```
yum install \
https://repo.ius.io/ius-release-el7.rpm \
https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
```

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


## Software

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


### Opera
```
wget http://get.geo.opera.com/pub/opera/linux/1216/opera-12.16-1860.x86_64.rpm
rpm -ivh opera-12.16-1860.x86_64.rpm
opera
```

- Extract a tar.xz file on CentOS
```
yum -y install xz
unxz filename.tar.xz
tar -xf filename.tar
```

### Node  
https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-a-centos-7-server  
```
sudo yum install epel-release
sudo yum install nodejs
node --version
```
nvm  
https://github.com/creationix/nvm  
```
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.4/install.sh | bash
nvm install node
```  

### Access iOS  
[libimobiledevice](https://github.com/libimobiledevice/libimobiledevice)  
Install libusbmuxd:  
```
export PKG_CONFIG_PATH=/usr/lib64:$PKG_CONFIG_PATH
yum install libusbmuxd
```
[ifuse](https://github.com/libimobiledevice/ifuse)
```
# mount
ifuse <mountpoint>
# unmount
fusermount -u <mountpoint>
```


### VLC Media Player
  - install Epel and Nux Dextop repository
  ```
  # rpm -Uvh https://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-8.noarch.rpm
  # rpm -Uvh http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-5.el7.nux.noarch.rpm
  ```
  - Check the Availability of VLC & Install & Start
  ```
  # yum info vlc
  # yum install vlc
  # vlc
  ```

### GNOME Desktop
```
yum grouplist
yum groups install "GNOME Desktop"
or
yum groupinstall "GNOME Desktop"
```

### SSH
```
yum install openssh-clients
```

### Enable full screen mode on Linux CentOS 7 VM
```
$sudo grubby --update-kernel=ALL --args="video=hyperv_fb:2560x1440"
$sudo reboot
```

### Install all necessary multimedia plugins and codes from nux-dextop repository
```

```

### Installing Git on CentOS 7
- $sudo nano /etc/yum.repos.d/wandisco-git.repo  
```
[wandisco-git]
name=Wandisco GIT Repository
baseurl=http://opensource.wandisco.com/centos/7/git/$basearch/
enabled=1
gpgcheck=1
gpgkey=http://opensource.wandisco.com/RPM-GPG-KEY-WANdisco
```
- $sudo rpm --import http://opensource.wandisco.com/RPM-GPG-KEY-WANdisco
- $sudo yum install git
