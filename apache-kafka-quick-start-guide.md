* http://www.ashishpaliwal.com/blog/2015/06/apache-kafka-quick-start-guide/
* http://www.ashishpaliwal.com/blog/2015/06/kafka-cookboook-simple-producer/
* http://wpcertification.blogspot.com/2014/08/java-client-for-publishing-and.html
* https://www.tutorialspoint.com/apache_kafka/apache_kafka_quick_guide.htm
* http://vulab.com/blog/?p=611
* http://khangaonkar.blogspot.com/2014/05/apache-kafka-java-tutorial.html

-----------------------------------------------------------------------------------------------------------------------
Kafka Commands 
-----------------------------------------------------------------------------------------------------------------------
--Start Server

       bin/zookeeper-server-start.sh config/zookeeper.properties

--Create Topic

      bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic myTopic

--List of topic

     bin/kafka-topics.sh --list --zookeeper localhost:2181

--Write in topic

     bin/kafka-console-producer.sh --broker-list localhost:9092 --topic myTopic

--Reading topic data
bin/kafka-console-consumer.sh --zookeeper localhost:2181 --from-beginning --topic myTopic


-----------------------------------------------------------------------------------------------------------------------
 First setup the consumer configuration
-----------------------------------------------------------------------------------------------------------------------


        Properties props = new Properties();
        props.put("zookeeper.connect", "host:2181");
        props.put("group.id", "mygroupid1");
        props.put("zookeeper.session.timeout.ms", "413");
        props.put("zookeeper.sync.time.ms", "203");
        props.put("auto.commit.interval.ms", "1000");
        ConsumerConfig cf = new ConsumerConfig(props) ;
-----------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------

**Apache Kafka - Fundamentals**

**1 Topics**
A stream of messages belonging to a particular category is called a topic. Data is stored in topics.
Topics are split into partitions. For each topic, Kafka keeps a mini-mum of one partition. Each such partition contains messages in an immutable ordered sequence. A partition is implemented as a set of segment files of equal sizes.

**2 Partition**
Topics may have many partitions, so it can handle an arbitrary amount of data.

**3 Partition offset**
Each partitioned message has a unique sequence id called as "offset".

**4 Replicas of partition**
Replicas are nothing but "backups" of a partition. Replicas are never read or write data. They are used to prevent data loss.

**5 Brokers**
Brokers are simple system responsible for maintaining the pub-lished data. Each broker may have zero or more partitions per topic. Assume, if there are N partitions in a topic and N number of brokers, each broker will have one partition.

Assume if there are N partitions in a topic and more than N brokers (n + m), the first N broker will have one partition and the next M broker will not have any partition for that particular topic.

Assume if there are N partitions in a topic and less than N brokers (n-m), each broker will have one or more partition sharing among them. This scenario is not recommended due to unequal load distri-bution among the broker.

**6 Kafka Cluster**
Kafka’s having more than one broker are called as Kafka cluster. A Kafka cluster can be expanded without downtime. These clusters are used to manage the persistence and replication of message data.

**7 Producers**
Producers are the publisher of messages to one or more Kafka topics. Producers send data to Kafka brokers. Every time a producer pub-lishes a message to a broker, the broker simply appends the message to the last segment file. Actually, the message will be appended to a partition. Producer can also send messages to a partition of their choice.

**8 Consumers**
Consumers read data from brokers. Consumers subscribes to one or more topics and consume published messages by pulling data from the brokers.

**9 Leader**
"Leader" is the node responsible for all reads and writes for the given partition. Every partition has one server acting as a leader.

**10 Follower**
Node which follows leader instructions are called as follower. If the leader fails, one of the follower will automatically become the new leader. A follower acts as normal consumer, pulls messages and up-dates its own data store.


**Apache Kafka - Cluster Architecture**
![](https://www.tutorialspoint.com/apache_kafka/images/cluster_architecture.jpg)





**1 Broker**
Kafka cluster typically consists of multiple brokers to maintain load balance. Kafka brokers are stateless, so they use ZooKeeper for maintaining their cluster state. One Kafka broker instance can handle hundreds of thousands of reads and writes per second and each bro-ker can handle TB of messages without performance impact. Kafka broker leader election can be done by ZooKeeper.

**2 ZooKeeper**
ZooKeeper is used for managing and coordinating Kafka broker. ZooKeeper service is mainly used to notify producer and consumer about the presence of any new broker in the Kafka system or failure of the broker in the Kafka system. As per the notification received by the Zookeeper regarding presence or failure of the broker then pro-ducer and consumer takes decision and starts coordinating their task with some other broker.

**3 Producers**
Producers push data to brokers. When the new broker is started, all the producers search it and automatically sends a message to that new broker. Kafka producer doesn’t wait for acknowledgements from the broker and sends messages as fast as the broker can handle.

**4 Consumers**

Since Kafka brokers are stateless, which means that the consumer has to maintain how many messages have been consumed by using partition offset. If the consumer acknowledges a particular message offset, it implies that the consumer has consumed all prior messages. The consumer issues an asynchronous pull request to the broker to have a buffer of bytes ready to consume. The consumers can rewind or skip to any point in a partition simply by supplying an offset value. Consumer offset value is notified by ZooKeeper.


