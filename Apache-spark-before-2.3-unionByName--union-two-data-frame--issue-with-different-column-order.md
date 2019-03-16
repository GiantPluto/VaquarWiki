val df31 = Seq((1,"VK",30,"IL"),(2, "AK", 31,"CA"),(3,"BK", 15,"KT"),(4,"CK",10,"NY")).toDF("policyId", "name", "premium", "state")

val df32 = Seq(("VK", "IL", 1,30),("AK","CA", 2,31),("NK", "IL", 11,30),("DK", "IL", 12,30)).toDF("name", "state", "policyId", "premium")

4
scala> df31.show()
+--------+----+-------+-----+
|policyId|name|premium|state|
+--------+----+-------+-----+
|       1|  VK|     30|   IL|
|       2|  AK|     31|   CA|
|       3|  BK|     15|   KT|
|       4|  CK|     10|   NY|
+--------+----+-------+-----+


scala> df32.show()
+----+-----+--------+-------+
|name|state|policyId|premium|
+----+-----+--------+-------+
|  VK|   IL|       1|     30|
|  AK|   CA|       2|     31|
|  NK|   IL|      11|     30|
|  DK|   IL|      12|     30|
+----+-----+--------+-------+




scala> val df13=df31.union(df32)
df13: org.apache.spark.sql.Dataset[org.apache.spark.sql.Row] = [policyId: string, name: string ... 2 more fields]

scala> df13.show()
+--------+----+-------+-----+
|policyId|name|premium|state|
+--------+----+-------+-----+
|       1|  VK|     30|   IL|
|       2|  AK|     31|   CA|
|       3|  BK|     15|   KT|
|       4|  CK|     10|   NY|
|      VK|  IL|      1|   30|
|      AK|  IL|      2|   30|
|      NK|  IL|     11|   30|
|      DK|  IL|     12|   30|
+--------+----+-------+-----+





------------------------------------

**Solution** 👍 

import org.apache.spark.sql.DataFrame
import org.apache.spark.sql.functions.col

def unionByName(a: DataFrame, b: DataFrame): DataFrame = {
  val columns = a.columns.toSet.intersect(b.columns.toSet).map(col).toSeq
  a.select(columns: _).unionAll(b.select(columns: _))
}

-----------------------------------------------

scala> import org.apache.spark.sql.DataFrame
import org.apache.spark.sql.DataFrame

scala> import org.apache.spark.sql.functions.col
import org.apache.spark.sql.functions.col

scala> def unionByName(a: DataFrame, b: DataFrame): DataFrame = {
     |   val columns = a.columns.toSet.intersect(b.columns.toSet).map(col).toSeq
     |   a.select(columns: _*).unionAll(b.select(columns: _*))
     | }
warning: there was one deprecation warning; re-run with -deprecation for details
unionByName: (a: org.apache.spark.sql.DataFrame, b: org.apache.spark.sql.DataFrame)org.apache.spark.sql.DataFrame

scala> unionByName(df31,df32).show
+--------+----+-------+-----+
|policyId|name|premium|state|
+--------+----+-------+-----+
|       1|  VK|     30|   IL|
|       2|  AK|     31|   CA|
|       3|  BK|     15|   KT|
|       4|  CK|     10|   NY|
|       1|  VK|     30|   IL|
|       2|  AK|     31|   CA|
|      11|  NK|     30|   IL|
|      12|  DK|     30|   IL|
+--------+----+-------+-----+



