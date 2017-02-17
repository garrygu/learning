
## bin/kafka-console-consumer.sh
### kafka-2.9.2

```
Option                                  Description                            
------                                  -----------                            
--autocommit.interval.ms <Integer: ms>  The time interval at which to save the
                                          current offset in ms (default: 60000)
--blacklist <blacklist>                 Blacklist of topics to exclude from    
                                          consumption.                         
--consumer-timeout-ms <Integer: prop>   consumer throws timeout exception      
                                          after waiting this much of time      
                                          without incoming messages (default:  
                                          -1)                                  
--csv-reporter-enabled                  If set, the CSV metrics reporter will  
                                          be enabled                           
--fetch-size <Integer: size>            The amount of data to fetch in a       
                                          single request. (default: 1048576)   
--formatter <class>                     The name of a class to use for         
                                          formatting kafka messages for        
                                          display. (default: kafka.consumer.   
                                          DefaultMessageFormatter)             
--from-beginning                        If the consumer does not already have  
                                          an established offset to consume     
                                          from, start with the earliest        
                                          message present in the log rather    
                                          than the latest message.             
--group <gid>                           The group id to consume on. (default:  
                                          console-consumer-35154)              
--max-messages <Integer: num_messages>  The maximum number of messages to      
                                          consume before exiting. If not set,  
                                          consumption is continual.            
--max-wait-ms <Integer: ms>             The max amount of time each fetch      
                                          request waits. (default: 100)        
--metrics-dir <metrics dictory>         If csv-reporter-enable is set, and     
                                          this parameter isset, the csv        
                                          metrics will be outputed here        
--min-fetch-bytes <Integer: bytes>      The min number of bytes each fetch     
                                          request waits for. (default: 1)      
--property <prop>                                                              
--refresh-leader-backoff-ms <Integer:   Backoff time before refreshing         
  ms>                                     metadata (default: 200)              
--skip-message-on-error                 If there is an error when processing a
                                          message, skip it instead of halt.    
--socket-buffer-size <Integer: size>    The size of the tcp RECV size.         
                                          (default: 2097152)                   
--socket-timeout-ms <Integer: ms>       The socket timeout used for the        
                                          connection to the broker (default:   
                                          30000)                               
--topic <topic>                         The topic id to consume on.            
--whitelist <whitelist>                 Whitelist of topics to include for     
                                          consumption.                         
--zookeeper <urls>                      REQUIRED: The connection string for    
                                          the zookeeper connection in the form
                                          host:port. Multiple URLS can be      
                                          given to allow fail-over.            
```
