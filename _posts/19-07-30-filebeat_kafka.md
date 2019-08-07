---
title : "[Data Infra] filebeat kafka 연동"
categories:
  - eco system
---

오랜만에 흥미를 끌 만한 것을 해 보았다.

## Step 1: kafka

공식사이트 : <https://kafka.apache.org/quickstart>

~~~
wget http://mirror.navercorp.com/apache/kafka/2.2.0/kafka_2.12-2.2.0.tgz

tar -xzf kafka_2.12-2.2.0.tgz
~~~

- zookeeper
    ~~~
    bin/zookeeper-server-start.sh [-daemon] config/zookeeper.properties

    :2181
    ~~~

- kafka server
    ~~~
    bin/kafka-server-start.sh [-daemon] config/server.properties

    :9092
    ~~~

- create topic
    ~~~
    bin/kafka-topics.sh \
    --create \
    --bootstrap-server localhost:9092 \
    --replication-factor 1 \
    --partitions 1 \
    --topic server-a
    ~~~

- consumer
    ~~~
    bin/kafka-console-consumer.sh \
    --bootstrap-server localhost:9092 \
    --topic server-a \
    --from-beginning
    ~~~

- producer는 filebeat가 맡는다.

## Step 2: filebeat

공식사이트 : <https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-getting-started.html>

~~~
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.2.0-linux-x86_64.tar.gz

tar xzvf filebeat-7.2.0-linux-x86_64.tar.gz
~~~

- vi filebeat.yml

~~~
filebeat.inputs:
- type: log
  enabled: true
  paths:
    - /home/filebeat/Documents/server_a.log


#----------------------------- kafka output --------------------------------
output.kafka:
  hosts: ["localhost:9092"]
  topic: "server-a"
  partition.round_robin:
    reachable_only: false

  required_acks: 1
  compression: gzip
  max_message_bytes: 1000000
~~~

- 실행

~~~
./filebeat -e
~~~

<figure>
  <img src="/assets/images/19-07-30-filebeat_kafka/run.png">
  <figcaption>실행화면</figcaption>
</figure>


