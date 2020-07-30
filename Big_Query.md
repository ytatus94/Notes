# Big Query Usage
```bash
bq.py [--global_flags] <command> [--command_flags] [args]
```
* command 可以是 cancel, cp, extract, get-iam-policy, head, help, init, insert, load, ls, mk, mkdef, partition, query, rm, set-iam-policy, shell, show, update, version, wait

```bash
# 當 job_id 的 job 結束後 cancel
bq cancel job_id

# 不等 job 結束，馬上 cancel
bq --nosync cancel job_id

# 拷貝表格
bq cp dataset.old_table dataset2.new_table

# 把表格存到 GCS，可以同時指定很多個 GCS URI，用逗號分隔開就好
bq extract ds.table gs://mybucket/table.csv

# 把 model 存到 GCS，一樣可以同時指定很多個 GCS URI
bq extract -m ds.model gs://mybucket/model

# 顯示表格前幾列，-s 指定 skip 前幾列，-n 指定顯示幾列
bq head -s 5 -n 10 dataset.table

# help
bq help 
bq help 命令
bq 命令 --help

# 把檔案讀到 BQ 內變表格，schema 是一個 json 檔案
bq load ds.new_tbl input1,input2,... schema.json

# 把檔案讀到 BQ 內變表格，schema 是 column_name:type, column_name:type 的形式
bq load ds.new_tbl input1,input2,... col1:type1,col2:type2,...
bq load project_ID:dataset.table_name input_file schema

# ls，可以加上 -a
bq ls

# ls 出 project 內的 dataset
bq ls project_ID

# ls 出 dataset
bq ls -m mydataset

# 建立新的 dataset, table, view
bq mk new_dataset
bq mk new_dataset.new_table
bq mk -t new_dataset.newtable name:integer,value:string

# 建立空的 dataset
bq mk project_ID:dataset

# 把 <source_table_prefix><YYYYmmdd> 拷貝到 partitioned table
bq partition <source_table_prefix> <destination_partitioned_table>
bq partition dataset1.sharded_ dataset2.partitioned_table

# query，大小寫有差，可以用 IGNORE CASE 來不分大小寫
bq query 'SQL 命令'

# 刪除 dataset, table 等等的
bq rm -r dataset
bq rm ds.table
bq rm -m ds.model
bq rm -r -f old_dataset

# 用 interactive session
bq shell

# 顯示物件資訊
bq show 
bq show project_ID:dataset:table
bq show [--schema] dataset.tabl
bq show -m ds.model
```
在 jupyter notebook 中使用 BQ 要先安裝 pip install --upgrade google-cloud-bigquery[pandas]

然後要 %load_ext google.cloud.bigquery

接下來就可以用 %%bigquery variable的名字 SQL expression 來執行，結果會存在 variable的名字 內
