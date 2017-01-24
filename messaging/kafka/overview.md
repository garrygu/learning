## Resources
- https://kafka.apache.org/
- https://kafka-summit.org/kafka-summit-2016/schedule/
- https://groups.google.com/forum/#!forum/confluent-platform


## Terminology
- message
  - A message is a key-value pair
- topic
  - Each message belongs to a topic  
  - Topics provide a way to group messages together  
  - There is no limit to the number of topics that can be used  
  - By default, a topic is automatically created the first time a Producer writes a message to it  
  - Topics are partitioned for parallelism


## Architecture Features
### Partition
- Topics are split into Partitions by Kafka  
- Producers share data over a set of partitions  
hash(key) % n by default
- If no key is specified, messages are sent to Partitions on a round-robin basis
- Data is stored in a Persistent Log  


### Fault Tolerance via Replicated Log




### Distributed Consumption
- Each consumer is assigned a subset of partitions
- Partitions are stored as separate logs
- The Result: A Linearly Scalable Firehose


### Zero-Copy Data Transfer


##  Intra-Cluster Replication
### Roles
#### Leader
#### Replicas/Followers
#### Controller
- One per cluster

#### ZooKeeper

###  Data Flow in Replication
### In Sync Replicas
- ISRs are replicas which are up-to-date with the leader
- ISR list: leader maintains this list
- Leader detects that a follower is slow and removes from the ISR  
`replica.lag.time.max.ms`
- Leader failure is managed by ZK/Controller  
`zookeeper.session.timeout.ms`
- How many brokers can we lose  
`min.insync.replicas`
- Committed  
Data is received by all the replicas in the ISR  
Data can not be seen (fetched) until it is committed
- Replica configurations
`default.replication.factor`: default 1


## Kafka Components
### Kafka Producer
### Kafka Consumer
- The Consumer Offset keeps track of the latest message Replicated  
- If necessary, the Consumer Offset can be changed
- The Consumer Offset is stored in a special Kafka topic (Kafka 0.9+)
#### Consumer groups
- Consumers in the group are typically on separate machines
- Each Consumer will read from one or more partitions for a given topic
- Data from a Partition will go to a single Consumer in the group
#### Consumer group coordinator
### Kafka Broker


##  Log Retention and Compaction
### Log Retention Policy
- `log.cleanup.policy`  
  - delete (default): old segments are deleted
  - compact: old values for the key are deleted
- Log compaction uses:  
  - database change capture
  - stateful stream processing
  - event sourcing
- `log.cleaner.min.cleanable.ratio`
- `log.cleaner.io.max.bytes.per.second`
- Deletes
  - Delete tombstone modelled as a null message
  - Danger of removing a deleted key too soon
  - `log.cleaner.delete.retention.ms`  
  Default 1 day
- JMX Metrics


## Hardware and Runtime Configurations
### Topic-level Configuration Parameters

### Controlled Shutdown
- `controlled.shutdown.enabled`
- `controlled.shutdown.max.retries`
- `controlled.shutdown.retry.backoff.ms`


## Kafka Security


## Kafka Connect
### Distributed Mode
- Components:
  - connectors
  - tasks
  - workers


## The Page Cache for High Performance
- Kafka makes use of the operation system's page cache to hold recently-used data
