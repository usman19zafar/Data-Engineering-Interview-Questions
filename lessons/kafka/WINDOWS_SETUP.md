Kafka + Zookeeper Setup on Windows
Author: Usman Zafar
Purpose: Clean, mechanical setup instructions for Kafka 3.x on Windows.

1. Install Java (Adoptium JDK 17)
Download and install:

Code
Eclipse Adoptium JDK 17 (HotSpot)
Verify:

Code
java -version
2. Fix System PATH
Ensure PATH includes:

Code
C:\Windows\System32
C:\Windows
C:\Windows\System32\Wbem
C:\Windows\System32\WindowsPowerShell\v1.0\
C:\Program Files\Eclipse Adoptium\jdk-17.0.17.10-hotspot\bin
Remove:

Code
C:\Program Files\Common Files\Oracle\Java\javapath
3. Download Kafka
Extract Kafka to:

Code
C:\Kafka
4. Start Zookeeper
Code
cd C:\Kafka\bin\windows
zookeeper-server-start.bat ..\..\config\zookeeper.properties
5. Start Kafka Broker
Code
cd C:\Kafka\bin\windows
kafka-server-start.bat ..\..\config\server.properties
6. Create Topic
Code
kafka-topics.bat --create --topic test --bootstrap-server localhost:9092
7. Start Producer
Code
kafka-console-producer.bat --broker-list localhost:9092 --topic test
8. Start Consumer
Code
kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic test --from-beginning
9. Validate with uzi/qzi Test
Producer:

Code
uzi
qzi
Consumer output:

Code
uzi
qzi
