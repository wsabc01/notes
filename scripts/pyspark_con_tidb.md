# Use Python/Pyspark Connect TiDB Example

### use mysql.connector

````
db = mysql.connector.connect(user="root", passwd="", database="test",host="172.16.10.64",port="5000")

    cursor = db.cursor()
    try:
        cursor.execute(sql)
        results = cursor.fetchall()
        for row in results:
            print(row)
    except:
        print "Error: unable to fecth data"
````

### use pytispark read
````
from pyspark.sql import SparkSession
import pytispark.pytispark as pti

spark = SparkSession.builder.master("local[2]").appName("testPy").getOrCreate()
ti = pti.TiContext(spark)
ti.tidbMapDatabase("test",False,True)

df = spark.sql(sql)
df.show()
````

### use pyspark jdbc read
````
from pyspark.sql import SparkSession
import pytispark.pytispark as pti

spark = SparkSession.builder.master("local[2]").appName("testPy").getOrCreate()
sql = "(" + sql + ") t"
df = spark.read.format('jdbc').options(
     url='jdbc:mysql://172.16.10.64:5000/test',
     dbtable=sql,
     user='root',
     password='',
     useSSL="false"
     ).load()
df.show()
````

### use pyspark jdbc write
````
from pyspark.sql import SparkSession
import pytispark.pytispark as pti

df.write.mode("append").format("jdbc").options(
    url='jdbc:mysql://172.16.10.64:5000/test',
    user='root',
    password='',
    dbtable="insertTable",
    batchsize="100",
    isolationLevel= "NONE",
).save()
````
