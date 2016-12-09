**Creating a SparkSession      **
      

           import org.apache.spark._
           import org.apache.spark.streaming._
           import org.apache.spark.streaming.StreamingContext._


        //set up the spark configuration and create contexts
        val sparkConf = new SparkConf().setAppName("SparkSessionZipsExample").setMaster("local")
        // your handle to SparkContext to access other context like SQLContext
        val sc = new SparkContext(sparkConf).set("spark.some.config.option", "some-value")
        val sqlContext = new org.apache.spark.sql.SQLContext(sc)

Example :
* https://github.com/dmatrix/examples/blob/master/spark/databricks/apps/scala/2.0/src/main/scala/zips/SparkSessionZipsExample.scala