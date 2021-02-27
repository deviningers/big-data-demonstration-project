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
  For my demonstation, we will be going through part of the fraud detection program, and learn about what goes into a fraud detection application.
  
  For our example, we want our fraud detector to send or create an alert every time we have a small transaction(>$1) followed by a large transaction(<$500). For this to happen, we will need to remember previous transactions and whether they were small or not, so we will use a KeyedProcessFunction, which will help us remember the state of our transactions. We will do this using a ValueState, which will keep track of whether or not a specific account, identified by the keyby() function. 
  Creating the ValueState to keep track of our account flags will be written as follows:
  
  ``` private transient ValueState<Boolean> flagState;```
  
  We will create a ValueStateDescriptor, which is used to create the valueState variable:
  
  ```
  public void open(Configuration parameters) {
        ValueStateDescriptor<Boolean> flagDescriptor = new ValueStateDescriptor<>(
                "flag",
                Types.BOOLEAN);
        flagState = getRuntimeContext().getState(flagDescriptor);
    }
   ```
   ValueState has three methods that we will use: update, value, and clear. Update will set the state of the variable, value will return the current value of the variable, and clear will remove the contents of the variable to null. 
   Now, we can go into how to use the ValueState to monitor our transactions. To get the current state of the key, we will create a boolean that keeps track of the flagState:
   `Boolean lastTransactionWasSmall = flagState.value();`
   Then, we will check whether the flag has been set by the last transaction. If it has, we then check the next transaction to see if it was a large transaction. If so, an alert is created for the transaction and account ID and logged.
   
   ```
   if (lastTransactionWasSmall != null) {
        if (transaction.getAmount() > LARGE_AMOUNT) {
            // Output an alert downstream
            Alert alert = new Alert();
            alert.setId(transaction.getAccountId());

            collector.collect(alert);            
        }
   ```
   We also need to clear the flagState, because if the transaction was not large, then the flag will be reset and if it was large, an alert has already been created.
   `flagState.clear();`
   
   Lastly, we check if a transaction is small once again to see if the flag needs to be reset.
   ```
   if (transaction.getAmount() < SMALL_AMOUNT) {
        // Set the flag to true
        flagState.update(true);
    }
   ```
   
  Link to Dylan Opoka's Demonstration:

## v2 State + Time = ❤️

## Pulling in Data from Other Sources
- So now that we have a working program that can detect fraud, lets modify it so that we can pull in actual data sources.
- The first thing we are going to change is the execution environment from the default streaming env
- So now that we have a working program that can detect fraud, lets modify it so that we can pull in actual data sources, in this case a CSV (Comma Separated Value file)
  - The CSV file I am using is the Synthetic Financial Datasets For Fraud Detection from kaggle.com (link below)
- We are going to have to replace the two DataStream: Transactions and alerts

### Transactions
- In order to mimic how Transactions functions we are going to are going to pull only the accountId and amount transferred, so we are first going to change FraudDetectionJob.java
```Java
ExecutionEnvironment env = ExecutionEnvironment.getExecutionEnvironment(); // replaces stream execution environment

...

DataSet<Tuple2<Double, String>> transaction = env.readCsvFile("file:///FraudDataSet")
  .includeFields("00110000000") // this takes in columns 3 (amount) and 4 (nameOrig)
  .types(Double.class, String.class);
```
  - `.includeFields("01")` is used to specify which columns of the CSV to parse, we are parsing columns 3 and 4 out of the 11. These are stored as a Tuple as a Double and a String

- Now we need to change FraudDetector.java as well
  - We need to remove each use of `Transaction` and replace it with reading in our Tuple2
```Java
final ExecutionEnvironment env = ExecutionEnvironment.getExecutionEnvironment();
public void processElement(Tuple2<Double, String> transaction, ... )
```
- We are going to want to change the DataStream<Transaction> lines to be DataSet<String>
  - We also are going to need to change how transaction is called so for each `transaction.getAmount()` we replace with `transaction.f0` (.f0 is our first value in the Tuple2 while .f1 is the second).  Similarly, `transaction.getAccountId()` is replaced with `transaction.f1`

##### [NOTE] you need to include these imports in your two .java files
```Java
import org.apache.flink.api.java.tuple.Tuple2;
import org.apache.flink.api.ExecutionEnvironment;
```

[Devin's video](https://use.vg/BRl4Iv)

## Another Data source

## Sources
[Fraud Detection with the DataStream API](https://ci.apache.org/projects/flink/flink-docs-stable/try-flink/datastream_api.html)

[Batch Processing Flink](https://dev.to/mushketyk/getting-started-with-batch-processing-using-apache-flink-bnh)

[Repo with Flink commands](https://dev.to/mushketyk/getting-started-with-batch-processing-using-apache-flink-bnh)

[Fraud paysim1 Data Set](https://www.kaggle.com/ntnu-testimon/paysim1)
