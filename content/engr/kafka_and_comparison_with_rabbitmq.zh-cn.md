---
title: "Apache Kafka以及与RabbitMQ的比较"  
date: 2022-12-23T14:01:10-05:00
categories: ['工程','技术', '2022']   
tags: ['工程','技术', 'kafka', 'Apache Kafka', 'RabbitMQ', '消息队列', '事件流式处理', '事件流']   
draft: false
---

> 本站的所有文章都默认为英文，中文版本由Google Translate 翻译。  
> 由于时间限制，并非所有文章都有中文版本。


## Kafka介绍
Apache Kafka 现在在我的工作中大量使用。 因此，我认为写一篇关于它的帖子会很有用和有帮助。

[Kafka](https://kafka.apache.org/documentation/) 是一个事件流平台。 Apache Kafka 定义事件流如下：

>从技术上讲，事件流是以事件流的形式从数据库、传感器、移动设备、云服务和软件应用程序等事件源实时捕获数据的实践；
> 持久存储这些事件流以供以后检索； 实时和回顾性地操纵、处理和响应事件流； 并根据需要将事件流路由到不同的目标
> 技术。 因此，事件流可确保数据的连续流动和解释，从而使正确的信息在正确的时间出现在正确的位置。

## Kafka安装和Hello World

>关于如何在其他系统上安装和运行Kafka，请参考官方文档：
>https://kafka.apache.org/quickstart

我正在使用 Mac。 [Homebrew](https://brew.sh/) 是一个非常好的工具，可以在 Mac 上安装和管理包，与 Ubuntu 上的 [apt-get](https://help.ubuntu.com/community/AptGet/Howto)  非常相似。 
MacOS 上的 Appstore 对于普通用户来说安装、升级、更新应用已经足够了，但是对于开发者来说绝对不够用。 老实说，我认为 Apple 应该收购 Homebrew 并将其集成到 MacOS 中。

### 安装Java
```shell
brew install java
java --version
```

### 安装Kafka
```shell
brew install kafka
```

### 启动ZooKeeper
打开一个新的终端，运行命令启动 ZooKeeper，因为 Kafka 利用 [ZooKeeper](https://zookeeper.apache.org/) 来管理集群。

```shell
zookeeper-server-start /usr/local/etc/kafka/zookeeper.properties
```
### 启动Kafka
>注意：
>编辑/usr/local/etc/kafka/server.properties，将listeners改为listeners=PLAINTEXT://localhost:9092

打开一个新终端，运行命令启动Kafka：
```shell
kafka-server-start /usr/local/etc/kafka/server.properties
```

### 创建 Kafka Topic
创建一个 replication-factor=1（没有额外的副本）和 partition=1（只有 1 个分区）的测试主题

```shell
kafka-topics --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic test
```

### 初始化2个Kafka生产者
打开其他 2 个终端并运行命令以启动 Kafka 生产者：

```shell
kafka-console-producer --broker-list localhost:9092 --topic test
```
按以下顺序写 4 条消息：
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

### 启动 Kafka 消费者
打开终端并运行命令来初始化 kafka 消费者：
```shell
kafka-console-consumer --bootstrap-server localhost:9092 --topic test --from-beginning
```
而我们将之前写的4条消息：

>(base) xxxx@MacBook-Air-2 ~ % kafka-console-consumer --bootstrap-server localhost:9092 --topic test --from-beginning  
>producer1 message 1  
>producer2 message1  
>producer1 message 2  
>producer2 message2  

Kafka 使用 [Pub-Sub](https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern) 模式。 我们可以为主题创建多个生产者和消费者来学习它。

## Kafka 和 RabbitMQ 的比较
按照设计，Kafka 是一个**事件流平台**，而 RabbitMQ 是一个**消息队列**。

以下是 **Apache Kafka** 和 [RabbitMQ](https://www.rabbitmq.com/) 之间的一些主要区别：

|       |                                                                           Kafka                                                                           |                                                                                                                    RabbitMQ |
|:------|:---------------------------------------------------------------------------------------------------------------------------------------------------------:|----------------------------------------------------------------------------------------------------------------------------:|
| 消息排序  |                            由于其分区，提供消息排序。 消息通过消息键发送到主题。                             |                                                                                                                         N/A |
| 消息保留  | 基于策略（例如，30 天）<br/> Kafka 是一个日志，这意味着它默认保留消息。 您可以通过指定保留策略来管理它。 | 基于确认<br/>RabbitMQ 是一个队列，因此消息一旦被消费就会被删除，并提供确认。|
| 消息优先级 |                                                                            N/A                                                                            |                               在RabbitMQ中，可以指定消息的优先级，优先消费高优先级的消息。 |
| 消息路由  |                                                                  Publish/Subscribe based                                                                  |                                                               Multiple exchange types: Direct, Fan out, Topic, Header-based |


关于性能，这两个系统都可以扩展到每秒处理数百万条消息。 一些[基准测试](https://www.confluent.io/blog/kafka-fastest-messaging-system/)结果表明：
1. 同样的资源配置，Kafka 比 RabbitMQ 有更高的吞吐量；
2. RabbitMQ 提供比 Kafka 更低的消息延迟

## 结论
Apache Kafka 和 RabbitMQ 都是很棒的系统。 当需要对消息进行复杂的路由和优先级排序时，RabbitMQ 是首选； 当需要消息排序和保留时，Kafka 是不二之选。
在业务案例方面，RabbitMQ 更多用于**事务系统**，非常适合在微服务集群之间路由消息，而 Kafka 更多用于数据流和管道等**分析系统**。
