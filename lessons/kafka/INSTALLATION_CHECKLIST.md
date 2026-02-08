Kafka + Zookeeper Installation Checklist (Windows)
Author: Usman Zafar
Purpose: Deterministic, step‑by‑step checklist to ensure a clean Kafka installation on Windows.

1. Java Installation
Verify JDK
[ ] Install Adoptium JDK 17 (HotSpot)

[ ] Run java -version

[ ] Output shows JDK 17 from Adoptium

Verify JAVA_HOME
[ ] JAVA_HOME points to:

Code
C:\Program Files\Eclipse Adoptium\jdk-17.0.17.10-hotspot
2. Windows PATH Validation
Required PATH entries
[ ] C:\Windows\System32

[ ] C:\Windows

[ ] C:\Windows\System32\Wbem

[ ] C:\Windows\System32\WindowsPowerShell\v1.0\

[ ] C:\Program Files\Eclipse Adoptium\jdk-17.0.17.10-hotspot\bin

Remove Oracle symlink
[ ] Delete:

Code
C:\Program Files\Common Files\Oracle\Java\javapath
Verify PATH
[ ] where java returns the Adoptium JDK path

[ ] where where returns C:\Windows\System32\where.exe

3. Kafka Installation
Extract Kafka
[ ] Extract Kafka to:

Code
C:\Kafka
Verify folder structure
[ ] C:\Kafka\bin\windows exists

[ ] C:\Kafka\config exists

[ ] C:\Kafka\libs exists

4. Zookeeper Startup
Start Zookeeper
[ ] Run:

Code
zookeeper-server-start.bat ..\..\config\zookeeper.properties
Healthy indicators
[ ] “binding to port 0.0.0.0:2181”

[ ] “Started AdminServer”

[ ] No errors

5. Kafka Broker Startup
Start Kafka
[ ] Run:

Code
kafka-server-start.bat ..\..\config\server.properties
Healthy indicators
[ ] “Connected to Zookeeper”

[ ] “Registered broker 0”

[ ] “Awaiting socket connections on 0.0.0.0:9092”

[ ] “Kafka version: 3.x.x”

6. Topic Creation
[ ] Run:

Code
kafka-topics.bat --create --topic test --bootstrap-server localhost:9092
7. Producer Test
[ ] Run:

Code
kafka-console-producer.bat --broker-list localhost:9092 --topic test
[ ] Type:

Code
uzi
qzi
8. Consumer Test
[ ] Run:

Code
kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic test --from-beginning
[ ] Output shows:

Code
uzi
qzi
9. Final Validation
[ ] Zookeeper running

[ ] Kafka broker running

[ ] Producer working

[ ] Consumer working

[ ] End‑to‑end pipeline validated

End of INSTALLATION_CHECKLIST.md
