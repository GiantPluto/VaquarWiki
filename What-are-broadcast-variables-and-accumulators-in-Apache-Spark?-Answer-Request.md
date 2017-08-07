Broadcast is a read-only global variable, which all nodes of a cluster can read. Think of them more like as a lookup variables.

On the other hand think of accumulators as a global counter variable where each node of the cluster can write values in to. These are the variables that you want to keep updating as a part of your operation like for example while reading log lines, one would like to maintain a real time count of number of certain log type records identified.


-------------------------------------------------------------------

**BROADCAST VARIABLE:**

Broadcast variables are pretty simple in concept. They're variables that we want to share throughout our cluster. However there are a couple of caveats that are important to understand. Broadcast variables have to be able to fit in memory on one machine. That means that they definitely should NOT be anything super large, like a large table or massive vector. Secondly, broadcast variables are immutable, meaning that they cannot be changed later on. This may seem inconvenient but it truly suits their use case.

broadcast variables are:

- Immutable
- Distributed to the cluster
- Fit in memory

------------------------------------------------------

**ACCUMULATOR VARIABLE:**

Accumulators are Spark’s answer to Map-reduce counters but they do much more than that. Let’s first start with a simple example which uses accumulators for simple count.

val ac = sc.accumulator(0);

val words = sc.textFile("/tmp/ashish/README").flatMap(_.split("\\W+")[1]);

words.foreach(w => ac += 1 );

ac.value.

Accumulator is simply for counter purpose it's value can be read by Driver only.

