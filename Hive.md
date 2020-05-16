








Yu-Ting Shen
/
How-to articles






Share


Hive
Created by Yu-Ting Shen
Last updated May 12, 2020
AnalyticsAnalytics

Tutorial
https://www.edureka.co/blog/hive-commands-with-examples


Hive
Developed by FB and became part of Apache Hadoop

Can use SQL-like language called HiveQL (HQL).

Use Hive, we can access files stored in Hadoop Distributed File System (HDFS) or Apache HBase

Sub queries are not supported in Hive

Table information such as schema and partitions are stored in Metastore

In Cloudera, Hive database store in a /user/hive/warehouse.

When we create a table in hive, it creates in the default location of the hive warehouse. – /user/hive/warehouse, after creation of the table we can move the data from HDFS to hive table.


Hive commands
Data Definition Language (DDL) command

CREAT, SHOW, ALTER, DESCRIBE, TRUNCATE, DELETE

Data Manipulation Language (DML)

Must add ; at the end of command

# Login Hive shell
hive

# Exit Hive shell
quit;
exit;

# Create the new database
CREATE database database_name;

# Create table
CREATE TABLE table_name (col1 type1, col2 type2, ...) raw formate delimited fields terminated by ',';

# List out the databases
SHOW databases;

# Use database
USE database_name;

# Show table schema
DESCRIBE table_name;

# Load
LOAD DATA INPATH 'hdfs_file_path' INTO TABLE table_name;
LOAD DATA LOCAL INPATH 'local_file_path' INTO TABLE table_name;
 

LikeBe the first to like this
kb-how-to-article
Write a comment…
Flag notifications
