
### chkconfig  
Updates and queries runlevel information for system services.  

chkconfig provides a simple command-line tool for maintaining the /etc/rc[0-6].d directory hierarchy by relieving system administrators of the task of directly manipulating the numerous symbolic links in those directories.
```
chkconfig [--list] [--type type][name]
chkconfig --add name
chkconfig --del name
chkconfig --override name
chkconfig [--level levels] [--type type] name <on|off|reset|resetpriorities>
chkconfig [--level levels] [--type type] name
```

Ref:
- https://linux.die.net/man/8/chkconfig
