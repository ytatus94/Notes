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
