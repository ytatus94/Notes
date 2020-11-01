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
* 常用的 pySpark import
```python
from pyspark.sql import SparkSession # 這個是進入點
from pyspark.sql import Row
from pyspark.sql.types import *
from pyspark.sql.functions import *
from pyspark.sql import functions as F
from pyspark.sql.functions import isnull, 
from pyspark.sql.functions import concat, concat_ws, format_string # 字串處理時用的
```
* Spark 是 lazy 的，分成了 transformation 和 action
  * transformation 是指明要做什麼動作，但還不會去執行動作
  * action 才是真正會去執行動作
    * `show()`, `collect()` 是 action
* 建立 Spark dataframe
```python
# 把 SQL query 的結果直接轉成 Spark dataframe
df = spark.sql("SQL query 敘述")
# 由 Row 物件來建立，並配合 toDF()
row = Row('欄位名 1', '欄位名 2', ...)
df = sc.parallelize([row(第一列欄位 1 的值, 第一列欄位 2 的值, ...), 
                     ..., 
                     row(第 n 列欄位 1 的值, 第 n 列欄位 2 的值, ...)]).toDF()
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
df.describe().show() # 記得要加上 show()
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
```python
# 把兩個字串欄位接在一起，變成新的欄位，結果是 str_col1str_col2
df.select(F.concat(df.str_col1, df.str_col2).alias('str_col3'))
# 把兩個字串欄位用連接符號接在一起，變成新的欄位，結果是 str_col1連接符號str_col2
df.select(concat_ws('連接符號', df.str_col1, df.str_col2).alias('str_col3')).show()
```


* `groupBy()`
```python
# 對全部欄位做一樣的 aggregation function
df.groupBy('欄位名').count().show()

# 對個別欄位做不同的 aggregation functions
df.groupBy('欄位名1').agg({'欄位名2': '函數名'})
import pyspark.sql.functions as F
df.groupBy('欄位名1').agg(F.max('欄位名2'), F.sum('欄位名3')).show()

```


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




