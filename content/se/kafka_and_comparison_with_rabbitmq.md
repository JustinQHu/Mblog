---
title: "Apache Kafka and A Comparison with RabbitMQ"  
date: 2022-12-23T11:01:01-05:00  
categories: ['engineering', ]  
tags: ['engineering', 'kafka', 'Apache Kafka','RabbitMQ', 'mq', 'event streaming']  
draft: false
---

## Kafka

### Introduction

Apache Kafka is heavily used in my work now. Therefore, I thought it would be useful and helpful to
write a post about it.

[Kafka](https://kafka.apache.org/documentation/) is an ***event streaming*** platform. Apache Kafka
defines event streaming as the following:
>Technically speaking, event streaming is the practice of capturing data in real-time from
> event sources like databases, sensors, mobile devices, cloud services, and software 
> applications in the form of streams of events; storing these event streams durably for 
> later retrieval; manipulating, processing, and reacting to the event streams in 
> real-time as well as retrospectively; and routing the event streams to different 
> destination technologies as needed. Event streaming thus ensures a continuous 
> flow and interpretation of data so that the right information is at the right place, 
> at the right time.


### Installation and Hello World with Kafka
> For how to install and run Kafka on other systems, please refer to the official
> documentation: https://kafka.apache.org/quickstart

I am using Mac. [Homebrew](https://brew.sh/) is a very nice tool to install and manage packages on Mac, quite similar
to [apt-get](https://help.ubuntu.com/community/AptGet/Howto) on Ubuntu. The Appstore on MacOS is enough to install, upgrade, and update
applications for normal users, but definitely not sufficient for developers.  Honestly, I think Apple should acquire Homebrew and integrate
it into MacOS.

#### Install Java
```shell
brew install java
java --version
```

#### Install Kafka 
```shell
brew install kafka
```

#### Start Zookeeper
Open a new terminal, run the command to start [ZooKeeper](https://zookeeper.apache.org/), because Kafka leverages ZooKeeper to manage clusters.
```shell
zookeeper-server-start /usr/local/etc/kafka/zookeeper.properties
```

#### Start Kafka
>Note: 
>Edit /usr/local/etc/kafka/server.properties to change listeners to listeners=PLAINTEXT://localhost:9092  

Open a new terminal, run the command to start Kafka:
```shell
kafka-server-start /usr/local/etc/kafka/server.properties
```

#### Create Kafka Topic
Create a **test** topic with replication-factor=1(no extra replicas) and partition=1(only 1 partition)
```shell
kafka-topics --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic test
```


#### Initialize 2 Kafka Producers
Open 2 other terminals and run the command to start Kafka producers:
```shell
kafka-console-producer --broker-list localhost:9092 --topic test
```
Write 4 messages in the following sequence:
1. producer1 --> message 1
2. producer2 --> message 1
3. producer1 --> message 2
4. producer2 --> message 2

>(base) xxxx@MacBook-Air-2 ~ % kafka-console-producer --broker-list localhost:9092 --topic test
>producer1 message 1  
>producer1 message 2

>(base) xxxx@MacBook-Air-2 ~ % kafka-console-producer --broker-list localhost:9092 --topic test
>producer2 message1   
>producer2 message2


#### Start a Kafka Consumer
Open a terminal and run the command to initialize a kafka consumer:
```shell
kafka-console-consumer --bootstrap-server localhost:9092 --topic test --from-beginning
```

And we will the 4 messages written before:

>(base) huqijun@MacBook-Air-2 ~ % kafka-console-consumer --bootstrap-server localhost:9092 --topic test --from-beginning  
>producer1 message 1  
>producer2 message1  
>producer1 message 2  
>producer2 message2  


Kafka is using [Pub-Sub](https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern) pattern. We can create multiple producers and consumers for a certain topic to play with it.


## A Comparison between Kafka and RabbitMQ

By design Kafka is an **event streaming** platform while RabbitMQ is a **message queue**.  

Here are some of their major differences between **Apache Kafka** and [RabbitMQ](https://www.rabbitmq.com/):

|                    |                                                                           Kafka                                                                           |                                                                                                                    RabbitMQ |
|:-------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------:|----------------------------------------------------------------------------------------------------------------------------:|
| Message Ordering   |                             provides message ordering thanks to its partitioning. Messages are sent to topics by message key.                             |                                                                                                                         N/A |
| Message Retention  | Policy-based (e.g., 30 days) <br/> Kafka is a log, which means that it retains messages by default. You can manage this by specifying a retention policy. | Acknowledgment based<br/>RabbitMQ is a queue, so messages are done away with once consumed, and acknowledgment is provided. |
| Message Priorities |                                                                            N/A                                                                            |                               In RabbitMQ, you can specify message priorities and consume message with high priority first. |
| Routing            |                                                                  Publish/Subcribe bassed                                                                  |                                                               Multiple exchange types: Direct, Fan out, Topic, Header-based |


Generally, Kafka retains messages while RabbitMQ doesn't; RabbitMQ offers more complex and flexible
ways of routing messages and prioritizing messages, while Kafka supports a simple Pub-Sub pattern. 

Regarding **performance**, both systems can be scaled to process millions of messages per second. Some 
[benchmarking results](https://www.confluent.io/blog/kafka-fastest-messaging-system/) suggest that:
1. Given same resource configuration, Kafka has higher throughput than RabbitMQ;
2. RabbitMQ delivers lower message latency than Kafka


## Conclusion
Both Apache Kafka and RabbitMQ are great systems. When complex routing and prioritizing of messages are
required, RabbitMQ is the preferred option; when message ordering and retention are needed, Kafka is the choice.   
In terms business cases,  RabbitMQ is more used for **transactional** systems and is perfect for routing
message between clusters of microservices whereas Kafka is more used in **analytical** systems such as data 
streaming and pipelines.


