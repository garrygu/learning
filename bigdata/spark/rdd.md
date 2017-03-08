
Each RDD is a small piece of metadata describing a step of the computation:
- Parent(s)
- Partitions
- Compute
- (Partitioner)
- (Locality Preference)

sc
```
  sc.parallelize
  sc.runJob
```

## Caching RDDs
- Options for serialization, local disk cache, replication, etc.
- Caching, like computation, is always at the granularity of partitions!

- RDD caching defaults to MEMORY_ONLY  
i.e., myRDD.cache is the same as myRDD.persist(MEMORY_ONLY)  
http://spark.apache.org/docs/latest/programming-guide.html#rdd-persistence
- DataFrame/Dataset default .cache as persist(MEMORY_AND_DISK) because the compressed encoded columnar cache is expensive to rebuild
- Some streaming sources default to stronger caching (e.g., MEMORY_AND_DISK_SER_2)

- rdd.unpersist() or dataset.unpersist() -- immediately remove from cache


## Job Execution In-Depth
- Application
- Job
- Stage
- Task


## Shuffle
### Sort Shuffle

$sql SET 
