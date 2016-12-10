 tar -xvf /home/osboxes/Downloads/kafka_2.10-0.10.1.0.tgz  -C /opt/Kafka/

/home/osboxes/Downloads/kafka_2.10-0.10.1.0.tgz


sudo tar -xvf kafka_2.10-0.10.1.0.tgz -C /opt/Kafka/

        sudo /opt/Kafka/kafka_2.10-0.10.1.0/bin/kafka-server-start.sh /opt/Kafka/kafka_2.10-0.10.1.0/config/server.properties --override property=value




       sudo /opt/Kafka/kafka_2.10-0.10.1.0/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1  --partitions 1 --topic testing


       sudo /opt/Kafka/kafka_2.10-0.10.1.0/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic testing


       sudo /opt/Kafka/kafka_2.10-0.10.1.0/bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic testing --from-beginning

