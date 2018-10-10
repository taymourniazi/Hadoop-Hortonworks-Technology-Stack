# Hadoop-Hortonworks-Technology-Stack
## Models and administrative guide for hadoop in Hortonworks and all technology stacks
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
