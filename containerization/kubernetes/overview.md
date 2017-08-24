http://kubernetes.io/


## Core constructs
### Pods
Pods essentially allow you to logically group containers and pieces of our application stacks together.

### Labels
Similar to tags, labels can be used as the basis of service discovery as well as a useful grouping tool for day-to-day operations and management tasks.  

Labels are just simple key-value pairs.

### Services
Every node in the cluster runs a proxy named **kube-proxy**.  

kube-proxy's job is to proxy communication from a service endpoint back to the corresponding pod that is running the actual application.

### Replication controllers
Replication controllers (RCs), as the name suggests, manage the number of nodes that a pod and included container images run on. They ensure that an instance of an image is being run with the specific number of copies.

Just like services, we will use selectors and labels to define a pod's membership in a replication controller.


## Kubernetes application
### Kubernetes resource definitions
- kind
- apiVersion
- metadata
- spec



## Kubernetes UI
`https://<your master ip>/api/v1/proxy/namespaces/kube-system/services/kube-ui`

`$ kubectl config view`

## Grafana
`https://<your master ip>/api/v1/proxy/namespaces/kube-system/services/monitoring-grafana`

## Swagger
`https://<your master ip>/swagger-ui/`  
`https://<your master ip>/api/v1/nodes/`  
`https://<your master ip>/api/v1/nodes/?pretty=false`
