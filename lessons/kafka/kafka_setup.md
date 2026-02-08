Kafka + Zookeeper on Windows — Full Setup, Recovery, and Validation
Author: Usman Zafar
Purpose: Complete, deterministic, architect‑grade setup and troubleshooting guide for running Kafka 3.x with Zookeeper on Windows.

1. Overview
This repository documents the full installation, repair, and commissioning of Apache Kafka + Zookeeper on Windows.
It includes:

Correct Java + PATH configuration

Recovery from a corrupted Windows PATH

Zookeeper startup and troubleshooting

Kafka broker startup

End‑to‑end producer → broker → consumer validation

ASCII architecture diagrams

Operational notes for identifying each active window

This repo is designed for engineers, architects, and learners who want a clean, mechanical, reproducible setup.

2. Prerequisites
Windows 10 or 11

Apache Kafka 3.x (binary download)

Adoptium JDK 17 (HotSpot)

CMD or PowerShell

3. Correct Java + PATH Configuration
Kafka requires a valid Java runtime.
Ensure:

Code
java -version
returns Adoptium JDK 17.

System PATH must include:
Code
C:\Windows\System32
C:\Windows
C:\Windows\System32\Wbem
C:\Windows\System32\WindowsPowerShell\v1.0\
C:\Program Files\Eclipse Adoptium\jdk-17.0.17.10-hotspot\bin
Remove Oracle symlink paths:
Code
C:\Program Files\Common Files\Oracle\Java\javapath
If PATH is missing or corrupted, recreate it manually.

4. Starting Zookeeper
Open Window 1:

Code
cd C:\Kafka\bin\windows
zookeeper-server-start.bat ..\..\config\zookeeper.properties
Healthy indicators:

Code
binding to port 0.0.0.0:2181
Started AdminServer
5. Starting Kafka Broker
Open Window 2:

Code
cd C:\Kafka\bin\windows
kafka-server-start.bat ..\..\config\server.properties
Healthy indicators:

Code
Connected to Zookeeper
Registered broker 0
Awaiting socket connections on 0.0.0.0:9092
Kafka version: 3.7.0
6. Creating a Topic
Open Window 3 (or any new window):

Code
cd C:\Kafka\bin\windows
kafka-topics.bat --create --topic test --bootstrap-server localhost:9092
7. End‑to‑End Validation (uzi / qzi Test)
This is the commissioning test proving the entire pipeline works.

Window 3 — Producer
Code
cd C:\Kafka\bin\windows
kafka-console-producer.bat --broker-list localhost:9092 --topic test
Type:

Code
uzi
qzi
Window 4 — Consumer
Code
cd C:\Kafka\bin\windows
kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic test --from-beginning
Expected output:

Code
uzi
qzi
This confirms:

Producer → Broker connectivity

Broker log append

Zookeeper metadata

Consumer group coordination

End‑to‑end pipeline health

8. Identifying the Four Windows
Code
Window 1 → Zookeeper (shows 2181 logs)
Window 2 → Kafka Broker (shows 9092 logs)
Window 3 → Producer (you type messages)
Window 4 → Consumer (messages appear automatically)
9. Kafka + Zookeeper Architecture (ASCII)
```Code
                           +-----------------------------+
                           |         ZOOKEEPER           |
                           |-----------------------------|
                           |  - Maintains cluster state  |
                           |  - Stores broker metadata   |
                           |  - Tracks controller        |
                           |  - Manages topic configs    |
                           +--------------+--------------+
                                          |
                                          | 2181
                                          |
                   -------------------------------------------------
                   |                                               |
                   v                                               v
        +----------------------+                        +----------------------+
        |      BROKER 0       |                        |      BROKER 1       |
        |----------------------|                        |----------------------|
        | - ID: 0             |                        | - ID: 1             |
        | - Listens: 9092     |                        | - Listens: 9093     |
        | - Registers w/ ZK   |                        | - Registers w/ ZK   |
        +----------+-----------+                        +----------+-----------+
                   |                                               |
                   v                                               v
        +----------------------+                        +----------------------+
        |   PARTITION LEADER  |                        |  PARTITION FOLLOWER |
        +----------------------+                        +----------------------+

                   ^                                               ^
                   |                                               |
        +----------+-----------+                        +----------+-----------+
        |   PRODUCER CLIENT   |                        |   CONSUMER CLIENT   |
        +----------------------+                        +----------------------+
```
10. Zookeeper Troubleshooting (Summary)
Common issues and fixes:

Symptom	Root Cause	Fix
Connection refused: 2181	Zookeeper not running	Start ZK first
Address already in use	Port conflict	netstat -ano → kill PID
dataDir missing	Folder not created	Create C:\tmp\zookeeper
java not found	PATH issue	Fix PATH + JAVA_HOME
Log rotation errors	Windows permissions	Run CMD as admin
Full troubleshooting guide included in repo.

11. Kafka Troubleshooting (Summary)
Symptom	Root Cause	Fix
Broker cannot connect to ZK	ZK not running	Start ZK first
Producer hangs	Broker not running	Start broker
Consumer silent	No messages	Send messages from producer
java.lang errors	Wrong Java	Use Adoptium JDK 17
12. Status
✔ Java validated
✔ PATH repaired
✔ Zookeeper running
✔ Kafka broker running
✔ Topic created
✔ Producer working
✔ Consumer working
✔ End‑to‑end pipeline validated

13. License
MIT License
