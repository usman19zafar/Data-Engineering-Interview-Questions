VALIDATION_LOGS.md
Kafka Commissioning Validation Logs (Windows)
Author: Usman Zafar
Purpose: Store clean, minimal, deterministic logs proving Kafka + Zookeeper are fully operational.

1. Zookeeper Startup Log (Healthy)
Code
INFO binding to port 0.0.0.0/0.0.0.0:2181
INFO Started AdminServer on address 0.0.0.0:8080
INFO ZooKeeper audit is disabled.
INFO Creating new log file: log.1
Meaning:  
Zookeeper is running, listening on port 2181, and ready for Kafka.

2. Kafka Broker Startup Log (Healthy)
Code
INFO Connected to Zookeeper
INFO Registered broker 0 at path /brokers/ids/0
INFO Awaiting socket connections on 0.0.0.0:9092
INFO Kafka version: 3.7.0
Meaning:  
Broker successfully connected to Zookeeper and is ready to accept producer/consumer traffic.

3. Producer Execution Log
Command:

Code
kafka-console-producer.bat --broker-list localhost:9092 --topic test
Input:

Code
uzi
qzi
Meaning:  
Producer successfully sent messages to the broker.

4. Consumer Execution Log
Command:

Code
kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic test --from-beginning
Output:

Code
uzi
qzi
Meaning:  
Consumer successfully read messages from the broker.
End‑to‑end pipeline is validated.

5. Four‑Window Operational Snapshot
Code
Window 1 → Zookeeper (2181 logs)
Window 2 → Kafka Broker (9092 logs)
Window 3 → Producer (you type messages)
Window 4 → Consumer (messages appear automatically)
6. Validation Summary
✔ Zookeeper running
✔ Kafka broker running
✔ Topic created
✔ Producer sending messages
✔ Consumer receiving messages
✔ End‑to‑end pipeline validated
✔ Environment fully commissioned

End of VALIDATION_LOGS.md
