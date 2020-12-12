* Scala:
  * **Sca**lable **la**nguage
  * object-oriented programming (OOP) + functional programming (FP)
  * OOP:
    * Every value is an object
    * Every operation is a method call
  * FP:
    * Scala is a functional programming language
    * functions are first-class values
    * map input to output
  * complied to Java bytecode, run on JVM
    * Scala runs on JVM and can use Java library directly
* REPL: **R**ead-**E**valuate-**P**rint-**L**oop
  * 就是指 interpreter 的互動式介面
* sbt:
  * simple build tool
  * scala 的編譯器
* Scala interpreter
  * 在 terminal 中輸入 `scala` 可以進入 Scala interpreter
  * 在 scala interpreter 中輸入 `:q` 可以離開 Scala interpreter
  * 在 scala interpreter 中輸入 `println("想輸出的東西")` 會把結果輸出到螢幕上，注意 scala 要用雙引號
  * 在 terminal 中輸入 `scala filename.scala` 就可以執行 script `filename.scala`
  * 在 terminal 中輸入 `scalac filename.scala` 就可以編譯 `filename.scala`
    * 編譯後的檔案就叫做 `filename` 在 terminal 中輸入 `scala filename` 執行
* Scala 有兩種變數宣告的方式
  * `var name : type = value` var 就是 variable 是在宣告後可以改變數值的
  * `val name : type = value` val 就是 value 代表 constant 宣告後就不能改變
  * Scala 的變數型態有: `Double` (64-bit), `Float`, `Long`, `Int` (32-bit, singed), `Short`, `Byte`, `Char` (16-bit unsinged unicode), `String` (其實是 sequence of `Char`), `Boolean` (小寫的 `true` 或 `false`), `Unit`
    * 對應的 Java type
    
    |Scala type|Java type|
    |:---|:---|
    |scala.Double|Java.lang.Double|
    |scala.Float|Java.lang.Float|
    |scala.Long|Java.lang.Long|
    |scala.Int|Java.lang.Integer|
    |scala.Short|Java.lang.Short|
    |scala.Byte|Java.lang.Byte|
    |scala.Char|Java.lang.Character|
    |scala.Boolean|Java.lang.Boolean|
    |scala.Unit||

* Scala function:
  * 定義 function
  ```scala
  def func_name(param : param_type) : func_return_type = {
      func_body
  }
  ```
  * function 的種類
    * method: 類別的函式, trait, singleton object
    * local function: 在別的函式內部定義的函式
    * procedure: 回傳值型態是 `Unit` 的函式
    * function literal: 匿名函式，執行期會被初始化成 function values 物件
* Collections:
  * collection 有分成 mutable 和 immutable 兩種
    * mutable: Array
    * immutable: List
  * Array: 
    * 元素的型態要相同，但是如果元素的型態用 Any 那元素可以是不同型態的
    * 宣告 Array
    ```scala
    val arr1 = Array(elem1, elem2, elem3)
    val arr2 = new Array[元素的型態](元素的個數) # arr2 是空的 array
    val arr3 : Array[元素的型態] = new Array[元素的型態](元素的個數) # arr3 是空的 array
    arr3(0) = elem1
    arr3(1) = elem2
    arr3(2) = elem3
    ```
    * 把 Array 指定給 var 或 val 都行，差別在於 
      * `var arr = Array(elem1, elem2, elem3)` 中 `arr` 是變數，可以指向其他的物件 
      * `val arr = Array(elem1, elem2, elem3)` 中 `arr` 是常數，就不能指向其他物件
  * List:
    * 元素的型態要相同
    * 宣告 List
    ```scala
    val li = List(elem1, elem2, elem3)
    ```
    * 有許多函式，例如 `drop()`, mkString(), `length`, `reverse`
    * `::` 是 prepend 的意思，`new_elem :: li` 會在原先的 list 中最前面加上 new_elem 變成一個新的 List(new_elem, elem1, elem2, elem3) 後傳回
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
