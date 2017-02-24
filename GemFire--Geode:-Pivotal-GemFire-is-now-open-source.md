#Topology:

## **Peer-to-Peer Configuration**

* In a peer-to-peer distributed system, applications can use GemFire 'mirroring' to replicate the data to other
nodes or to partition large volumes of data across multiple nodes.

* In this cache configuration, the cache is maintained locally in each peer application
and shares the cache space with the application memory space.

* This configuration offers the best performance when the cache hit rate is high. Multiple such embedded caches can be linked together to form a distributed peer-to-peer network.

* When using the embedded cache, applications configure data regions either using an XML-based cache configuration file or through runtime invocation of the GemFire API. 

![](https://image.slidesharecdn.com/effectiveapplicationdevelopmentwithgemfireusingspringdatagemfire-141105113157-conversion-gate02/95/effective-application-development-with-gemfire-and-spring-data-gemfire-21-638.jpg?cb=1415188008)



