There is no practical difference when it comes to performance whatsoever:

RDD.fold action is using fold on the partition Iterators which is implemented using foldLeft.
RDD.reduce is using reduceLefton the partition Iterators.
Both methods keep mutable accumulator and process partitions sequentially using simple loops with foldLeft implemented like 

https://github.com/scala/scala/blob/2.12.x/src/library/scala/collection/TraversableOnce.scala#L155

Practical difference between these methods in Spark is only related to their behavior on empty collections and ability to use mutable buffer (arguably it is related to performance). You'll find some discussion 

https://stackoverflow.com/questions/34529953/why-is-the-fold-action-necessary-in-spark


Moreover there is no difference in the overall processing model:

- Each partition is processed sequentially using a single thread.
- Partitions are processed in parallel using multiple executors / executor threads.
- Final merge is performed sequentially using a single thread on the driver.



