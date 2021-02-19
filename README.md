# Fraud Detection using Java and Apache Flink
Big Data group project covering Fraud data using Java

### Group
- Devin Ingersoll
- Seth Bennett
- Dylan Opoka
- Enid Maharjan
- Rajeev Chapagain

## Setup / Going through the code

## Writing a Real Application v1

## v2 State + Time = ❤️

## Pulling in Data from other sources
- You can add this java code to pull in data from Kafka
~~~java
public static FlinkKafkaConsumer011<String> createStringConsumerForTopic(
  String topic, String kafkaAddress, String kafkaGroup ) {

    Properties props = new Properties();
    props.setProperty("bootstrap.servers", kafkaAddress);
    props.setProperty("group.id",kafkaGroup);
    FlinkKafkaConsumer011<String> consumer = new FlinkKafkaConsumer011<>(
      topic, new SimpleStringSchema(), props);

    return consumer;
}
~~~
- We also need to pull in the following dependencies:
~~~xml
<dependency>
    <groupId>org.apache.flink</groupId>
    <artifactId>flink-core</artifactId>
    <version>1.5.0</version>
</dependency>
<dependency>
    <groupId>org.apache.flink</groupId>
    <artifactId>flink-connector-kafka-0.11_2.11</artifactId>
    <version>1.5.0</version>
</dependency>
~~~
-


## Another Data source

## Sources
[Fraud Detection with the DataStream API](https://ci.apache.org/projects/flink/flink-docs-stable/try-flink/datastream_api.html)
[Building Data Pipeline](https://www.baeldung.com/kafka-flink-data-pipeline)
