## Network Setup
- `nmcli d`  
output:
```
DEVICE      TYPE      STATE         CONNECTION
virbr0      bridge    connected     virbr0     
eno1        ethernet  disconnected  --         
wlp0s20u4   wifi      unavailable   --         
lo          loopback  unmanaged     --         
virbr0-nic  tun       unmanaged     --   
```

- `nmtui`  
- `ip a`

- Activate network after install:
Edit `/etc/sysconfig/network-scripts/ifcfg-eth0`:
`ONBOOT =yes`  
`/etc/init.d/network restart`  or `systemctl restart network`


- Verify the status of Network Manager service  
`systemctl status NetworkManager.service`


- Check which network interface is managed by Network Manager
`nmcli dev status`


## Web Proxy
```
# vi /etc/environment
export http_proxy=https://user:pwd@proxy_address/
export https_proxy=https://user:pwd@proxy_address/
export ftp_proxy=https://user:pwd@proxy_address/
export no_proxy=".mylan.local,.domain1.com,host1,host2"
```
```
# vi /etc/yum.conf
proxy=http://proxysrv:8080/
proxy_username=***
proxy_password=***
```
