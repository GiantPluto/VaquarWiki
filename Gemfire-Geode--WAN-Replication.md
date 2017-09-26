The GemFire Multi-site (WAN) Architecture provides a way to connect to and asynchronously distribute events between disparate GemFire distributed systems. The main purpose of this architecture is to keep the data in two or more distributed systems consistent with each other. Each distributed system is referred to as a site, and each site is typically connected by a metropolitan area network (MAN) for disaster recovery or wide area network (WAN) for follow-the-sun processing. Events being distributed from one site to another are stored in queues on the local site and asynchronously batched and distributed to the remote site. The queues can be persistent, conflated, overflowed and/or highly available.

Each locator in a WAN configuration defines a unique distributed-system-id property that identifies the local cluster to which it belongs. A locator uses the remote-locators property to define the addresses of one or more locators in remote clusters to use for WAN distribution.

When a locator starts up, it contacts each locator that is configured in the remote-locators property to exchange information about the available locators and gateway receivers in the cluster. 

**Gateway Senders**

A Geode cluster uses a gateway sender to distribute region events to another, remote Geode cluster. You can create multiple gateway sender configurations to distribute region events to multiple remote clusters, and/or to distribute region events concurrently to another remote cluster.

A gateway sender always communicates with a gateway receiver in a remote cluster. Gateway senders do not communicate directly with other cache server instances. 

**Serial Gateway Senders**

serial gateway sender has a single distribution point, it provides the most control over ordering region events as they are distributed across the WAN. However, a serial gateway sender provides only a finite amount of throughput, so it may be a performance bottleneck.

![](https://gemfire.docs.pivotal.io/geode/images/serial_sender.png)


**Parallel Gateway Senders**

Parallel gateway sender distributes region events from each of the Geode servers that host a partitioned region. For a partitioned region, each server that hosts primary buckets for the region uses its own logical queue to distribute events for those buckets. WAN distribution throughput scales automatically with each new instance of the parallel gateway sender.

Replicated regions cannot use a parallel gateway sender.


![](https://gemfire.docs.pivotal.io/geode/images/parallel_sender.png)


**Gateway Sender Queues**

The queue that a gateway sender uses to distribute events to a remote site overflows to disk as needed, in order to prevent the Geode member from running out of memory. You can configure the maximum amount of memory that each queue uses, as well as the batch size and frequency for processing batches in the queue. You can also configure these queues to persist to disk,


By default gateway sender queues use 5 threads to dispatch queued events. With a serial gateway sender, the single, logical queue that is hosted on a member is divided into multiple physical queues (5 by default) each with a dedicated dispatcher thread.

**NOTE **

1. When a serial gateway sender configuration is deployed to multiple Geode members, only one “primary” sender is active at a given time. All other serial gateway sender instances are inactive “secondaries” that are available as backups if the primary sender shuts down. Geode designates the first gateway sender to start up as the primary sender, and all other senders become secondaries. As gateway senders start and shut down in the distributed system, Geode ensures that the oldest running gateway sender operates as the primary.

2. A parallel gateway sender is deployed to multiple Geode members by default, and each member that hosts primary buckets for a partitioned region actively distributes data to the remote Geode site. When you use parallel gateway senders, high availability for WAN distribution is provided if you configure the partitioned region for redundancy. With a redundant partitioned region, if a member that hosts primary buckets fails or is shut down, then a Geode member that hosts a redundant copy of those buckets takes over WAN distribution for those buckets.


