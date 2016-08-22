NiFi was built to automate the flow of data between systems.


## Terms
**Flowfile**  
Made up of two parts:
- Attributes: key-value pairs associated with user data
- Content: user data  

**Processor**  
Most important building block that is responsible for creating, sending, receiving, transforming, routing, splitting, merging, and processing FlowFiles.


## NiFi Cluster
A NiFi cluster is comprised of one or more NiFi Nodes (Node) controlled by a single NiFi Cluster Manager (NCM).

- What will happen if the master dies  
The absence of the NCM simply means new nodes cannot join the cluster and cluster flow changes cannot occur until the NCM is restored.
