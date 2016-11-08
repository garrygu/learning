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
