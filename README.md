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
  
### LOADING TWEETS INTO HADOOP  
  
hadoop fs -copyFromLocal tweets01.json /user/tweets/  
  
##### In Hive  
CREATE EXTERNAL TABLE t_tweets (   
  id BIGINT,   
  created_at STRING,    
  source STRING,   
  favorited BOOLEAN,    
  retweet_count INT,    
  retweeted_status STRUCT<   
     text:STRING,     
     user:STRUCT<screen_name:STRING,name:STRING>>,   
  entities STRUCT<   
     urls:ARRAY<STRUCT<expanded_url:STRING>>,   
     user_mentions:ARRAY<STRUCT<screen_name:STRING,name:STRING>>,   
     hashtags:ARRAY<STRUCT<text:STRING>>>,   
  text STRING,   
  user STRUCT<    
     screen_name:STRING,   
     name:STRING,   
     friends_count:INT,   
     followers_count:INT,   
     statuses_count:INT,   
     verified:BOOLEAN,   
     utc_offset:STRING,    
     time_zone:STRING>,   
  in_reply_to_screen_name STRING,   
  year int,   
  month int,     
  day int,   
  hour int   
)   
ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'   
STORED AS TEXTFILE   
LOCATION '/user/tweets';   
  
hive> ADD JAR json-serde-1.3.6-jar-with-dependencies.jar;   
hive> set hive.support.sql11.reserved.keywords=false;  
  
select count(*) b, ip, regexp_extract(uri, '[a-zA-Z]+',0)   
from t_accesslog a   
where uri not like '%prod%' group by ip, uri order by ip, b desc limit 5;  
  
