## Links
http://mesos.apache.org/documentation/latest/  
https://docs.mesosphere.com/  
https://dcos.io/docs/1.8/

https://github.com/everpeace/vagrant-mesos

## Components
### The master
- At any point Mesos has only one active master (elected using ZooKeeper)
- Offers slave resources to frameworks in the form of resource offers and launch tasks on slaves for accepting offers
- Responsible for all the communication between the tasks and frameworks


### Slaves
- Manage various resources such as CPU, memory, ports, etc.
- Execute tasks submitted by frameworks
- **Attributes**  
  - Expressed as key-value pairs with support for three different value types—`scalar`, `range`, and `text`—that are sent along with the offers to frameworks
- **Resources**  
  - Mesos can manage three different types of resources: `scalars`, `ranges`, and `sets` that a Mesos slave has to offer
  - The Mesos master predefines how it handles the following list of resources
    - cpus
    - mem
    - disk
    - ports
  - Examples:  
    `resources='cpus:24;mem:24576;disk:409600;ports:[21000-24000];bugs:{a,b,c}'`  
    `attributes='rack:abc;zone:west;os:centos5;level:10;keys:[1000-1500]'`


### Frameworks
- Each framework consists of a scheduler and executor
- **Scheduler**:
  - Decide whether to accept or reject the resource offers
  - Track the current state of the cluster
  - Communication with the Mesos master is handled by the `SchedulerDriver` module
- **Executor**:
  - Resource consumer run on slaves, responsible for running tasks
  - Communication with the slaves is handled by the `ExecutorDriver` module, which is also responsible for sending status updates to the scheduler
- Mesos API  
Allows programmers to develop their own custom frameworks that can run on top of Mesos
- Frameworks built on Mesos  
http://mesos.apache.org/documentation/latest/frameworks/


## Mesos DNS
http://mesosphere.github.io/mesos-dns/


## Installation
### Mac OS
- Install [Homebrew](http://brew.sh)  
`$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" < /dev/null 2> /dev/null`
- Install Mesos  
`$ brew edit mesos (optional)`  
`$ brew install mesos`  
`$ brew info mesos`
