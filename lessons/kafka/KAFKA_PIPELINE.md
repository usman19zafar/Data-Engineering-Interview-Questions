Kafka End‑to‑End Pipeline (Producer → Broker → Consumer)
Author: Usman Zafar
Purpose: Mechanical validation of Kafka message flow.

1. Pipeline Components
Code
Producer → Broker → Log → Consumer
2. Start Zookeeper
Code
zookeeper-server-start.bat ..\..\config\zookeeper.properties
3. Start Kafka Broker
Code
kafka-server-start.bat ..\..\config\server.properties
4. Create Topic
Code
kafka-topics.bat --create --topic test --bootstrap-server localhost:9092
5. Producer
Code
kafka-console-producer.bat --broker-list localhost:9092 --topic test
Send:

Code
uzi
qzi
6. Consumer
Code
kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic test --from-beginning
Expected:

Code
uzi
qzi
7. What the Test Proves
Producer connectivity

Broker routing

Log append

Zookeeper metadata

Consumer group coordination

End‑to‑end pipeline health

End of KAFKA_PIPELINE.md
