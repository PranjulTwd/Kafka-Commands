# Setting up the Kafka

_Make sure you are navigated inside the bin directory._

## Start Zookeeper and Kafka Broker

### Start up the Zookeeper.

`./zookeeper-server-start.sh ../config/zookeeper.properties`

### Add the below properties in the server.properties

```
listeners=PLAINTEXT://localhost:9092
auto.create.topics.enable=false
```

### Start up the Kafka Broker

`./kafka-server-start.sh ../config/server.properties`

## How to create a topic ?

`./kafka-topics.sh --create --topic test-topic --replication-factor 1 --partitions 4 --bootstrap-server localhost:9092`

## How to instantiate a Console Producer?

### Without Key

`./kafka-console-producer.sh --broker-list localhost:9092 --topic test-topic`

### With Key

`./kafka-console-producer.sh --broker-list localhost:9092 --topic test-topic --property "key.separator=-" --property "parse.key=true"`

## How to instantiate a Console Consumer?

### Without Key

`./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test-topic --from-beginning`

### With Key

`./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test-topic --from-beginning -property "key.separator= - " --property "print.key=true"`

### With Consumer Group

`./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test-topic --group <group-name>`

## Setting Up Multiple Kafka Brokers

* The first step is to add a new server.properties.

* We need to modify three properties to start up a multi broker set up.

```
broker.id=<unique-broker-d>
listeners=PLAINTEXT://localhost:<unique-port>
log.dirs=/tmp/<unique-kafka-folder>
auto.create.topics.enable=false
```

Example config will be like below.

```
broker.id=1
listeners=PLAINTEXT://localhost:9093
log.dirs=/tmp/kafka-logs-1
auto.create.topics.enable=false
```

### Starting up the new Broker

Provide the new server.properties thats added.

`./kafka-server-start.sh ../config/server-1.properties`

`./kafka-server-start.sh ../config/server-2.properties`

## Advanced Kafka CLI operations:


### List the topics in a cluster

`./kafka-topics.sh --bootstrap-server localhost:9092 --list`

### Describe topic

#### The below command can be used to describe all the topics.

`./kafka-topics.sh --bootstrap-server localhost:9092 --describe`

#### The below command can be used to describe a specific topic.

`./kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic <topic-name>`

### Alter the partitions of a topic

`./kafka-topics.sh --bootstrap-server localhost:9092 --alter --topic test-topic --partitions 40`

### Delete a topic

`./kafka-topics.sh --bootstrap-server localhost:9092 --delete --topic test-topic`

### How to view consumer groups

`./kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list`

### Consumer Groups and their Offset

`./kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group console-consumer-27773`

### Viewing the Commit Log

`./kafka-run-class.sh kafka.tools.DumpLogSegments --deep-iteration --files /tmp/kafka-logs/test-topic-0/00000000000000000000.log`
