# KVM
Ref： https://www.cyberciti.biz/faq/how-to-install-kvm-on-centos-7-rhel-7-headless-server/

- Test if CPU Support Intel VT and AMD-V Virtualization tech:  
`$ lscpu | grep Virtualization`    
ref: https://www.cyberciti.biz/faq/linux-xen-vmware-kvm-intel-vt-amd-v-support/  
or：  
`egrep '(vmx|svm)' /proc/cpuinfo`  
If the above command returns with output showing VMX or SVM, then your hardware supports VT else it does not.  
ref: https://www.itzgeek.com/how-tos/linux/centos-how-tos/install-kvm-qemu-on-centos-7-rhel-7.html

## Step 1: Install kvm
- Install kvm  
`# yum install qemu-kvm libvirt libvirt-python libguestfs-tools virt-install`  
or  
`yum install -y qemu-kvm qemu-img virt-manager libvirt libvirt-python libvirt-client virt-install virt-viewer`  
qemu-kvm =  QEMU emulator  
qemu-img = QEMU disk image manager  
virt-install =  Command line tool to create virtual machines.  
libvirt = Provides libvirtd daemon that manages virtual machines and controls hypervisor.  
libvirt-client  = provides client-side API for accessing servers and also provides the virsh utility which provides command line tool to manage virtual machines.  
virt-viewer – Graphical console  

- Start the libvirtd service  
`# systemctl enable libvirtd`  
`# systemctl start libvirtd`

## Step 2: Verify kvm installation
- Verify kvm installation
`# lsmod | grep -i kvm`


## Step 3: Configure bridged networking
- Configure bridged networking  
`# brctl show`  
`# virsh net-list`

-  All VMs (guest machine) only have network access to other VMs on the same server. A private network 192.168.122.0/24 created for you. Verify it:  
`# virsh net-dumpxml default`

- If you want your VMs avilable to other servers on your LAN, setup a a network bridge on the server that connected to the your LAN. Update your nic config file such as ifcfg-enp3s0 or em1:  
`# vi /etc/sysconfig/network-scripts/enp3s0`  
Add line:  
`BRIDGE=br0`

- Edit /etc/sysconfig/network-scripts/ifcfg-br0 and add:  
`# vi /etc/sysconfig/network-scripts/ifcfg-br0`  
Add the following：
```
DEVICE="br0"
# I am getting ip from DHCP server #
BOOTPROTO="dhcp"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
ONBOOT="yes"
TYPE="Bridge"
DELAY="0"
```

- Restart the networking service (warning ssh command will disconnect, it is better to reboot the box):  
`# systemctl restart NetworkManager`

- Verify it with brctl command:
`# brctl show`

## Step 4: Create your first virtual machine
```
# cd /var/lib/libvirt/boot/
# wget https://mirrors.kernel.org/centos/7.4.1708/isos/x86_64/CentOS-7-x86_64-Minimal-1708.iso
```
Creating CentOS 7.x VM with 2GB RAM, 2 CPU core, 1 nics and 40GB disk space, enter:  
```  
# virt-install \
  --virt-type=kvm \
  --name centos7 \
  --ram 2048 \
  --vcpus=1 \
  --os-variant=centos7.0 \
  --cdrom=/var/lib/libvirt/boot/CentOS-7-x86_64-Minimal-1708.iso \
  --network=bridge=br0,model=virtio \
  --graphics vnc \
  --disk path=/var/lib/libvirt/images/centos7.qcow2,size=40,bus=virtio,format=qcow2
```
To configure vnc login from another terminal over ssh and type:  
`# virsh dumpxml centos7 | grep vnc`  

Type the following SSH port forwarding command from your client/desktop:  
`$ ssh vivek@server1.cyberciti.biz -L 5904:127.0.0.1:5904`

## Useful commands
- List all VMs  
`# virsh list --all`

- Get VM info  
`# virsh dominfo vmName`  
`# virsh dominfo centos7-vm1`

- Stop/shutdown a VM  
`# virsh shutdown centos7-vm1`

- Start VM  
`# virsh start centos7-vm1`

- Mark VM for autostart at boot time  
`# virsh autostart centos7-vm1`

- Reboot (soft & safe reboot) VM  
`# virsh reboot centos7-vm1`

- Reset (hard reset/not safe) VM  
`# virsh reset centos7-vm1`

- Delete VM  
```
# virsh shutdown centos7-vm1
# virsh undefine centos7-vm1
# virsh pool-destroy centos7-vm1
# D=/var/lib/libvirt/images
# VM=centos7-vm1
# rm -ri $D/$VM
```

- To see a complete list of virsh command type
`# virsh help | less`
`# virsh help | grep reboot`

# VirtualBox
https://wiki.centos.org/HowTos/Virtualization/VirtualBox

## Installing VirtualBox
- Download the RHEL repo config.  
`cd /etc/yum.repos.d`  
`wget http://download.virtualbox.org/virtualbox/rpm/rhel/virtualbox.repo`

- Installing DKMS from the EPEL repository is recommended before installing VirtualBox.   
`yum --enablerepo=epel install dkms`

- Install the RPM:
`yum install VirtualBox-5.2'

- The installer will create the "vboxusers" group. For each "username" that will run VirtualBox:  
`usermod -a -G vboxusers username`

## Running VirtualBox
`VirtualBox &"`

## Making USB Work in VirtualBox
- Install VirtualBox Extension Pack
https://www.nakivo.com/blog/how-to-install-virtualbox-extension-pack/
