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


* Groupby
```python
# 對全部欄位做一樣的 aggregation function
df.groupBy('欄位名').count().show()

# 對個別欄位做不同的 aggregation functions
import pyspark.sql.functions as F
df.groupBy('欄位名1').agg(F.max('欄位名2'), F.sum('欄位名3')).show()
```
* 建立 Spark dataframe
```python
list_name = [(raw1_val1, raw1_val2, raw1_val3, ...),
             (raw2_val1, raw2_val2, raw2_val3, ...),
             (raw3_val1, raw3_val2, raw3_val3, ...),
             ...]
df = spark.createDataFrame(list_name, schema=['col1', 'col2', 'col3', ...]) # schema 可以用 list 或是用 tuple
df.show()
```
* Join:
  * join() 預設是使用 inner join
  * 若 join 兩個的表格有相同的欄位名字時，join 產生的結果就會有重複的欄位名字
    * pandas 會在重複的欄位名字後面加上 `_x, _y` 等，但是 Spark 不會
```python
df = df1.join(df2, df1.col1==df2.col2, how='left')\ # 注意兩個 dataframe 用來連接的 key 不同名字時，是用 ==
        .join(df3, on=['col3', 'col4'], how='outer') # 如果兩個 dataframe 用來連接的 key 同名字時可以直接用 on=['key1', 'key2', ...]
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



