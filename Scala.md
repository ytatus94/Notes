* REPL: **R**ead-**E**valuate-**P**rint-**L**oop
* Scala runs on JVM and can use Java library directly
  * Scala is a functional programming language
* Distribution system allows fault tolerance
* HDFS: **H**adoop **D**istributed **F**ile **S**ystem
  * HDFS uses 128 MB blocks of data and replicates data blocks 3 times
  * Name Node: master
  * Data Node: slave
* MapReduce allows computations on data and splits a computation task to a distributed set of files
  * MapReduce writes the data to disk after each map and reduce operation
  * Job tracker: sends code to run on the trak tracker
  * Task tracker: allocate CPU and RAM for tasks and monitor the task on worker nodes
* Spark is a flexible alternative to MapReduce
  * Spark can use data stored on distribution system, such as Cassandra, AWS S3, HDFS etc
  * SPark keeps the data in memory after each transformation, 100x faster
  * Spark 2.0 uses DataFrame based syntax
* RDD: **R**esilient **D**istributed **D**ataset
  * RDD are immutable, lazily evaluated, cacheable
  * Two type of RDD operations: transformations and actions
* Start spark-shell `bash>spark-shell` and exit spark-shell `spark>:q`


division is integer division
math.pow(4, 2)
val <name>:<type> = <literal>
var <name>:<type> = <literal>
val: values are immutable
var: variables can be reassigned, but must use the same data type
Int, Double, String, Boolean
Scala can infer the data type
string must use double quote
If there is a dot in the <name> like val my.string = "hello" doesn't work but val `my.string` = "hello"
true false
println("some strings")
val str = "str1" + "str2"
val str = "str1" * 5
str. press tab see all available methods
val str = s"Hello ${str1}", val str = f"Hello $str2" can use {} or don't use, must add $ in front of variable, s-string or f-string
printf("some formated string", arguments)
str.charAt(0)
str.indexOf("A")
str slice (start index, end index not including)
str matches "some other string"
str contains "some other string"
tuple can put different data type, can nested (1, 2.2, "hello", true, (2, 3))
tuple._3 tuple index string from 1
