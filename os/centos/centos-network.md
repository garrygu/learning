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


## Tools
- CanYouSeeMe.org


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
