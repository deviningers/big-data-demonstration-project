# Fraud Detection using Java and Apache Flink
Big Data group project covering Fraud data using Java

### Group
- Devin Ingersoll
- Seth Bennett ->Setup / Going through the code
- Dylan Opoka
- Enid Maharjan
- Rajeev Chapagain

## Setup / Going through the code


[Link to My Video](https://youtu.be/Ar6iEwQcnvY)

### Software download links
[VMWare](https://my.vmware.com/en/web/vmware/downloads/details?downloadGroup=WKST-1610-WIN&productId=1038&rPId=55777)
 - [Free VMWare Keys from GitHub](https://gist.github.com/gopalindians/ec3f3076f185b98353f514b26ed76507)
 - [Ubuntu](https://ubuntu.com/download/desktop)

[Flink](https://flink.apache.org/downloads.html#apache-flink-1121)
### Useful Commands
Maven and Java Download commands
```Bash
sudo apt-get install maven -y
sudo apt-get install openjdk-11-jdk -y
```
Base Fraud Detection program
```Bash
mvn archetype:generate \
    -DarchetypeGroupId=org.apache.flink \
    -DarchetypeArtifactId=flink-walkthrough-datastream-java \
    -DarchetypeVersion=1.12.1 \
    -DgroupId=frauddetection \
    -DartifactId=frauddetection \
    -Dversion=0.1 \
    -Dpackage=spendreport \
    -DinteractiveMode=false
```
Package maven file
```Bash
mvn package
```
Start/stop flink clusters
```Bash
./bin/start-cluster.sh
./bin/stop-cluster.sh
```
Run Program
```Bash
./bin/flink run ../temp/frauddetection/target/frauddetection-0.1.jar
```
## Writing a Real Application v1
  Link to Dylan Opoka's Demonstration: 

## v2 State + Time = ❤️

## Pulling in Data from other sources
- So now that we have a working program that can detect fraud, lets modify it so that we can pull in actual data sources.
- The first thing we are going to change is the execution environment from the default streaming env
```Java
final ExecutionEnvironment env = ExecutionEnvironment.getExecutionEnvironment();
```
-
- We are going to want to change the DataStream<Transaction> lines to be DataSet<String>

link to Devin's video:
## Another Data source

## Sources
[Fraud Detection with the DataStream API](https://ci.apache.org/projects/flink/flink-docs-stable/try-flink/datastream_api.html)

[Batch Processing Flink](https://dev.to/mushketyk/getting-started-with-batch-processing-using-apache-flink-bnh)

[Repo with Flink commands](https://dev.to/mushketyk/getting-started-with-batch-processing-using-apache-flink-bnh)
