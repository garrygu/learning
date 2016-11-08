# Apache ZooKeeper

## Start/stop zookeeper
```
bin/zkServer.sh {start|start-foreground|stop|restart|status|upgrade|print-cmd}
```


## Connecting to ZooKeeper  
```
bin/zkCli.sh -server 127.0.0.1:2181
```


## CLI
- cmd args
```
ZooKeeper -server host:port cmd args
	stat path [watch]
	set path data [version]
	ls path [watch]
	delquota [-n|-b] path
	ls2 path [watch]
	setAcl path acl
	setquota -n|-b val path
	history
	redo cmdno
	printwatches on|off
	delete path [version]
	sync path
	listquota path
	rmr path
	get path [watch]
	create [-s] [-e] path data acl
	addauth scheme auth
	quit
	getAcl path
	close
	connect host:port
```

- create/delete a new znode  
```
create /zk_test my_data  
delete /zk_test
```
- get znode  
```
> get /zk_test
>
my_data
cZxid = 0x7
ctime = Thu Oct 13 11:19:29 PDT 2016
mZxid = 0x7
mtime = Thu Oct 13 11:19:29 PDT 2016
pZxid = 0x7
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 7
numChildren = 0
```

- change node
```
set /zk_test junk
```

## conf/zoo.cfg
```
# The number of milliseconds of each tick
tickTime=2000

# The number of ticks that the initial synchronization phase can take
initLimit=10

# The number of ticks that can pass between sending a request and getting an acknowledgement
syncLimit=5

# the directory where the snapshot is stored.
# do not use /tmp for storage, /tmp here is just example sakes.
dataDir=/tmp/zookeeper

# the port at which the clients will connect
clientPort=2181

# the maximum number of client connections.
# increase this if you need to handle more clients
#maxClientCnxns=60

# Be sure to read the maintenance section of the administrator guide before turning on autopurge.
#
# http://zookeeper.apache.org/doc/current/zookeeperAdmin.html#sc_maintenance
#
# The number of snapshots to retain in dataDir
#autopurge.snapRetainCount=3

# Purge task interval in hours
# Set to "0" to disable auto purge feature
#autopurge.purgeInterval=1

```

## Running Replicated ZooKeeper
```
# conf/zoo.cfg
tickTime=2000
dataDir=/var/lib/zookeeper
clientPort=2181
initLimit=5
syncLimit=2
server.1=zoo1:2888:3888
server.2=zoo2:2888:3888
server.3=zoo3:2888:3888
```
