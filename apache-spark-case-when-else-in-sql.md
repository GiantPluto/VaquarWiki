https://stackoverflow.com/questions/30783517/apache-spark-add-an-case-when-else-calculated-column-to-an-existing-d
https://stackoverflow.com/questions/37064315/how-to-write-case-with-when-condition-in-spark-sql-using-scala
https://stackoverflow.com/questions/40522149/spark-sql-implement-and-condition-inside-a-case-statement


    df.registerTempTable("data")
    val df4 = sql(""" select *, case when color = 'green' then 1 else 0 end as Green_ind from data """)

   SELECT CASE WHEN key = 1 THEN 1 ELSE 2 END FROM testData

     print(s"Spark ${spark.version}")
     val df = spark.createDataFrame(Seq(( 2,  9), ( 1,  5),( 1,  1),( 1,  2),( 2,  8)))
              .toDF("y", "x")
      df.createOrReplaceTempView("test")
 
     spark.sql("select CASE WHEN y = 2 THEN 'A' ELSE 'B' END AS flag, x from test").show


