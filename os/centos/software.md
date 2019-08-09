

- Checking your CentOS Version
```
cat /etc/centos-release
```

- Opera
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

- Node  
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

- Access iOS  
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


- VLC Media Player
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

- GNOME Desktop
```
yum grouplist
yum groups install "GNOME Desktop"
or
yum groupinstall "GNOME Desktop"
```

- SSH
```
yum install openssh-clients
```

- Enable full screen mode on Linux CentOS 7 VM
```
$sudo grubby --update-kernel=ALL --args="video=hyperv_fb:2560x1440"
$sudo reboot
```

- install all necessary multimedia plugins and codes from nux-dextop repository
```

```
