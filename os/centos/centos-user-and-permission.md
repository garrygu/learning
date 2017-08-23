## Groups
There are two types of groups under Linux operating systems:
- Primary user group.
- Secondary or supplementary user group.

## User account related files
- /etc/passwd  
Contains one line for each user account.
- /etc/shadow  
Contains the password information in encrypted format for the system’s accounts and optional account aging information.
- /etc/group  
Defines the groups on the system.
- /etc/default/useradd  
This file contains a value for the default group, if none is specified by the useradd command.
- /etc/login.defs  
This file defines the site-specific configuration for the shadow password suite stored in /etc/shadow file.

## Add a new user to secondary group
Syntax:  
`useradd -G {group-name} username`  
Capital G (-G) option add user to a list of supplementary groups;  

- Add group  
`groupadd developers`

- Verify group  
`grep developers /etc/group`

- Add a new user to group  
`useradd -G developers testuser`  (but will remove old groups not in the list)
or:  
`gpasswd -a testuser developers`

- Verify user  
`grep ^testuser /etc/passwd`

- Set password for user  
`passwd testuser`

- Ensure user added to group
`id testuser`  
`groups testuser`


## Add a new user to primary group
Small g (-g) option add user to initial login group (primary group).   
`useradd -g developers testuser2`


## Add a existing user to existing group
`usermod -a -G developers <username>`  
Add the user to sudo group:  
`usermod -aG wheel <username>`  

**Options**:  
`-a --append`	 
Add the user to the supplementary group(s). Use only with the -G option.  
`-g GROUP  --gid GROUP`	  
Use this GROUP as the default group.  
`-G GRP1,GRP2  --groups GRP1,GRP2`
Add the user to GRP1,GRP2 secondary group.  

## Change group ownership
Syntx:
`chgrp [OPTION]... GROUP FILE...`   

Examples:  
`chgrp staff /u`  
Change the group of /u to "staff"

`chgrp -hR staff /u`  
Change the group of /u and subfiles to "staff"  


## Disable SELinux on CentOS 7
- See how SELinux is configured  
`sestatus`

- Open selinux configuration file
`nano /etc/sysconfig/selinux`  
Change `SELINUX=enforcing` to `SELINUX=disabled`  

- Rebooting your linux system to take effect
`reboot`

- Or, Permissive mode
`setenforce 0`



## Access Control
http://www.cis.rit.edu/class/simg211/unixintro/Access_Permissions.html  
Note: a directory must have both r and x permissions if the files it contains are to be accessed.  

File access permissions can also be changed by a numerical (octal) chmod specification. Read permission is given the value 4, write permission the value 2 and execute permission 1.

     r  w  x
     4  2  1

These values are added together for any one user category:

    0   =   no permissions
    1   =   execute only
    2   =   write only
    3   =   write and execute (1+2)
    4   =   read only
    5   =   read and execute (4+1)
    6   =   read and write (4+2)
    7   =   read and write and execute (4+2+1)

For example:
664: rw-rw-r
775: rwx-rwx-rw
