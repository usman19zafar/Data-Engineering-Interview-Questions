Kafka + Zookeeper Troubleshooting Guide (Windows)
Author: Usman Zafar
Purpose: Deterministic RCA + mechanical fixes for all common failures in Kafka 3.x on Windows.

1. Zookeeper Not Running
Symptom
Code
Connection refused: localhost:2181
SessionExpiredException
Timed out waiting for connection
Root Cause
Zookeeper was not started before Kafka.

Fix
Code
cd C:\Kafka\bin\windows
zookeeper-server-start.bat ..\..\config\zookeeper.properties
2. Port 2181 Already in Use
Symptom
Code
Address already in use: bind
Fix
Code
netstat -ano | findstr 2181
taskkill /PID <pid> /F
3. Kafka Broker Cannot Connect to Zookeeper
Symptom
Code
KafkaServer: Timed out waiting for connection
Fix
Ensure Zookeeper is running and reachable on localhost:2181.

4. Java Not Found / Wrong Java Version
Symptom
Code
java not recognized
Unsupported major.minor version
Fix
Use Adoptium JDK 17:

Code
java -version
Ensure PATH includes:

Code
C:\Program Files\Eclipse Adoptium\jdk-17.0.17.10-hotspot\bin
5. Windows PATH Corrupted
Symptom
Code
'where' is not recognized
Fix
Recreate System PATH:

Code
C:\Windows\System32
C:\Windows
C:\Windows\System32\Wbem
C:\Windows\System32\WindowsPowerShell\v1.0\
C:\Program Files\Eclipse Adoptium\jdk-17.0.17.10-hotspot\bin
6. Log Rotation Errors
Symptom
Code
Failed to rename log-cleaner.log
Access denied
Fix
Run CMD as Administrator
OR
Change log directory in log4j.properties.

7. Consumer Not Receiving Messages
Root Causes
Wrong topic

Wrong bootstrap server

Producer not sending

Broker not running

Fix
Validate with the uzi/qzi test.

8. Producer Hangs
Fix
Ensure broker is running:

Code
localhost:9092
9. Zookeeper Data Directory Missing
Fix
Create:

Code
C:\tmp\zookeeper
10. IPv6 Resolution Issues
Fix
Add to zookeeper.properties:

Code
clientPortAddress=127.0.0.1
