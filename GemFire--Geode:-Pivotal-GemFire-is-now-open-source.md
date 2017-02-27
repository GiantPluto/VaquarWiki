#Topology:


![](https://d1fto35gcfffzn.cloudfront.net/images/products/pivotal-gemfire/gemfire-marchitecture-diagram.png)

## **Peer-to-Peer Configuration**

* In a peer-to-peer distributed system, applications can use GemFire 'mirroring' to replicate the data to other
nodes or to partition large volumes of data across multiple nodes.

* In this cache configuration, the cache is maintained locally in each peer application
and shares the cache space with the application memory space.

* This configuration offers the best performance when the cache hit rate is high. Multiple such embedded caches can be linked together to form a distributed peer-to-peer network.

* When using the embedded cache, applications configure data regions either using an XML-based cache configuration file or through runtime invocation of the GemFire API. 

### Proc
* It provides instantaneous access to data for READ_MOSTLY applications
* It provides recovery capability for other members in the system by replicating all the members' region values

###Con
Using same application memory .


### Configuration 


    <cache>
       <vm-root-region name='items'>
       <region-attributes mirror-type='keys-values'>
       </region-attributes>
      </vm-root-region>
     </cache>


** Declaring a partitioned region**

       <cache>
        <vm-root-region name='items'>
        <region-attributes>
        <partition-attributes redundant-copies='1'>
        </partition-attributes>
       </region-attributes>
        </vm-root-region>
      </cache>

![](https://image.slidesharecdn.com/effectiveapplicationdevelopmentwithgemfireusingspringdatagemfire-141105113157-conversion-gate02/95/effective-application-development-with-gemfire-and-spring-data-gemfire-21-638.jpg?cb=1415188008)



###Peer-to-Peer Scenario: Partitioned cache

* Addressable memory on a given machine ranges from as little as 2GB (Windows) to 4GB (depending on the flavor of Linux) and actual allocation is limited to 1.5GB to 3.5GB in 32-bit operating environments. 

* In addition to this limitation, if the cache is collocated with the application process, then the cache has to share this addressable space with the application, further limiting the maximum size of the cache. 

* To manage data much larger than the single process space limit or the capacity of a single machine, GemFire supports a model for automatically partitioning the data across many processes and nodes.

*  It provides this functionality through highly-available, concurrent, scalable distributed data structures.



## **Client-Server Topology**

Client-server caching is a deployment model that allows a large number of caches in any number of nodes
to be connected to one another in a client-server relationship. In this configuration, client caches—GemFire
caches at the outer edge of the hierarchy, communicate with server caches on back-end servers. Server
caches can in turn be clients of other server caches or can replicate their data by enlisting server peers as
mirrors. This is a fed


More ... http://gemfire.docs.pivotal.io/geode/topologies_and_comm/cs_configuration/chapter_overview.html 


![](https://image.slidesharecdn.com/effectiveapplicationdevelopmentwithgemfireusingspringdatagemfire-141105113157-conversion-gate02/95/effective-application-development-with-gemfire-and-spring-data-gemfire-22-638.jpg?cb=1415188008)


## Multi-site (WAN) Configuration

Use the multi-site configuration to scale horizontally between disparate, loosely-coupled distributed systems. A wide-area network (WAN) is the main use case for the multi-site topology.


![](https://image.slidesharecdn.com/vmwarevfabric5-whatsnewtechnicalsalestrainingpresentation-150728070254-lva1-app6892/95/v-mware-v-fabric-5-whats-new-technical-sales-training-presentation-19-638.jpg?cb=1438067983)


* http://techiekhannotes.blogspot.com/2017/01/apachegeodegemfire-real-time-and.html

* http://techiekhannotes.blogspot.com/2016/07/pivotal-gemfire-with-example_27.html

* https://www.slideshare.net/SpringCentral/effective-application-development-with-gem-fire-using-spring-data-gemfire