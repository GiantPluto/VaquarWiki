If we have following hardware then calculate spark 
* 6 Node
* 16 code 
* 64 GB of Ram
------------------------------------------------
* Number of executer (--num -executors)
* Coures for each executer(--executorr-cores)
* Memory for each executer (--executor-memory)
------------------------------------------------
* --executer-memory controls the heap size
* Node some overhead(controlledby spark.yarn.executor.memory.overhead) for off heap memory default is max
 (384 MB ,.07* Spark.executer.memory)
------------------------------------------------
* 15 cores per executer can lead to bad HDFS I/O throughput.
* Best is to keep under 5 cores per executor
------------------------------------------------

![](http://blog.cloudera.com/wp-content/uploads/2015/03/spark-tuning2-f1.png)


**Calculations:**

* 5 core per executor
* -For max HDFS throughput
* Cluster has 6*15 =90 cores in total
* afer taking out Hadoop /Yarn daemon cores)
* 90 cores /5 cores/executor  (19/5=18-1)
* =18 executors
* Each node has 3 executors
* 63 GB/3 =21 GB ,21*(1 -0,07)
* ~19 GB
* 1 executor for AM=> 17 executor

------------------------------------------------

Ans 

* 17 Executors in total
* 19 GB memory /executor
* 5 cores  /executor
------------------------------------------------
* Dynamic resources allocation in production not recommended as you already aware your requirements and resources.
* Dynamic allocation you can use in before pro to play

------------------------------------------------
*  Partition rule of thumb 128 MB per partition 
*  If partition less but near 2000 bump to more than 2000 (Spark hardcoded value is 2000 for compress )

------------------------------------------------
* Shuffles are t be avoided 
* Cross Join should be avoided
* ReduceByKey over GroupByKey
* TreesReduce over Reduce
* Use complex/Nested type 
------------------------------------------------

https://blog.cloudera.com/blog/2015/03/how-to-tune-your-apache-spark-jobs-part-2/




------------------------------------------
**Spark Memory Management**


![](![](https://0x0fff.com/wp-content/uploads/2016/01/Spark-Memory-Management-1.6.0-768x808.png))



You can see 3 main memory regions on the diagram:

Reserved Memory. This is the memory reserved by the system, and its size is hardcoded. As of Spark 1.6.0, its value is 300MB, which means that this 300MB of RAM does not participate in Spark memory region size calculations, and its size cannot be changed in any way without Spark recompilation or setting spark.testing.reservedMemory, which is not recommended as it is a testing parameter not intended to be used in production. Be aware, this memory is only called “reserved”, in fact it is not used by Spark in any way, but it sets the limit on what you can allocate for Spark usage. Even if you want to give all the Java Heap for Spark to cache your data, you won’t be able to do so as this “reserved” part would remain spare (not really spare, it would store lots of Spark internal objects).


 For your information, if you don’t give Spark executor at least 1.5 * Reserved Memory = 450MB heap, it will fail with “please use larger heap size” error message.

User Memory. This is the memory pool that remains after the allocation of Spark Memory, and it is completely up to you to use it in a way you like. You can store your own data structures there that would be used in RDD transformations. For example, you can rewrite Spark aggregation by using mapPartitions transformation maintaining hash table for this aggregation to run, which would consume so called User Memory. 


In Spark 1.6.0 the size of this memory pool can be calculated as (“Java Heap” – “Reserved Memory”) * (1.0 – spark.memory.fraction), which is by default equal to (“Java Heap” – 300MB) * 0.25. For example, with 4GB heap you would have 949MB of User Memory. And again, this is the User Memory and its completely up to you what would be stored in this RAM and how, Spark makes completely no accounting on what you do there and whether you respect this boundary or not. Not respecting this boundary in your code might cause OOM error.


Spark Memory. Finally, this is the memory pool managed by Apache Spark. Its size can be calculated as (“Java Heap” – “Reserved Memory”) * spark.memory.fraction, and with Spark 1.6.0 defaults it gives us (“Java Heap” – 300MB) * 0.75. For example, with 4GB heap this pool would be 2847MB in size.

This whole pool is split into 2 regions – Storage Memory and Execution Memory, and the boundary between them is set by spark.memory.storageFraction parameter, which defaults to 0.5. 

The advantage of this new memory management scheme is that this boundary is not static, and in case of memory pressure the boundary would be moved, i.e. one region would grow by borrowing space from another one. I would discuss the “moving” this boundary a bit later, now let’s focus on how this memory is being used:

Storage Memory. This pool is used for both storing Apache Spark cached data and for temporary space serialized data “unroll”. Also all the “broadcast” variables are stored there as cached blocks.

In case you’re curious, here’s the code of unroll. As you may see, it does not require that enough memory for unrolled block to be available – in case there is not enough memory to fit the whole unrolled partition it would directly put it to the drive if desired persistence level allows this. As of “broadcast”, all the broadcast variables are stored in cache with MEMORY_AND_DISK persistence level.

Execution Memory. This pool is used for storing the objects required during the execution of Spark tasks. For example, it is used to store shuffle intermediate buffer on the Map side in memory, also it is used to store hash table for hash aggregation step. This pool also supports spilling on disk if not enough memory is available, but the blocks from this pool cannot be forcefully evicted by other threads (tasks).

Ok, so now let’s focus on the moving boundary between Storage Memory and Execution Memory. Due to nature of Execution Memory, you cannot forcefully evict blocks from this pool, because this is the data used in intermediate computations and the process requiring this memory would simply fail if the block it refers to won’t be found. But it is not so for the Storage Memory – it is just a cache of blocks stored in RAM, and if we evict the block from there we can just update the block metadata reflecting the fact this block was evicted to HDD (or simply removed), and trying to access this block Spark would read it from HDD (or recalculate in case your persistence level does not allow to spill on HDD).

So, we can forcefully evict the block from Storage Memory, but cannot do so from Execution Memory. When Execution Memory pool can borrow some space from Storage Memory? It happens when either:

There is free space available in Storage Memory pool, i.e. cached blocks don’t use all the memory available there. Then it just reduces the Storage Memory pool size, increasing the Execution Memory pool.
Storage Memory pool size exceeds the initial Storage Memory region size and it has all this space utilized. This situation causes forceful eviction of the blocks from Storage Memory pool, unless it reaches its initial size.
In turn, Storage Memory pool can borrow some space from Execution Memory pool only if there is some free space in Execution Memory pool available.

Initial Storage Memory region size, as you might remember, is calculated as “Spark Memory” * spark.memory.storageFraction = (“Java Heap” – “Reserved Memory”) * spark.memory.fraction * spark.memory.storageFraction. With default values, this is equal to (“Java Heap” – 300MB) * 0.75 * 0.5 = (“Java Heap” – 300MB) * 0.375. For 4GB heap this would result in 1423.5MB of RAM in initial Storage Memory region.

This implies that if we use Spark cache and the total amount of data cached on executor is at least the same as initial Storage Memory region size, we are guaranteed that storage region size would be at least as big as its initial size, because we won’t be able to evict the data from it making it smaller. However, if your Execution Memory region has grown beyond its initial size before you filled the Storage Memory region, you won’t be able to forcefully evict entries from Execution Memory, so you would end up with smaller Storage Memory region while execution holds its blocks in memory.