log.retention.hours=168  #other accepted keys are(log.retention.ms, log.retention.minutes) 
log.retention.bytes=104857600



telnet localhost 2181
Ruok

# Add Kafka User
adduser --system --no-create-home --disabled-password --disabled-login kafka

# Download Kafka and Zookeeper
cd /opt
wget http://apache.claz.org/kafka/2.0.0/kafka_2.11-2.0.0.tgz

# Deploy Kafka and Zookeeper
mkdir /opt/kafka
tar -xvzf kafka_2.11-2.0.0.tgz --directory /opt/kafka --strip-components 1
ln -s kafka_2.11-2.0.0/ kafka
chown -R kafka:nogroup /opt/kafka

# Setup Kafka folders
mkdir /var/lib/kafka
mkdir /var/lib/kafka/data
chown –R kafka:nogroup /var/lib/kafka

# Fixup Kafka server properties
vim /opt/kafka/config/server.properties
# Should look like this (without #). Validate values:
#       log.dirs=/var/lib/kafka/data
#       log.retention.hours=168  
#       log.retention.bytes=104857600


/opt/kafka/bin/kafka-server-start.sh /opt/kafka/config/server.properties

/opt/kafka/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test

/opt/kafka/bin/kafka-topics.sh --list --zookeeper localhost:2181
/opt/kafka/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test

/opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning

