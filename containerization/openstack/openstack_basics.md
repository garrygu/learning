## Nova
http://docs.openstack.org/developer/nova/
> Nova is an OpenStack project designed to provide power massively scalable, on demand, self service access to compute resources.

Runs on the controller node.
- controller
- network
- compute
- block
- object


Typical Controller Components:
- Horizon - Dashboard
- Neutron - Network
- Cinder - Block storage
- Keystone - Identity
- Glance - Image Service
- Nova - Compute


## Glance
Image is stored in one of the supported backends:
- local FS
- Rados
- Swift
- S3


## Cinder
- cinder-api
- cinder-volume
- cinder-scheduler


## Horizon
BUI interface for OpenStack management


## Object Storage
- adds another layer to the storage model
- Is based on binary objects


### Swift
- Allowing turning arbitrary disks into one storage device

### Ceph
- The trend is towards Ceph based storage
- More than an object store


## Networking
- Controlled by the cloud
- Software Defined Networking (SDN)

### Neutron
https://github.com/yeasy/openstack_understand_Neutron
https://yeasy.gitbooks.io/openstack_understand_neutron/content/

- Originally known as Quantum
- Nodes are connected to a switch
- Architecture
  - neutron-server
  - neutron plugins
  - neutron agents
- Typical Different Physical networks
  - Management
  - Data
  - External
  - API


## Ceilometer

## Orchestration- Heat

## Database- Trove
- Provides Database as a Service (DBaaS)

## Oslo
Provide functionality required by moren than one OpenStack components
  - Talk to an AMQP queue
  - connect to a database
  - provide a CLI client


## Installation
### Packstack
> Packstack is a utility that uses Puppet modules to deploy various parts of OpenStack on multiple pre-installed servers over SSH automatically. Currently only Fedora , Red Hat Enterprise Linux (RHEL) and compatible derivatives of both are supported.
