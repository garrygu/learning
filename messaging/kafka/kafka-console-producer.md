## bin/kafka-console-producer.sh
### kafka-2.9.2

```
Option                                  Description                            
------                                  -----------                            
--batch-size <Integer: size>            Number of messages to send in a single
                                          batch if they are not being sent     
                                          synchronously. (default: 200)        
--broker-list <broker-list>             REQUIRED: The broker list string in    
                                          the form HOST1:PORT1,HOST2:PORT2.    
--compress                              If set, messages batches are sent      
                                          compressed                           
--key-serializer <encoder_class>        The class name of the message encoder  
                                          implementation to use for            
                                          serializing keys. (default: kafka.   
                                          serializer.StringEncoder)            
--line-reader <reader_class>            The class name of the class to use for
                                          reading lines from standard in. By   
                                          default each line is read as a       
                                          separate message. (default: kafka.   
                                          producer.                            
                                          ConsoleProducer$LineMessageReader)   
--message-send-max-retries <Integer>    Brokers can fail receiving the message
                                          for multiple reasons, and being      
                                          unavailable transiently is just one  
                                          of them. This property specifies the
                                          number of retires before the         
                                          producer give up and drop this       
                                          message. (default: 3)                
--property <prop>                       A mechanism to pass user-defined       
                                          properties in the form key=value to  
                                          the message reader. This allows      
                                          custom configuration for a user-     
                                          defined message reader.              
--queue-enqueuetimeout-ms <Long: queue  Timeout for event enqueue (default:    
  enqueuetimeout ms>                      2147483647)                          
--queue-size <Long: queue_size>         If set and the producer is running in  
                                          asynchronous mode, this gives the    
                                          maximum amount of  messages will     
                                          queue awaiting suffient batch size.  
                                          (default: 10000)                     
--request-required-acks <Integer:       The required acks of the producer      
  request required acks>                  requests (default: 0)                
--request-timeout-ms <Integer: request  The ack timeout of the producer        
  timeout ms>                             requests. Value must be non-negative
                                          and non-zero (default: 1500)         
--retry-backoff-ms <Long>               Before each retry, the producer        
                                          refreshes the metadata of relevant   
                                          topics. Since leader election takes  
                                          a bit of time, this property         
                                          specifies the amount of time that    
                                          the producer waits before refreshing
                                          the metadata. (default: 100)         
--socket-buffer-size <Integer: size>    The size of the tcp RECV size.         
                                          (default: 102400)                    
--sync                                  If set message send requests to the    
                                          brokers are synchronously, one at a  
                                          time as they arrive.                 
--timeout <Long: timeout_ms>            If set and the producer is running in  
                                          asynchronous mode, this gives the    
                                          maximum amount of time a message     
                                          will queue awaiting suffient batch   
                                          size. The value is given in ms.      
                                          (default: 1000)                      
--topic <topic>                         REQUIRED: The topic id to produce      
                                          messages to.                         
--value-serializer <encoder_class>      The class name of the message encoder  
                                          implementation to use for            
                                          serializing values. (default: kafka.
                                          serializer.StringEncoder)

```
