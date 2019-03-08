`package com.khan.vaquar;`

 

`import java.util.Arrays;`

 

`import org.apache.spark.SparkConf;`

`import org.apache.spark.SparkContext;`

`import org.apache.spark.api.java.JavaRDD;`

`import org.apache.spark.api.java.function.Function;`

`import org.apache.spark.ml.feature.NGram;`

`import org.apache.spark.ml.util.DefaultParamsReader.Metadata;`

`import org.apache.spark.sql.Dataset;`

`import org.apache.spark.sql.Row;`

`import org.apache.spark.sql.RowFactory;`

`import org.apache.spark.sql.SQLContext;`

`import org.apache.spark.sql.SparkSession;`

`import org.apache.spark.sql.types.DataTypes;`

`import org.apache.spark.sql.types.StringType;`

`import org.apache.spark.sql.types.StructField;`

`import org.apache.spark.sql.types.StructType;`

 

`public class SparkHiveConnection {`

                `public static void main(String[] args) {`

                                `SparkHiveConnection app = new SparkHiveConnection();`

                                `try`

`{                                                 app.start_1();                                 }`
`catch (Exception e)`

`{                                                 e.printStackTrace();                                 }`
                `}`

 

 

 

                `private void start_1() {`

 

                                `SparkConf conf = new SparkConf().setAppName("Checkpoint").setMaster("local[*]");`

 

                                `SparkContext sparkContext = new SparkContext(conf);`

 

                                `// We need to specify where Spark will save the checkpoint file. It can`

                                `// be an HDFS location.`

                                `sparkContext.setCheckpointDir("/tmp");`

                                `//`

                                `// SparkSession spark =`

                                `// SparkSession.builder().appName("SparkHiveExample").master("local[*]").getOrCreate();`

 

                                `String hiveLocation = "jdbc:hive2://1XXXXXXXXXXXXX:2181,1XXXXXXXXXY:2181,1XXXXXXXXXXXZ:2181/;serviceDiscoveryMode=zookeeper;zookeeperNameSpace=hiveserver2";`

                               

 

                                `SparkSession spark = SparkSession.builder().appName("SparkHiveExample")`

                                                                `// .master("local[*]")`

                                                                `// .config("spark.sql.warehouse.dir", hiveLocation)`

                                                                `// .config("hive.metastore.uris",hiveLocation)//`

                                                                `// "thrift://localhost:9083"`

                                                                `// .config("hive.mapred.supports.subdirectories", "true")`

                                                                `// .config("spark.driver.allowMultipleContexts", "true")`

                                                                `// .config("mapreduce.input.fileinputformat.input.dir.recursive",`

                                                                `// "true")`

                                                                `// .config("checkpointLocation", "/tmp/hive") // <-- checkpoint`

                                                                `// directory`

                                                                `// .config(" spark.sql.warehouse.dir",`

                                                                `// "//1x.x.x.x/apps/hive/warehouse")`

                                                                `// .enableHiveSupport()`

                                                                `.getOrCreate();`

 

                               

                               

                                `StructType schema = DataTypes.createStructType(new StructField[]`

`{                             DataTypes.createStructField("AAAA",  DataTypes.StringType, true),                             DataTypes.createStructField("BBBB", DataTypes.TimestampType, true),                             DataTypes.createStructField("CCCCC", DataTypes.TimestampType, true),                             DataTypes.createStructField("DDDDD", DataTypes.StringType, true),                             DataTypes.createStructField("EEEEEEEE", DataTypes.StringType, true),                             DataTypes.createStructField("FFFFFFFFFFFF", DataTypes.StringType, true),                             DataTypes.createStructField("GGGGGGGGGGGG", DataTypes.StringType, true),                             DataTypes.createStructField("HHHHHHHHH", DataTypes.StringType, true)                     }`
`);`

                               

                               

                               

                               

                                `System.out.print("SQL Session--------------------");`

 

                                `Dataset<Row> jdbcDF = spark.read()`

                                                                `.format("jdbc")`

                                                                `.option("url", hiveLocation)`

                                                                `.option("dbtable", "schema.TableName")`

                                                                `.option("user", "username")`

                                                                `.option("password", "password")`

                                                                `.option("fetchsize", "20")`

                                                                `.option("inferSchema", false)`

                                                                `//.schema(schema)`

                                                                `// .option("driver", "org.apache.hadoop.hive.jdbc.HiveDriver")`

                                                                `.load();`

 

                                 

                                `System.out.print("able to connect------------------ ");`

 

                                `jdbcDF.printSchema();`

 

                                `System.out.print("Results ------------------");`

                               

                               

                                `jdbcDF.select("columnName").alias("alias").limit(2).show();`

 

 

                `}`

               

               

 

`}`


Error 👍

`   org.apache.spark.sql.AnalysisException: cannot resolve '`XXX.yyy`' given input columns: error`

      `at org.apache.spark.sql.catalyst.analysis.package$AnalysisErrorAt.failAnalysis(package.scala:42)`

       `at org.apache.spark.sql.catalyst.analysis.CheckAnalysis$$anonfun$checkAnalysis$$anonfun$apply.applyOrElse(CheckAnalysis.scala:88)`

       `at org.apache.spark.sql.catalyst.analysis.CheckAnalysis$$anonfun$checkAnalysis$$anonfun$apply.applyOrElse(CheckAnalysis.scala:85)`


**Resolution :**

If Apache Spark column contain "." in between hive column name will get above Exception .
You can update it with col("`ABC`") char .
 
 

