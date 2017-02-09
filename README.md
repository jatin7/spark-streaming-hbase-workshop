## Stream Time Series Data into Hbase with Apache Spark


### Workshop Details

Open the ./doc/index.html page in your browser


### Run the Application

Create an hbase table to write to:
launch the hbase shell
$hbase shell

create '/user/user01/sensor', {NAME=>'data'}, {NAME=>'alert'}, {NAME=>'stats'}

Commands to run labs:

Step 1: Checkout code on the server where MapR Client is installed & configured or on hadoop server. 
git clone https://github.com/jatin7/spark-streaming-hbase-workshop.git

Also, create a directory 
sudo mkdir -p /user/user01/sparkstreaminglab/data

Step 2: start the streaming app

spark-submit --class "solutions.HBaseSensorStream" target/sparkstreaminglab-1.0.jar

Step 3: copy the streaming data file to the stream directory
cp sensordata.csv  /user/user01/stream/.

Step 4: you can scan the data written to the table, however the values in binary double are not readable from the shell
launch the hbase shell,  scan the data column family and the alert column family 
$hbase shell
scan '/user/user01/sensor',  {COLUMNS=>['data'],  LIMIT => 10}
scan '/user/user01/sensor',  {COLUMNS=>['alert'],  LIMIT => 10 }

Step 5: launch one of the programs below to read data and calculatecalculate stats for one column
/opt/mapr/spark/spark-<version>/bin/spark-submit --driver-class-path `hbase classpath` --class solutions.HBaseReadWrite sparkstreaminglab-1.0.jar
calculate stats for whole row
spark-submit --driver-class-path `hbase classpath` --class solutions.HBaseReadRowWriteStats sparkstreaminglab-1.0.jar

launch the shell and scan for statistics
scan '/user/user01/sensor',  {COLUMNS=>['stats']}


