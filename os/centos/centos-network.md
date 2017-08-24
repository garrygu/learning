## Setup
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


## Tools
- CanYouSeeMe.org
