# Spark SQL Cheat sheet
The Spark SQL module consists of two main parts. The first one is the representation of the Structure APIs, called DataFrames and Datasets, that define the high-level APIs for working with structured data. The DataFrame concept was inspired by the Python pandas DataFrame; the main difference is that a DataFrame in Spark can handle a large volume of data that is spread across many machines. The second part of the Spark SQL module is the Catalyst optimizer, which does all the hard work behind the scenes to make your life easier and to speed up your data processing logic. One of the cool features the Spark SQL module offers is the ability to execute SQL queries to perform data processing.
The figure below shows how the Spark SQL component is built on top of the good old reliable Spark Core component.

![Spark SQL Components](SparkSQLComponents.png)
	
## DataFrames
A DataFrame is an immutable, distributed collection of data that is organized into rows, where each one consists a set of columns and each column has a name and an associated type. In other words, this distributed collection of data has a structure defined by a schema. If you are familiar with the table concept in a relational database management system (RDBMS), then you will realize that a DataFrame is essentially equivalent. Each row in the DataFrame is represented by a generic Row object.

### Creating DataFrames
DataFrames can be created by reading data from the many structured data sources as well as by reading data from tables in Hive and databases.
In addition, the Spark SQL module makes it easy to convert an RDD to a DataFrame by providing the schema information about the data in the RDD. The DataFrame APIs are available in Scala, Java, Python, and R.

#### Creating Dataframes from RDDs
Let’s start with creating a DataFrame from an RDD.
- Creating a DataFrame from an RDD of Numbers
```
import scala.util.Random
val rdd = spark.sparkContext.parallelize(1 to 10).map(x => (x, Random.nextInt(100)* x))
val kvDF = rdd.toDF("key","value")
```
Another way to create a DataFrame is by specifying an RDD with a schema that is created programmatically.
- Creating a DataFrame from an RDD with a Schema Created Programmatically
```
import org.apache.spark.sql.Row
import org.apache.spark.sql.types._
val peopleRDD = spark.sparkContext.parallelize(Array(Row(1L, "John Doe",  30L),
                                                     Row(2L, "Mary Jane", 25L)))
val schema = StructType(Array(
        StructField("id", LongType, true),
        StructField("name", StringType, true),
        StructField("age", LongType, true)
))
val peopleDF = spark.createDataFrame(peopleRDD, schema)
```
#### Creating Dataframes from a Range of Numbers
Spark 2.0 introduced a new entry point for Spark applications. It is represented by a class called SparkSession, which has a convenient function called range that can easily create a DataFrame with a single column with the name id and the type LongType. This function has a few variations that can take additional parameters to specify the start and end of a range as well as the steps of the range.
- Using the SparkSession.range Function to Create a DataFrame
```
val df1 = spark.range(5).toDF("num").show

val df2 = spark.range(5,10).toDF("num").show

val df3 = spark.range(5,15,2).toDF("num").show // The last param represents the step size
```
Notice the range function can create only a single-column DataFrame.
One option to create a multicolumn DataFrame is to use Spark’s implicits that convert a collection of tuples inside a Scala Seq collection.
- Converting a Collection Tuple to a DataFrame Using Spark’s toDF Implicit
```
val movies = Seq(("Damon, Matt", "The Bourne Ultimatum", 2007L),
                 ("Damon, Matt", "Good Will Hunting", 1997L))
val moviesDF = movies.toDF("actor", "title", "year")
```

#### Creating DataFrames from Data Sources







































