import org.apache.spark.sql.DataFrame
import org.apache.spark.sql.functions.col

def unionByName(a: DataFrame, b: DataFrame): DataFrame = {
  val columns = a.columns.toSet.intersect(b.columns.toSet).map(col).toSeq
  a.select(columns: _*).unionAll(b.select(columns: _*))
}



Background: I need to union two dataset but issue is columns are not in sequence 

policyId, name, premium, state
name, state, policyId, premium

import org.apache.spark.sql.functions._
import spark.implicits._


val df31 = Seq((1,"VK",30,"IL"),(2, "AK", 31,"CA"),(3,"BK", 15,"KT"),(4,"CK",10,"NY")).toDF("policyId", "name", "premium", "state")

val df32 = Seq(("VK", "IL", 1,30),("AK","CA", 2,31),("NK", "IL", 11,30),("DK", "IL", 12,30)).toDF("name", "state", "policyId", "premium")

                                                                                                  
																								  
scala> val df21 = List(df11).toDF("ID","Name","ID2","Marks")



Microsoft Windows [Version 10.0.17134.648]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Users\vaqua>spark-shell
2019-03-15 20:21:47 WARN  NativeCodeLoader:62 - Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
Spark context Web UI available at http://LAPTOP-P0O04VLD:4040
Spark context available as 'sc' (master = local[*], app id = local-1552699389527).
Spark session available as 'spark'.
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 2.3.1
      /_/

Using Scala version 2.11.8 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_171)
Type in expressions to have them evaluated.
Type :help for more information.

scala>

scala> val df1 = Seq((1,"VK",30,"IL"),(2, "AK", 31,"CA"),(3,"BK", 15,"KT"),(4,"CK",10,"NY")).toDF("policyId", "name", "premium", "state")
2019-03-15 20:25:23 WARN  ObjectStore:568 - Failed to get database global_temp, returning NoSuchObjectException
df1: org.apache.spark.sql.DataFrame = [policyId: int, name: string ... 2 more fields]

scala> val df2 = Seq(("VK", "IL", 1,30),("AK","IL", 2,30),("NK", "IL", 11,30),("DK", "IL", 12,30)).toDF("name", "state", "policyId", "premium")
df2: org.apache.spark.sql.DataFrame = [name: string, state: string ... 2 more fields]

scala> df1.show90
<console>:26: error: value show90 is not a member of org.apache.spark.sql.DataFrame
       df1.show90
           ^

scala> df1.show()
+--------+----+-------+-----+
|policyId|name|premium|state|
+--------+----+-------+-----+
|       1|  VK|     30|   IL|
|       2|  AK|     31|   CA|
|       3|  BK|     15|   KT|
|       4|  CK|     10|   NY|
+--------+----+-------+-----+


scala> df2.show()
+----+-----+--------+-------+
|name|state|policyId|premium|
+----+-----+--------+-------+
|  VK|   IL|       1|     30|
|  AK|   IL|       2|     30|
|  NK|   IL|      11|     30|
|  DK|   IL|      12|     30|
+----+-----+--------+-------+


scala> import org.apache.spark.sql.functions._
import org.apache.spark.sql.functions._

scala> val df1 = sc.parallelize(List(
     |   (50, 2),
     |   (34, 4)
     | )).toDF("age", "children")
df1: org.apache.spark.sql.DataFrame = [age: int, children: int]

scala> )).toDF("age", "children")
<console>:1: error: illegal start of definition
)).toDF("age", "children")
^

scala> import spark.implicits._
import spark.implicits._

scala> val a = (1,"12",1,333)
a: (Int, String, Int, Int) = (1,12,1,333)

scala> val b = (1,"",3,989)
b: (Int, String, Int, Int) = (1,"",3,989)

scala> val c = (7,"98",8,878)
c: (Int, String, Int, Int) = (7,98,8,878)

scala> val df1 = List(a).toDF("ID","Name","ID2","Marks")
df1: org.apache.spark.sql.DataFrame = [ID: int, Name: string ... 2 more fields]

scala> val df2 = List(b, c).toDF("ID","Name","ID2","Marks")
df2: org.apache.spark.sql.DataFrame = [ID: int, Name: string ... 2 more fields]

scala> df1.show()
+---+----+---+-----+
| ID|Name|ID2|Marks|
+---+----+---+-----+
|  1|  12|  1|  333|
+---+----+---+-----+


scala> df2.show()
+---+----+---+-----+
| ID|Name|ID2|Marks|
+---+----+---+-----+
|  1|    |  3|  989|
|  7|  98|  8|  878|
+---+----+---+-----+


scala> df1.union(df2)
res5: org.apache.spark.sql.Dataset[org.apache.spark.sql.Row] = [ID: int, Name: string ... 2 more fields]

scala> df3=df1.union(df2)
<console>:35: error: not found: value df3
val $ires9 = df3
             ^
<console>:33: error: not found: value df3
       df3=df1.union(df2)
       ^

scala> val df3=df1.union(df2)
df3: org.apache.spark.sql.Dataset[org.apache.spark.sql.Row] = [ID: int, Name: string ... 2 more fields]

scala> df3.show()
+---+----+---+-----+
| ID|Name|ID2|Marks|
+---+----+---+-----+
|  1|  12|  1|  333|
|  1|    |  3|  989|
|  7|  98|  8|  878|
+---+----+---+-----+


scala> val df11 = Seq((1,"VK",30,"IL"),(2, "AK", 31,"CA"),(3,"BK", 15,"KT"),(4,"CK",10,"NY")).toDF("policyId", "name", "premium", "state")
df11: org.apache.spark.sql.DataFrame = [policyId: int, name: string ... 2 more fields]

scala>

scala> val df12 = Seq(("VK", "IL", 1,30),("AK","IL", 2,30),("NK", "IL", 11,30),("DK", "IL", 12,30)).toDF("name", "state", "policyId", "premium")
df12: org.apache.spark.sql.DataFrame = [name: string, state: string ... 2 more fields]

scala> val 13=df11.union(df12)
<console>:33: error: type mismatch;
 found   : Int(13)
 required: org.apache.spark.sql.Dataset[org.apache.spark.sql.Row]
       val 13=df11.union(df12)
           ^

scala> val df13=df11.union(df12)
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


scala> df11.select(("policyId", "name", "premium", "state").show()
     |
     | ;
<console>:3: error: ')' expected but ';' found.
;
^

scala> df11.select(("policyId", "name", "premium", "state").show()
     | df11.select("policyId", "name", "premium", "state").show();
<console>:2: error: ')' expected but '.' found.
df11.select("policyId", "name", "premium", "state").show();
    ^

scala> df11.select("policyId", "name", "premium", "state").show();
+--------+----+-------+-----+
|policyId|name|premium|state|
+--------+----+-------+-----+
|       1|  VK|     30|   IL|
|       2|  AK|     31|   CA|
|       3|  BK|     15|   KT|
|       4|  CK|     10|   NY|
+--------+----+-------+-----+


scala> df12.select("policyId", "name", "premium", "state").show();
+--------+----+-------+-----+
|policyId|name|premium|state|
+--------+----+-------+-----+
|       1|  VK|     30|   IL|
|       2|  AK|     30|   IL|
|      11|  NK|     30|   IL|
|      12|  DK|     30|   IL|
+--------+----+-------+-----+


scala>  Microsoft Windows [Version 10.0.17134.648]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Users\vaqua>spark-shell
2019-03-15 20:21:47 WARN  NativeCodeLoader:62 - Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
Spark context Web UI available at http://LAPTOP-P0O04VLD:4040
Spark context available as 'sc' (master = local[*], app id = local-1552699389527).
Spark session available as 'spark'.
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 2.3.1
      /_/

Using Scala version 2.11.8 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_171)
Type in expressions to have them evaluated.
Type :help for more information.

scala>

scala> val df1 = Seq((1,"VK",30,"IL"),(2, "AK", 31,"CA"),(3,"BK", 15,"KT"),(4,"CK",10,"NY")).toDF("policyId", "name", "premium", "state")
2019-03-15 20:25:23 WARN  ObjectStore:568 - Failed to get database global_temp, returning NoSuchObjectException
df1: org.apache.spark.sql.DataFrame = [policyId: int, name: string ... 2 more fields]

scala> val df2 = Seq(("VK", "IL", 1,30),("AK","IL", 2,30),("NK", "IL", 11,30),("DK", "IL", 12,30)).toDF("name", "state", "policyId", "premium")
df2: org.apache.spark.sql.DataFrame = [name: string, state: string ... 2 more fields]

scala> df1.show90
<console>:26: error: value show90 is not a member of org.apache.spark.sql.DataFrame
       df1.show90
           ^

scala> df1.show()
+--------+----+-------+-----+
|policyId|name|premium|state|
+--------+----+-------+-----+
|       1|  VK|     30|   IL|
|       2|  AK|     31|   CA|
|       3|  BK|     15|   KT|
|       4|  CK|     10|   NY|
+--------+----+-------+-----+


scala> df2.show()
+----+-----+--------+-------+
|name|state|policyId|premium|
+----+-----+--------+-------+
|  VK|   IL|       1|     30|
|  AK|   IL|       2|     30|
|  NK|   IL|      11|     30|
|  DK|   IL|      12|     30|
+----+-----+--------+-------+


scala> import org.apache.spark.sql.functions._
import org.apache.spark.sql.functions._

scala> val df1 = sc.parallelize(List(
     |   (50, 2),
     |   (34, 4)
     | )).toDF("age", "children")
df1: org.apache.spark.sql.DataFrame = [age: int, children: int]

scala> )).toDF("age", "children")
<console>:1: error: illegal start of definition
)).toDF("age", "children")
^

scala> import spark.implicits._
import spark.implicits._

scala> val a = (1,"12",1,333)
a: (Int, String, Int, Int) = (1,12,1,333)

scala> val b = (1,"",3,989)
b: (Int, String, Int, Int) = (1,"",3,989)

scala> val c = (7,"98",8,878)
c: (Int, String, Int, Int) = (7,98,8,878)

scala> val df1 = List(a).toDF("ID","Name","ID2","Marks")
df1: org.apache.spark.sql.DataFrame = [ID: int, Name: string ... 2 more fields]

scala> val df2 = List(b, c).toDF("ID","Name","ID2","Marks")
df2: org.apache.spark.sql.DataFrame = [ID: int, Name: string ... 2 more fields]

scala> df1.show()
+---+----+---+-----+
| ID|Name|ID2|Marks|
+---+----+---+-----+
|  1|  12|  1|  333|
+---+----+---+-----+


scala> df2.show()
+---+----+---+-----+
| ID|Name|ID2|Marks|
+---+----+---+-----+
|  1|    |  3|  989|
|  7|  98|  8|  878|
+---+----+---+-----+


scala> df1.union(df2)
res5: org.apache.spark.sql.Dataset[org.apache.spark.sql.Row] = [ID: int, Name: string ... 2 more fields]

scala> df3=df1.union(df2)
<console>:35: error: not found: value df3
val $ires9 = df3
             ^
<console>:33: error: not found: value df3
       df3=df1.union(df2)
       ^

scala> val df3=df1.union(df2)
df3: org.apache.spark.sql.Dataset[org.apache.spark.sql.Row] = [ID: int, Name: string ... 2 more fields]

scala> df3.show()
+---+----+---+-----+
| ID|Name|ID2|Marks|
+---+----+---+-----+
|  1|  12|  1|  333|
|  1|    |  3|  989|
|  7|  98|  8|  878|
+---+----+---+-----+


scala> val df11 = Seq((1,"VK",30,"IL"),(2, "AK", 31,"CA"),(3,"BK", 15,"KT"),(4,"CK",10,"NY")).toDF("policyId", "name", "premium", "state")
df11: org.apache.spark.sql.DataFrame = [policyId: int, name: string ... 2 more fields]

scala>

scala> val df12 = Seq(("VK", "IL", 1,30),("AK","IL", 2,30),("NK", "IL", 11,30),("DK", "IL", 12,30)).toDF("name", "state", "policyId", "premium")
df12: org.apache.spark.sql.DataFrame = [name: string, state: string ... 2 more fields]

scala> val 13=df11.union(df12)
<console>:33: error: type mismatch;
 found   : Int(13)
 required: org.apache.spark.sql.Dataset[org.apache.spark.sql.Row]
       val 13=df11.union(df12)
           ^

scala> val df13=df11.union(df12)
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


scala> df11.select(("policyId", "name", "premium", "state").show()
     |
     | ;
<console>:3: error: ')' expected but ';' found.
;
^

scala> df11.select(("policyId", "name", "premium", "state").show()
     | df11.select("policyId", "name", "premium", "state").show();
<console>:2: error: ')' expected but '.' found.
df11.select("policyId", "name", "premium", "state").show();
    ^

scala> df11.select("policyId", "name", "premium", "state").show();
+--------+----+-------+-----+
|policyId|name|premium|state|
+--------+----+-------+-----+
|       1|  VK|     30|   IL|
|       2|  AK|     31|   CA|
|       3|  BK|     15|   KT|
|       4|  CK|     10|   NY|
+--------+----+-------+-----+


scala> df12.select("policyId", "name", "premium", "state").show();
+--------+----+-------+-----+
|policyId|name|premium|state|
+--------+----+-------+-----+
|       1|  VK|     30|   IL|
|       2|  AK|     30|   IL|
|      11|  NK|     30|   IL|
|      12|  DK|     30|   IL|
+--------+----+-------+-----+


scala>Microsoft Windows [Version 10.0.17134.648]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Users\vaqua>spark-shell
2019-03-15 20:21:47 WARN  NativeCodeLoader:62 - Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
Spark context Web UI available at http://LAPTOP-P0O04VLD:4040
Spark context available as 'sc' (master = local[*], app id = local-1552699389527).
Spark session available as 'spark'.
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 2.3.1
      /_/

Using Scala version 2.11.8 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_171)
Type in expressions to have them evaluated.
Type :help for more information.

scala>

scala> val df1 = Seq((1,"VK",30,"IL"),(2, "AK", 31,"CA"),(3,"BK", 15,"KT"),(4,"CK",10,"NY")).toDF("policyId", "name", "premium", "state")
2019-03-15 20:25:23 WARN  ObjectStore:568 - Failed to get database global_temp, returning NoSuchObjectException
df1: org.apache.spark.sql.DataFrame = [policyId: int, name: string ... 2 more fields]

scala> val df2 = Seq(("VK", "IL", 1,30),("AK","IL", 2,30),("NK", "IL", 11,30),("DK", "IL", 12,30)).toDF("name", "state", "policyId", "premium")
df2: org.apache.spark.sql.DataFrame = [name: string, state: string ... 2 more fields]

scala> df1.show90
<console>:26: error: value show90 is not a member of org.apache.spark.sql.DataFrame
       df1.show90
           ^

scala> df1.show()
+--------+----+-------+-----+
|policyId|name|premium|state|
+--------+----+-------+-----+
|       1|  VK|     30|   IL|
|       2|  AK|     31|   CA|
|       3|  BK|     15|   KT|
|       4|  CK|     10|   NY|
+--------+----+-------+-----+


scala> df2.show()
+----+-----+--------+-------+
|name|state|policyId|premium|
+----+-----+--------+-------+
|  VK|   IL|       1|     30|
|  AK|   IL|       2|     30|
|  NK|   IL|      11|     30|
|  DK|   IL|      12|     30|
+----+-----+--------+-------+


scala> import org.apache.spark.sql.functions._
import org.apache.spark.sql.functions._

scala> val df1 = sc.parallelize(List(
     |   (50, 2),
     |   (34, 4)
     | )).toDF("age", "children")
df1: org.apache.spark.sql.DataFrame = [age: int, children: int]

scala> )).toDF("age", "children")
<console>:1: error: illegal start of definition
)).toDF("age", "children")
^

scala> import spark.implicits._
import spark.implicits._

scala> val a = (1,"12",1,333)
a: (Int, String, Int, Int) = (1,12,1,333)

scala> val b = (1,"",3,989)
b: (Int, String, Int, Int) = (1,"",3,989)

scala> val c = (7,"98",8,878)
c: (Int, String, Int, Int) = (7,98,8,878)

scala> val df1 = List(a).toDF("ID","Name","ID2","Marks")
df1: org.apache.spark.sql.DataFrame = [ID: int, Name: string ... 2 more fields]

scala> val df2 = List(b, c).toDF("ID","Name","ID2","Marks")
df2: org.apache.spark.sql.DataFrame = [ID: int, Name: string ... 2 more fields]

scala> df1.show()
+---+----+---+-----+
| ID|Name|ID2|Marks|
+---+----+---+-----+
|  1|  12|  1|  333|
+---+----+---+-----+


scala> df2.show()
+---+----+---+-----+
| ID|Name|ID2|Marks|
+---+----+---+-----+
|  1|    |  3|  989|
|  7|  98|  8|  878|
+---+----+---+-----+


scala> df1.union(df2)
res5: org.apache.spark.sql.Dataset[org.apache.spark.sql.Row] = [ID: int, Name: string ... 2 more fields]

scala> df3=df1.union(df2)
<console>:35: error: not found: value df3
val $ires9 = df3
             ^
<console>:33: error: not found: value df3
       df3=df1.union(df2)
       ^

scala> val df3=df1.union(df2)
df3: org.apache.spark.sql.Dataset[org.apache.spark.sql.Row] = [ID: int, Name: string ... 2 more fields]

scala> df3.show()
+---+----+---+-----+
| ID|Name|ID2|Marks|
+---+----+---+-----+
|  1|  12|  1|  333|
|  1|    |  3|  989|
|  7|  98|  8|  878|
+---+----+---+-----+


scala> val df11 = Seq((1,"VK",30,"IL"),(2, "AK", 31,"CA"),(3,"BK", 15,"KT"),(4,"CK",10,"NY")).toDF("policyId", "name", "premium", "state")
df11: org.apache.spark.sql.DataFrame = [policyId: int, name: string ... 2 more fields]

scala>

scala> val df12 = Seq(("VK", "IL", 1,30),("AK","IL", 2,30),("NK", "IL", 11,30),("DK", "IL", 12,30)).toDF("name", "state", "policyId", "premium")
df12: org.apache.spark.sql.DataFrame = [name: string, state: string ... 2 more fields]

scala> val 13=df11.union(df12)
<console>:33: error: type mismatch;
 found   : Int(13)
 required: org.apache.spark.sql.Dataset[org.apache.spark.sql.Row]
       val 13=df11.union(df12)
           ^

scala> val df13=df11.union(df12)
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


scala> df11.select(("policyId", "name", "premium", "state").show()
     |
     | ;
<console>:3: error: ')' expected but ';' found.
;
^

scala> df11.select(("policyId", "name", "premium", "state").show()
     | df11.select("policyId", "name", "premium", "state").show();
<console>:2: error: ')' expected but '.' found.
df11.select("policyId", "name", "premium", "state").show();
    ^

scala> df11.select("policyId", "name", "premium", "state").show();
+--------+----+-------+-----+
|policyId|name|premium|state|
+--------+----+-------+-----+
|       1|  VK|     30|   IL|
|       2|  AK|     31|   CA|
|       3|  BK|     15|   KT|
|       4|  CK|     10|   NY|
+--------+----+-------+-----+


scala> df12.select("policyId", "name", "premium", "state").show();
+--------+----+-------+-----+
|policyId|name|premium|state|
+--------+----+-------+-----+
|       1|  VK|     30|   IL|
|       2|  AK|     30|   IL|
|      11|  NK|     30|   IL|
|      12|  DK|     30|   IL|
+--------+----+-------+-----+


scala> val df14 = df11.join(df12, Seq("policyId"))



val df24 = df21.join(df22, Seq("policyId"))

df31.join(df32, Seq("policyId"), "full_outer").show()
