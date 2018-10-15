# Hadoop-Hortonworks-Technology-Stack
## Models and administrative guide for hadoop in Hortonworks and all technology stacks
##                            HDFS
### Create a directory on HDFS:
hadoop fs -mkdir /user/hadoop/dir1
### Copy a local file weblog.txt to HDFS:
hadoop fs -put /home/anurag/weblog.txt /user/hadoop/dir1/
### List an HDFS directory contents:
hadoop fs -ls /user/hadoop/dir1
### Show the space utilization on HDFS:
hadoop fs -du /user/hadoop/dir1
### Copy an HDFS file to a file weblog.txt.1 in the local directory:
hadoop fs -get /user/hadoop/dir1/weblog.txt /home/anurag/weblog.txt.1
### Get help on HDFS commands:
hadoop fs -help
  
##                            HIVE  
[hduser@sandbox ~]$ hadoop fs -mkdir /hbp
[hduser@sandbox ~]$ hadoop fs -mkdir /hbp/chapt2
[hduser@sandbox ~]$ hadoop fs -put ibmstockquotes.txt /hbp


create external table if not exists t_ibm_stocks (  
Value_Date DATE,  
Open FLOAT,  
High FLOAT,  
Low FLOAT,  
Close FLOAT,  
Volume INT, 
Adj_Close FLOAT)  
row format delimited fields terminated by ','  
location '/hbp/chapt2; 
### finds the maximum opening stock price for each year in the dataset
select MAX(Open), YEAR(Value_Date) a  
  from t_ibm_stocks     
  group by YEAR(Value_Date);    
    
  
###   Finds the average stock prices for each year in the dataset  
select YEAR(Value_Date) as yr, AVG(Open) as av   
  from t_ibm_stocks   
  group by YEAR(Value_Date);  

  
##                            SQOOP
  
sqoop import --connect jdbc:mysql://localhost:3306/test --username root  --table Persons  --hive-table Persons --hive-import --split-by id --direct



### Creating Web Access log Database in Hive and log files are stored in HDFS

CREATE EXTERNAL TABLE t_accesslog (   
        `ip`                STRING,   
        `time_local`        STRING,   
        `method`            STRING,   
        `uri`               STRING,   
        `protocol`          STRING,   
        `status`            STRING,   
        `bytes_sent`        STRING,   
        `referer`           STRING,   
        `useragent`         STRING   
        )   
    ROW FORMAT SERDE 'org.apache.hadoop.hive.contrib.serde2.RegexSerDe'   
    WITH SERDEPROPERTIES (   
    'input.regex'='^(\\S+) \\S+ \\S+ \\[([^\\[]+)\\] "(\\w+) (\\S+) (\\S+)" (\\d+) (\\d+) "([^"]+)" "([^"]+)".*'   
)   
STORED AS TEXTFILE  
LOCATION '/user/access_log_20151010-081346.log';     



#### The Regular Expression Create in preceding hive query was by http://rubular.com/  


yayuver@polyswarms.com  
