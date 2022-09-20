# Quick start

This is a simple project to learn more about Apache Kafka

## 1. Set up a Kafka Broker

```
  version: '3'

  services:
    zookeeper:
      image: confluentinc/cp-zookeeper:7.0.1
      container_name: zookeeper
      environment:
        ZOOKEEPER_CLIENT_PORT: 2181
        ZOOKEEPER_TICK_TIME: 2000

    broker:
      image: confluentinc/cp-kafka:7.0.1
      container_name: broker
      ports:
        - "9092:9092"
      depends_on:
        - zookeeper
      environment:
        KAFKA_BROKER_ID: 1
        KAFKA_ZOOKEEPER_CONNECT: 'zookeeper:2181'
        KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_INTERNAL:PLAINTEXT
        KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://localhost:9092,PLAINTEXT_INTERNAL://broker:29092
        KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
        KAFKA_TRANSACTION_STATE_LOG_MIN_ISR: 1
        KAFKA_TRANSACTION_STATE_LOG_REPLICATION_FACTOR: 1
```

## 2. Start the Kafka Broker and Zookeeper

```
  docker-compose up -d
```

## 3. Verify if the containers is running

```
  docker-compose ps
```

## 4. Create a new topic

```
  docker exec -it broker bash
```

```
  kafka-topics --bootstrap-server broker:9092 --create --topic firstTopic
```

```
  // Set the Replication Factor
  kafka-topics --bootstrap-server broker:9092 --create --topic firstTopic --replication-factor 1
```

```
  // Set the Partitions number
  kafka-topics --bootstrap-server broker:9092 --create --topic firstTopic --partitions 3
```

## 5. Write messages to the topic

```
  kafka-console-producer --bootstrap-server broker:9092 --topic firstTopic
```

```
  this is my first kafka message
  hello world!
  this is my third kafka message. I’m on a roll :-D
```

When you’ve finished, press Ctrl-D to return to your command prompt.

## 6. Read messages from topic

```
  kafka-console-consumer --bootstrap-server broker:9092 --topic firstTopic --from-beginning
```

When you’ve finished, press Ctrl-C to return to your command prompt.

## 7. Show information about the Topic

```
  kafka-topics --describe --bootstrap-server broker:9092 --topic firstTopic
```

## 8. Create a new Consumer Groups

```
  kafka-console-consumer --bootstrap-server broker:9092 --topic firstTopic --from-beginning --group firstGroup
```

## 9. Show information about the Consumer Groups

```
  kafka-consumer-groups --group firstGroup --bootstrap-server broker:9092 --describe
```

## 10. Stop the Kafka Broker

```
  docker-compose down
```
