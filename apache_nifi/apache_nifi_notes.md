NiFi was built to automate the flow of data between systems.

> Flows are made up of a series of processors each with a specific task.  

> Flows can be saved as templates and templates can be combined into more complex flows and quickly replicated across multiple servers again and again with minimal effort.

**Advantages of NiFi**
- Data source and destination-agnostic
- provides connection processors for many data sources - new ones can be developed quickly
- Run on any devices that run Java
- Build in one place, copy to anywhere else
- Good for places with limited connectivity
- Built in message prioritization


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
