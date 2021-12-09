# Spark
* 在 terminal 中輸入 `spark-shell` 可以進入 spark shell，要退出用 `:q`
* Spark 不包含 storage，它使用既有的 storage 技術像是 HDFS, Amazon S3 等等
  * Hadoop = HDFS + MapReduce
* Spark 也有用到 MapReduce 的概念 (map, reduce, shuffle)
  * Spark 的目標是要取代 Hadoop 中的 MapReduce implementation (而不是取代 Hadoop 本身)
* Spark 有自己的 resource manager 但是也可以用其他服務提供的 (例如用 YARN)
  * Hadoop 是用 YARN 來做 resource management 
* Hadoop 在做 iterative algorithm 和 interactive data mining 的時候很慢，因為每一步驟結束後都會把資料存回到硬碟上
  * Spark 藉由把資料保存在記憶體中，所以省掉了讀取硬碟所需的時間，因而加快速度
* Spark 提供 in-memory computing at distributed scale
* Spark execution eigine: spark 把要做的步驟 (就是每一步的 command) 先轉成 logical plan 之後再建立 physical plan，最後把 task 送到 spark cluster 上去執行
* RDD: Resilient Distributed Datasets
  * 有 fault tolerance 
  * RDD is a collection of elements
  * RDD is immutable, once RDD is created, you cannot change the elements inside the RDD
* RDD properties:
  * list of partitions
    * block of data 叫做 partition
  * compute functions
    * Function you apply on the RDD will be applied to all the elements in the RDD
    * Functions 分成兩類
      * Transformation: 把一個 RDD 轉換成另一個 RDD
      * Action: 建立 logical plan
      * Spark 是 lazy evaluation，遇到 transformation functions 時只檢查語法，遇到 action functions 時才會建立 logical plan
  * list of dependencies   
* Spark 能加快執行的速度是因為
  * in-memory computing
  * spark execution eigine
  * RDD
  * spark articheture
* sc 表示 Spark context 是做為 getway to Spark cluster
* 128 MB block size
* Spark 有兩種 dependency
  * Narrow dependency
    * Depend on everything
    * Child partition depends on entire parent partition
    * 用來建立 task
  * Wide dependency
    * Depend on portion 
    * Child partition depends on a portion of each of the parent partition
    * Wide dependency 會被當成 task execution bounday 用來區分不同的 stage, i.e. 用來建立 stage
  * Type of dependency
    * Determine number of task
    * has an impact on fault tolerance 
* 啟動一個 spark shell 就會分派一個 application id 給它
  * 每一個 application id 可以執行很多 jobs
    * 一個 job 包含很多 stages, stages 之間以 wide dependency 當作邊界
      * 一個 stage 可以有很多 tasks
  *  spark shell 存取 driver，當遇到 action function 時，spark context (sc) 就會把所有的 logical plan 丟給 DAG scheduler，DAG scheduler 把 logical plan 翻譯成 physical plan 然後餵給 Task scheduler，Task scheduler 就把 tasks 丟到 executor 上執行，執行的結果再一路回傳到 driver
    * DAG: Directly acyclic graph，是一種 execution style，acyclic 就是說當執行 stage N 的時候就不會再回到 stage N-1 了
    * DAG scheduler 和 Task scheduler 都是在 Driver 上
    * plysical plan 包含了 stages 和每個 stage 裡面的 tasks，就是告訴 spark 要怎麼執行
    *   



* Spark dataframe 或是 RDD 都是 immutable 的，一但建立後就無法更改
* Spark dataframe 有 lazy evaluations 的特性，可以對 dataframe 執行 transformations 但是實際上不會動作，直到 action 時才會真的去執行。
* 建立 SparkContext
```python
from pyspark import SparkContext
sc = SparkContext()

sqlContext = SQLContext(sc) # 順便建立 SQL Context
```
* 讀檔時使用 `inferSchema = True` 可以自動判別欄位的資料型態
* `.printSchema()` 就類似 `df.info()`
* 有 `.head(n)` 但是更常用 `.show(n)`
* `.count()` 計算有多少 rows，`.columns` 顯示 column 的名字
* `.describe().show()` 就像是 pandas 的 `df.describe()`
* `.select(col1, col2, ...)` 選出要使用的 columns, 用 `select(col1, col2, ...).distinct()` 會只把 columns 選出 distinct 的 rows
* `..subtract()`
* `.crosstab()`
* `.dropDuplicates()`
* `.dropna(how='any'/'all', subset=[col1, col2, ...])`
* `.fillna(val)` or `.fillna({col1:val1, col2:val2, ...})`
* `.filter(條件)` 跟 where() 一樣
* `.groupby(col).agg({col:func, ...})`, `.groupby(col1, col2, ...).func()` 其中 func() 可以是 sum(), min(), max(), count()
* `.sample(withReplacement, fraction, seed)`
* `.map(func)` 結果會是 RDD
* `.orderBy(col.desc())`
* `.withColumn(new_col, 算式)`
* `.drop(col)`
---
# Spark SQL

# Spark MLlib


* 常用的 pySpark import
```python
from pyspark.sql import SparkSession # 這個是進入點，SparkConf, SparkContext, SQLContext 都已經被包含在 SparkSession 裡面了
from pyspark import SparkConf, SparkContext # 這個是在 SparkSession 推出來以前所使用的舊的方式
from pyspark import HiveContext # 需要用到 Hive 時使用的
from pyspark.sql import Row
from pyspark.sql.types import * # pyspark 的數據類型
from pyspark.sql.functions import *
from pyspark.sql import functions as F
import pyspark.sql.functions as F
from pyspark.sql.functions import col
from pyspark.sql.functions import isnull, isnan 
from pyspark.sql.functions import concat, concat_ws, format_string, instr, substring, regexp_extract, regexp_replace, repeat, split # 字串處理時使用的
from pyspark.sql.functions import udf # 要用到 user defined functions 時使用的
```
* Spark 是 lazy 的，分成了 transformation 和 action
  * transformation 是指明要做什麼動作，但還不會去執行動作
    * 像是 `filter`, `map`, `flatMap`, reduce` 是 transformation
  * action 才是真正會去執行動作
    * 像是 `show()`, `collect()`, 'count()`, `first()`, `head()`, `take()` 是 action
* Spark 的 dataframe 是 immutable 的，一但建立了就不能再改變
  * 如果要改變 dataframe 的話，只能建立新的
* 設定 Spark 的進入點，這是在 import library 後第一個要執行的東西
```python
# 新的方式用 SparkSession
spark = SparkSession.builder.appName("輸入些什麼").getOrCreate()
spark = SparkSession.builder.master("local[2]").appName("輸入些什麼").enableHiveSupport().getOrCreate()
# 舊的方式用 SparkContext
sc = SparkContext()
sc = SparkContext('local', '輸入點什麼')
# 用 SparkSession 來存取 SparkContext
sc = spark.sparkContext
```
* pyspark 常用的數據型態有
```python
StructType() # 結構 
StructField() # 結構中的元素
LongType() # 長整數 
StringType() # 字串 
IntegerType() # 整數
FloatType() # 浮點數
```
* 定義 Spark dataframe 的數據結構
  * 第三個元素是說該欄位是否允許有 null (i.ei nullable)
```python
schema = StructType([
    StructField("col1", 數據型態, True),
    StructField("col2", 數據型態, True),
    ...
])
```
* 建立 Spark dataframe
```python
# 把 SQL query 的結果直接轉成 Spark dataframe
df = spark.sql("SQL query 敘述")
# 由 Row 物件來建立，並配合 toDF()
row = Row('欄位名 1', '欄位名 2', ...)
df = sc.parallelize([row(第一列欄位 1 的值, 第一列欄位 2 的值, ...), 
                     ..., 
                     row(第 n 列欄位 1 的值, 第 n 列欄位 2 的值, ...)]).toDF() # 這邊其實是建立 RDD 再把 RDD 轉成 Spark dataframe
# 由 RDD 來建立
spark_RDD = spark.sparkContext.parallelize(
    [(row1_col1, row1_col2, ...),
     (row2_col1, row2_col2, ...),
     ...
    ]
)
table_schema = StrucType(
    [StrucField('col1', type, nullable),
     StrucField('col2', type, nullable),
     ...
    ]
)
df = spark.createDataFrame(spark_RDD, table_schema)
df = spark_RDD.toDF(table_schema)

createDataFrame(data,schema) ,其中data可以是list,rdd，pandas.DF ;schema可以是变量名列表，显示定义的类型以及默认（自行推断）

# 由 list 來建立
list_name = [(raw1_val1, raw1_val2, raw1_val3, ...),
             (raw2_val1, raw2_val2, raw2_val3, ...),
             (raw3_val1, raw3_val2, raw3_val3, ...),
             ...]
df = spark.createDataFrame(list_name, schema=['col1', 'col2', 'col3', ...]) # schema 可以用 list 或是用 tuple
# 用 tuple 來建立
df = spark.createDataFrame((
      (row1_val1, row1_val2, ...),
      (raw2_val1, raw2_val2, ...),
      (raw3_val1, raw3_val2, ...)
    )).toDF("欄位 1", "欄位 2", ...)
```
* 顯示 Spark dataframe 的內容
```python
df.show() # 預設顯示前 20 rows
df.show(n) # 顯示前 n rows
df.show(truncate=False) # 不截斷數據顯示，適合有很長的數據時使用
```
* 顯示 Spark dataframe 每個欄位的名稱與型態的資訊
```python
df.printSchema()
```
* 顯示 Spark dataframe 共有幾個 rows
```python
df.count()
```
* 顯示 Spark dataframe 的欄位名字
```python
df.columns
```
* 顯示 Spark dataframe 頭幾 rows 的資料 (會從 work nodes 把資料抓回 local)
```python
df.head(n) # 前 n rows
df.take(n) # 前 n rows
```
傳回的結果是一個元素是 Row 的 list 而, 長得像是這樣
```python
[Row(欄位 1 = 值, 欄位 2 = 值, ...), Row(欄位 1 = 值, 欄位 2 = 值, ...), ...]
```
所以要取回第 i 個 Row 時，要用 `list[i]`，而一個 Row 物件裡面其實是每個欄位在該 row 的值，所以要存取數值的話要用 `list[i][j]` 就是第 i row 第 j column 的數值
* 把全部的數據從 work nodes 抓回 local
```python
df.collect()
```
* 顯示 Spark dataframe 的統計資訊
```python
df.describe().show() # 顯示全部欄位的，記得要加上 show()
df.describe('欄位').show() # 只顯示某個欄位的
```
* 去除重複的
```python
df.select('欄位').distinct().show()
df.dropDuplicates()
df.dropDuplicates(subset=['col']) # 只刪除某欄位的重複值
```
* 選擇欄位的方式有下面幾種
```python
df["欄位"]
df.欄位
df.select('欄位1', df[‘欄位2’], df.欄位3, df[‘欄位4’]的運算, ...)
df.where("欄位的條件判斷" )
```
* 用 `orderBy()` 排序，預設是遞增排序
```python
df.orderBy(df.欄位).show() 
df.orderBy(df['欄位'].desc()).show() # 改成遞減排序
```
* 用 `sample()` 抽樣，用 `withReplacement` 決定要不要放回去，第二個參數是抽樣的比例，第三個參數是 seed
```python
df.sample(withReplacement=False, 0.2, 42)
```
* 新增欄位
```python
df.withColumn('欄位', F.lit(值)) # 這樣整個欄位都會有相同的值
df.withColumn('欄位', 依照其他欄位做的計算式)
```
* 把欄位改名
```python
df.withColumnRenamed('舊名字', '新名字')
```
* 把 Spark dataframe 以上下的方式接起來，要相接的 dataframe 要有相同的欄位
```python
df1.union(df2)
```
* 用 `join()` 合併欄位:
  * `join()` 預設是使用 inner join
  * 若 join 兩個的表格有相同的欄位名字時，join 產生的結果就會有重複的欄位名字
    * pandas 會在重複的欄位名字後面加上 `_x, _y` 等，但是 Spark 不會
```python
df = df1.join(df2, df1.col1==df2.col2, how='left')\ # 注意兩個 dataframe 用來連接的 key 不同名字時，是用 ==
        .join(df3, on=['col3', 'col4'], how='outer') # 如果兩個 dataframe 用來連接的 key 同名字時可以直接用 on=['key1', 'key2', ...]
```
* 兩個 Spark dataframes 的聯集，交集，差集
```python
df1.select('欄位').union(df2.select('欄位')) # 聯集，就是兩個 dataframes 全部的資料
df1.select('欄位').intersect(df2.select('欄位')) # 交集，兩個 dataframes 共同的資料
df1.select('欄位').subtract(df2.select('欄位')) # 差集，把 df2 中和 df1 中相同的部分自 df1 中移除
```
* 對某個欄位做運算產生新的欄位，常用在把欄位拆成幾個部分，運算的結果放在新的欄位中
```python
df.explode('欄位', '新欄位'){x: 欄位型態 => 對 x 做的運算} # x 是表示原本的欄位的數值
```
* 對字串欄位做處理
  * 字符就是一個字母或數字，字串是好多個字元或數字組成的
```python
# 把兩個字串欄位接在一起，變成新的欄位，結果是 str_col1str_col2
df.select(F.concat(df.str_col1, df.str_col2).alias('str_col3')).show()
# 把兩個字串欄位用連接符號接在一起，變成新的欄位，結果是 str_col1連接符號str_col2
df.select(F.concat_ws('連接符號', df.str_col1, df.str_col2).alias('str_col3')).show()
# 格式化輸出字串欄位
df.select(F.format_string('%d %s', df.str_col1, df.str_col2).alias('str_col3')).show()
# 查詢字符的位置，傳回的結果是 index + 1，也就是說從 1 開始算的
df.select(F.instr(df.str_col1, '要查詢的字符').alias('str_col2')).show()
# 字符取代
df.select(substring(df.str_col1, 要被取代的字符, 用來取代的字符).alias('str_col2')).show()
# Regular expansion
df = spark.createDataFrame([('100-200',)], ['str'])
df.select(regexp_extract('str', '(\d+)-(\d+)', 1).alias('d')).show()
# '100'

df = spark.createDataFrame([('foo',)], ['str'])
df.select(regexp_extract('str', '(\d+)', 1).alias('d')).show()

df = spark.createDataFrame([('100-200',)], ['str'])
df.select(regexp_replace('str', '(\\d+)', '--').alias('d')).collect()
```
* 使用 user defined functions (UDF)
https://blog.csdn.net/crazybean_lwb/article/details/87006752?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-7.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-7.channel_param
  * 比較簡單的函數可以用 lambda function
  * 比較複雜一點的函數可以自己定義一個
    * 先定義一個 python function
    * 把定義好的 python function 註冊到 udf 裡面
    * 然後就能在 pySpark 中使用 udf
```python
# 用 lambda function
my_udf = F.udf(lambda x, y: do_something_for_x_and_y)
df.withColumn('new_col', my_udf(df.col1, df.col2)).show()
# 用自己定義的函數
def my_function(x):
    return something
my_function_UDF = udf(my_function, my_function 的回傳值的型態)
df.withColumn('new_col', my_function_UDF('col')).show()
```
* PySpark 統計
https://blog.csdn.net/suzyu12345/article/details/79673557?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.channel_param


* PySpark 日期函數
https://blog.csdn.net/suzyu12345/article/details/79673569?utm_medium=distribute.pc_relevant.none-task-blog-title-4&spm=1001.2101.3001.4242

* PySpark functions
https://blog.csdn.net/qq_40176087/article/details/100110804?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-10.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-10.channel_param


* 用 `groupBy()`
  * 可用的 aggregation functions 有 `avg('cols')`, `count()`, `max('cols')`, `min('cols'), `mean('cols')`, `sum('cols')`
```python
# 對全部欄位做一樣的 aggregation function
df.groupBy('欄位名').count().show()

# 對個別欄位做不同的 aggregation functions
df.groupBy('欄位名1').agg({'欄位名2': '函數名'})
import pyspark.sql.functions as F
df.groupBy('欄位名1').agg(F.max('欄位名2'), F.sum('欄位名3')).show()

```
* Spark 的設定
  * 增加 spark 記憶體
```bash
set by SparkConf:  conf.set("spark.driver.maxResultSize", "3g")
set by spark-defaults.conf  spark.driver.maxResultSize 3g
set when callingspark-submit    --conf spark.driver.maxResultSize=3g
```
  * 啟用 Arrow
  `spark.conf.set("spark.sql.execution.arrow.enabled", "true")`
  也可以在conf/spark-defaults.conf文件中写入：spark.sql.execution.arrow.enabled=true 
  
* Missing value
```python
# 刪除有 missing 的 column
df = df1.na.drop()
df = df1.na.drop(thresh=n) # 該欄位內有超過 n 個 missing value 時才要刪除欄位

# missing value imputation
df = df1.na.fill(value) # 全部的 missing value 都用 value 取代
df = df1.na.fill({'col1': val1, 'col2': val2, ...}) # 不同的欄位用不同的 value 取代

# 用條件判斷來填補 missing value
np.where(isnull(a), b, a) # 如果 a 是空，就填上 b 否則叫就繼續用 a, numpy 的 where 就是 if-else 敘述
a[:-n].combine_first(b[n:]) # 如果 a 中指定的部分是空，就填上 b 中指定的不份
df1.combine_first(df2) # 就是用 df2 中的數值來填補 df1 中的 missing value

# pyspark
df = spark.createDataFrame([(1.0, float('nan')), (float('nan'), 2.0)], ("a", "b"))
df.select(nanvl("a", "b").alias("r1"), nanvl(df.a, df.b).alias("r2")).show()
```
* `collect()` 可以把數據傳回到 driver node 上，傳回的結果是 Row 物件，用 `collect()[0]` 得到 Row 的值，但實際上要獲得數值要用 `collect()[0][0]`
* Spark dataframe 中有兩種空值，一個是數值型態的 nan 另一個是普通的 None 要判斷空的時候兩個都要判斷
```python
from pyspark.sql.functions import isnull, isnan
df = spark.createDataFrame([(1, None), (None, 2), (3.0, float('nan')), (float('nan'), 4.0)],
                           ('a', 'b', 'c', 'd'))
df.select(isnull('a').alias('a1'), isnull(df.b).alias('b1'), isnan('c').alias('c1'), isnan(df.d).alias('c2')).show()
```
* 統計計算
```python
# mean
mean_value = df.select(F.mean('col')).collect()[0][0]
# deviation
deviation = df.select( (df.col - mean_value)**2).alias('deviation) )
# standard error
std = math.floor(math.squart(deviation.groupBy().avg('deviation').first()[0])) # 自己手算
std = df.select(F.stddev('col')).collect()[0][0] # 直接用統計函數來算
```
* 去重

* Spark dataframe 和 Pandas dataframe 互相轉換
```python
# Spark dataframe 變成 Pandas dataframe
pandas_df = spark_df.toPandas()
# Pandas dataframe 變成 Spark dataframe
spark_df = sqlContext.createDataFrame(pandas_df)
spark_df = spark.createDataFrame(pandas_df)
spark_df = spark.createDataFrame(pandas_df.values.tolist(), list(pandas_df.columns))
```
* Spark dataframe 和 Spark RDD 互相轉換
  * RDD 要變成 dataframe 的話，RDD 的類型必須是 Row
```python
rdd_df = spark_df.rdd # Spark dataframe 變成 RDD
spark_df = rdd_df.toDF() # RDD 變成 Spark dataframe 
```
* 讀檔到 Spark 裡面
```python
# 從 Hive 上讀檔案
sc = SparkContext
hive_context = HiveContext(sc)
parquet_data = hive_context.read.load(檔案在 HDFS 上的絕對路徑)
csv_data = hive_context.read.csv(檔案在 HDFS 上的絕對路徑, header=True)
```

CSV
df = spark.read.csv('../data/rating.csv', sep = ',', header = True) #自动判断格式interSchema=True

df.select('欄位 1', '欄位 2', ...).show()
df.select('欄位 1', '欄位 2', ...).distinct().show() # 會去重

* 過濾數據 `filter()` 或是 `where()`
```python
df.filter(df.col == '某個值').show()
df.filter((df.col1 == '某個值') & (df.col2 == '某個值') & ...).show() # 有好幾個條件時
```
* 刪除欄位
df.drop(col)
* 把 Spark dataframe 存到 table 中
df.write.mode('overwrite').saveAsTable('table_name')
* 緩存 Spark dataframe
df.persis()
df.cache()
* 聚合運算
df.agg(某函數(col))
* 取別名
df.col.alias('新名字')
* 隨機劃分 Spark dataframe
df.randomSplit()

df.registerTempTable('table_name') 可以用 spark.sql('SQL Query').show() 來查詢資料

df.where(F.col('欄位').between(數值1, 數值2))

from pyspark.sql.functions import monotonically_increasing_id
df1 = df.withColumn('index', monotonically_increasing_id()) # 添加一個欄位當索引
df1.select(df1.col1, df1.index.between(數值1, 數值2)).show()

df.drop(how='any or all', thresh=某個整數, subset=[要被丟掉的欄位們]
df.na.fill(value, subset=[要填補的欄位們]) value 可以是 {'col1': 要填的值, 'col2': 要填的值, ...}
df.replace(要被替換的值, 要替換成什麼值, subset=[要被替換的欄位們]
如果要被替換的是 {'col1': val1, 'col2': val2} 就不需要有第二格參數
如果要被替換的是 list 那要替換成什麼也必須是 list 且長度一樣，就是一對一的替換

df.createGlobalTempView("temp_table")
df_anothor=spark.sql("select * from global_temp.temp_table")
df_local=spark.sql("select * from temp_table")
spark.catalog.dropGlobalTempView("temp_table")
createOrReplaceTempView()
createTempView() 
spark.catalog.dropGlobalTempView() 
spark.catalog.dropTempView()

df1.crossJoin(df2)
相关系数&协方差（corr,cov）
DF.corr(col_name1,col_name2)
DF.cov(col_name1,col_name2)

列联表 crosstab: 作用: 统计某一个变量（y）各类中，制定特征x的分布。
y 等於某個值的時候 x 等於某個值的有幾個
df.crosstab("y","x").show()

ratings = spark.read.load("/FileStore/tables/u.data",format="csv", sep="\t", inferSchema="true", header="false")

如果想對 RDD 用 UDF，就只要用 python 的方式定義出函數 row_wise_function() 再利用 map() 就可以了
new_RDD = old_RDD.map(lambda row: row_wise_function(row))



ratings.registerTempTable('ratings_table')


* 查看分區數目
df.rdd.getNumPartitions()

* 讀寫檔案
```python
# 讀 CSV
df = spark.read.csv(csv 檔案的路徑, header=True, inferSchema=True, sep=',')
# 讀 parquet
df = spark.read.parquet(parquet 檔案的路徑)
# 讀 json
df = spark.read.json(json 檔案的路徑)
# 讀 txt
df = spark.read.text(txt 檔案的路徑)
# 讀 Oracle 或是 MySQL
# 要把 mysql-jar 放到 spark-version-bin-hadoop2.7/jars 裡面
df = spark.read.format('jdbc')\
          .options(url='jdbc:oracle:thin:@ip:port:database', # 或是 'jdbc:mysql://ip'
                   dbtable='table_name or SQL Query',
                   user='user_name',
                   password='password')\
          .load()
# 讀 Hive
from pyspark.sql import SparkSession
spark = SparkSession.builder.enableHiveSupport().master('ip:port').appName('輸入點什麼').getOrCreate()
df = spark.sql('用 SQL query 查詢 Hive table')
# 讀 Impala
from impala.dbapi import connect
conn = connect(host='ip', port=port, user='user_name', password='password')
cur = conn.cursor(user='user_name')
cur.execute('SQL query 查詢')
rdd = spark.sparkContext.parallelize(cur.fetchall())


# 寫 CSV
df.write.csv(path=要寫入的 CSV 檔案的路徑, header=True, sep=',', mode='overwrite')
# 寫 parquet
df.write.parquet(path=要寫入的 parquet 檔案的路徑, mode='overwrite')
# 寫 Hive
# 打開動態 partition
spark.sql('set hive.exec.dynamic.partition.mod = nonstrict')
spark.sql('set hive.exec.dynamic.partition=true')
spark.sql("""
    insert overwrite table hive_table_name
    partition(name1)
    SQL query 查詢，最後要加上 distribute by name1
""")
# 每次都重建一個 partition
df.write.mode('overwrite').partitionBy('name1').insertInto('hive_table_name')
df.write.saveAsTable('hive_table_name', None, 'append', partitionBy='name1')
# 寫 HDFS
df.write.mode('overwrite').options(header='true').csv(檔案路徑)
# 寫 MySQL
# 寫入方式可以是 overwrite 或是 append
df.write.mode('寫入方式').format('jdbc')\
  .options(url='jdbc:mysql://ip',
           user='user_name',
           password='password',
           dbtable='table_name',
           batchsize='batch 的大小')\
  .save()
```

# Spark submit
* [官方網頁說明](http://spark.apache.org/docs/latest/submitting-applications.html)
```bash
spark2-submit pyspark_file.py
spark2-submit --master local[2] pyspark_file.py # 在 local 上用兩個 nodes 去跑
spark2-submit --master yarn pyspark_file.py # 丟到 yarn 上去跑
# 做詳細設定
spark2-submit --master yarn \
              --deploy-mode cluster \
              --jars jar_file1.jar, jar_file2.jar, ...\
              --driver-class-path driver1.jar : driver2.jar : ... \
              --executor-memory 多少G \ # worker 上記憶體要用多大
              --driver-memory 多少G \ # driver 上記憶體要用多大
              --conf spark.app.name='輸入些什麼' \
              --conf spark.default.parallelism=某個數 \
              --conf spark.memory.fraction=0 ~ 1 之間的數 \
              --conf spark.memory.storageFraction=0 ~ 1 之間的數 \
              --conf spark.yarn.executor.memoryOverhead=2048 \
              --conf spark.yarn.driver.memoryOverhead=1024 \
              --conf spark.yarn.maxAppAttempts=1 \
              --conf spark.serializer=org.apache.spark.serializer.KryoSerializer \
              pyspark_file.py
```

df.foreach(func_name) df.rdd.foreach(func_name)
df.foreachPartition(func_name), df.rdd.foreachPartition(func_name)

* map 和 reduce
  * map 把 RDD 中的每個元素都經過 map 裡面的函數處理後，再傳回給原來的 RDD，每個 RDD 都是單獨處理的，不會影響到其他的。
    * 最後的結果 RDD 的總數目是不變的
    * map 是 1 對 1 關係
  * flatMap 把 RDD 中的每個元素都經過處理後，傳回一個 list。
    * 這個 list 裡面可以是 1 個或很多個 RDD，最後的結果 RDD 的總數目是不變或是變多
    * flatMap 是 1 變多關係
  * reduce 首先把 RDD 的前兩個元素拿來處理，產生的結果與 RDD 的第三格元素再拿來處理，一直這樣下去直到最後只剩下一個元素
    * reduce 是多變 1
  * reduceByKey(binary_function) 是作用在 RDD 的元素是 key-value 時，把相同 key 的元素拿來做 reduce 的處理，所以最終的結果是每個 key 只會有一個 value
df.map(func_name)
df.reduct(func_name)

* 排序
df.sort('col', ascending=False)

* 把 Spark dataframe 注冊成 SQL 的表格
  * 這樣就能對 dataframe 直接使用 SQL query 來查詢
```python
df.createOrReplaceTempView('table_name')
```

df.fillna() 和 df.na.fill() 是一樣的

df.sort("age",ascending=False).show()
df.sort(df.age.desc()).show()
df.orderBy(df.age.desc()).show()

DataFrame().registerTempTable() createOrReplaceTempView 互为同名函数

data=df.repartition(7,'age').show()
data.rdd.getNumPartitions()

df.toJSON()

* 顯示每個欄位的名字和型態，傳回的是 [('col1', 'type1'), ('col2', 'type2'), ...]
df.dtypes

F.countDistinct()
df.selectExpr('A as B')
