
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

- boot log
`cat /var/log/boot | more`


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
