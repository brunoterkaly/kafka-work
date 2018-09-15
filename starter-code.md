# Getting Started With Kafka (and Zookeeper)

This is a getting started section.

## Add Kafka User

```
adduser --system --no-create-home --disabled-password --disabled-login kafka
```

## Download Kafka and Zookeeper

```
cd /opt
wget http://apache.claz.org/kafka/2.0.0/kafka_2.11-2.0.0.tgz
```

## Deploy Kafka and Zookeeper

```
mkdir /opt/kafka
tar -xvzf kafka_2.11-2.0.0.tgz --directory /opt/kafka --strip-components 1
cd /opt/kafka
ln -s kafka_2.11-2.0.0/ kafka
chown -R kafka:nogroup /opt/kafka
```

## Setup Kafka folders

```
mkdir /var/lib/kafka
mkdir /var/lib/kafka/data
chown –R kafka:nogroup /var/lib/kafka
```

## Fixup Kafka server properties

```
vim /opt/kafka/config/server.properties
```

#### Should look like this

```
log.dirs=/var/lib/kafka/data
log.retention.hours=168  
log.retention.bytes=104857600
```
## Start Zookeeper

```
/opt/kafka/bin/zookeeper-server-start.sh /opt/kafka/config/zookeeper.properties
```

## Test Zookeeper

```
telnet localhost 2181
Ruok
```

## Start Kafka

```
/opt/kafka/bin/kafka-server-start.sh /opt/kafka/config/server.properties
```

## Create a Topic

```
/opt/kafka/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test-kafka
```

## List Topics

```
/opt/kafka/bin/kafka-topics.sh --list --zookeeper localhost:2181
```

## Produce Messages

```
/opt/kafka/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test-kafka

> ENTER IN DATA
```

## Consume Messages

```
/opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test-kafka --from-beginning
```
