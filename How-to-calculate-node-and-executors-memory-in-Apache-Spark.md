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



https://blog.cloudera.com/blog/2015/03/how-to-tune-your-apache-spark-jobs-part-2/