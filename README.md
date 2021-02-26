# Fraud Detection using Java and Apache Flink
Big Data group project covering Fraud data using Java

### Group
- Devin Ingersoll
- Seth Bennett
- Dylan Opoka
- Enid Maharjan
- Rajeev Chapagain

## Setup / Going through the code




### Software download links
[VMWare](https://my.vmware.com/en/web/vmware/downloads/details?downloadGroup=WKST-1610-WIN&productId=1038&rPId=55777)
 - [Free VMWare Keys from GitHub](https://gist.github.com/gopalindians/ec3f3076f185b98353f514b26ed76507)
 - [Ubuntu](https://ubuntu.com/download/desktop)

[Flink](https://flink.apache.org/downloads.html#apache-flink-1121)
![Script to install maven and java](MavenJavaDownload.sh)
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
