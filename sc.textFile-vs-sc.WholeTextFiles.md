**Spark Core: sc.textFile vs sc.WholeTextFiles**

While loading a RDD from source data, there are two choices which look similar.

    scala&gt; val movies = sc.textFile("movies")
    scala&gt; val movies = sc.wholeTextFiles("movies")

**sc.textFile**
SparkContext’s TextFile method, i.e., sc.textFile in Spark Shell, creates a RDD with each line as an element. If there are 10 files in movies folder, 10 partitions will be created. You can verify the number of partitions by:
scala&gt; movies.partitions.length

**sc.wholeTextFiles**
SparkContext’s whole text files method, i.e., sc.wholeTextFiles in Spark Shell, creates a PairRDD with the key being the file name with a path. It’s a full path like “hdfs://m1.zettabytes.com:9000/user/hduser/movies/movie1.txt”. The value is the whole content of file in String. Here the number of partitions will be 1 or more depending upon how many executor cores you have.