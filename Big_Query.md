bq show project_ID:dataset_ID:table_ID

bq help 或是 bq help 命令

bq query 'SQL 命令' 大小寫有差，可以用 IGNORE CASE 來不分大小寫

bq ls 在 project 內的 dataset

bq ls project_ID

bq mk project_ID:dataset 建立空的 dataset

bq load project_ID:dataset.table_name input_file schema 其中 schema 是 column_name:type, column_name:type 的形式

bq rm -r dataset

在 jupyter notebook 中使用 BQ 要先安裝 pip install --upgrade google-cloud-bigquery[pandas]

然後要 %load_ext google.cloud.bigquery

接下來就可以用 %%bigquery variable的名字 SQL expression 來執行，結果會存在 variable的名字 內
