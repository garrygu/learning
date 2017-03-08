NiFi was built to automate the flow of data between systems.

> There are two different ways that most developers are going to come at NiFi:  
> From a traditional software programming background with some data processing experience but mostly application focused development, or  
> From a data warehousing background where extract, transform, and load (ETL) tools are the norm.
>
> Unless they’re already facing Big Data scalability challenges, skeptics from the application development camp might see NiFi as a point-and-click waste of time where configuration files and dynamic code will be much more flexible.
> (https://dzone.com/articles/apache-nifi-not-from-scratch)   

## Use Cases
https://hortonworks.com/press-releases/hortonworks-dataflow-drives-the-internet-of-anything-for-the-enterprise/
- **Big Data Ingest**  
Offers a simple, reliable and secure way to collect data streams.
- **IoAT Optimization**  
Allows organizations to overcome real world constraints such as limited or expensive bandwidth while ensuring data quality and reliability.
- **Compliance**  
Enables organizations to understand everything that happens to data in motion from its creation to its final resting place, which is particularly important for regulated industries that must retain and report on chain of custody.
- **Digital Security**  
Helps organizations collect large volumes of data from many sources and prioritize which data is brought back for analysis first, a critical capability given the time sensitivity of identifying security breaches.


## Terms
**Flow-based programming**  
> Flow-based programming (FBP) is a programming paradigm that defines applications as networks of "black box" processes, which exchange data across predefined connections by message passing, where the connections are specified externally to the processes. (https://en.wikipedia.org/wiki/Flow-based_programming)

**Flowfile**  
Made up of two parts:
- Attributes: key-value pairs associated with user data
- Content: user data  

**Processor**  
Most important building block that is responsible for creating, sending, receiving, transforming, routing, splitting, merging, and processing FlowFiles.

## Flow Files
To work on flow files nifi provides 3 callback interfaces:
- `InputStreamCallback`: For reading the contents of the flow file through a input stream.
- `OutputStreamCallback`: For writing to a flowfile, this will over write not concatenate.
- `StreamCallback`: For both reading and writing to the same flow file.

## Processors
Ref: [Apache Nifi: What Processors Are There?](http://www.nifi.rocks/apache-nifi-processors/) (through release 0.7.0) (02/05/2015)


## NiFi Cluster
A NiFi cluster is comprised of one or more NiFi Nodes (Node) controlled by a single NiFi Cluster Manager (NCM).

- What will happen if the master dies  
The absence of the NCM simply means new nodes cannot join the cluster and cluster flow changes cannot occur until the NCM is restored.
