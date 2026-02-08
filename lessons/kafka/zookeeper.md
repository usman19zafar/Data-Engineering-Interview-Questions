Zookeeper Troubleshooting Guide (Windows, Kafka 3.x)
Author: Usman Zafar
Purpose: Mechanical RCA + deterministic fixes for all Zookeeper startup failures on Windows.

1. Zookeeper Not Running (Most Common)
Symptom
Kafka broker logs show:

Code
Connection refused: localhost:2181
SessionExpiredException
Timed out waiting for connection while in state: CONNECTING
Root Cause
Zookeeper was not started before Kafka.

Fix
Start Zookeeper first:

Code
cd C:\Kafka\bin\windows
zookeeper-server-start.bat ..\..\config\zookeeper.properties
Kafka must be started only after Zookeeper is fully running.

2. Port 2181 Already in Use
Symptom
Zookeeper window shows:

Code
Address already in use: bind
Root Cause
Another process is using port 2181.

Fix
Check which process is using the port:

Code
netstat -ano | findstr 2181
Kill the process:

Code
taskkill /PID <pid> /F
Restart Zookeeper.

3. Zookeeper Data Directory Missing or Corrupted
Symptom
Zookeeper fails with errors referencing:

Code
dataDir
myid
jute.maxbuffer
Root Cause
The folder defined in zookeeper.properties does not exist or is corrupted.

Default:

Code
dataDir=/tmp/zookeeper
Fix
Create the directory manually:

Code
C:\tmp\zookeeper
Or update the config to a Windows‑friendly path:

Code
dataDir=C:/Kafka/zookeeper-data
Restart Zookeeper.

4. Zookeeper Fails Due to Java Issues
Symptom
Zookeeper window shows:

Code
java not found
Unsupported major.minor version
Root Cause
PATH or JAVA_HOME is wrong.

Fix
Ensure:

Code
java -version
returns your Adoptium JDK.

Ensure System PATH includes:

Code
C:\Program Files\Eclipse Adoptium\jdk-17.0.17.10-hotspot\bin
C:\Windows\System32
C:\Windows
Restart Zookeeper.

5. Zookeeper Logs Cannot Be Written (Windows Permission Issue)
Symptom
Zookeeper prints:

Code
Failed to rename log file
Access denied
Root Cause
Windows blocks log rotation in:

Code
C:\Kafka\logs
Fix
Run CMD as Administrator  
OR
Change log directory in log4j.properties:

Code
log4j.appender.ROLLINGFILE.File=C:/Kafka/zookeeper-logs/zookeeper.log
Create the folder manually.

Restart Zookeeper.

6. Zookeeper Starts Then Immediately Shuts Down
Symptom
Zookeeper window opens → prints a few lines → closes.

Root Cause
Missing or invalid myid file (for multi‑node configs).

Fix
For single‑node setups, remove the entire data directory:

Code
C:\tmp\zookeeper
Zookeeper will recreate it.

Restart Zookeeper.

7. Zookeeper Using IPv6 Instead of IPv4
Symptom
Kafka logs show:

Code
Opening socket connection to server localhost/[0:0:0:0:0:0:0:1]:2181
Root Cause
Windows resolves localhost to IPv6.

Fix
Force IPv4 in zookeeper.properties:

Code
clientPortAddress=127.0.0.1
Restart Zookeeper.

8. Zookeeper Fails Because of CRLF Line Endings
Symptom
Zookeeper prints weird parsing errors.

Root Cause
Windows Notepad saved config files with CRLF.

Fix
Open zookeeper.properties in VS Code → bottom‑right → change:

Code
CRLF → LF
Save and restart.

9. Zookeeper Fails Because Java Heap Too Small
Symptom
Zookeeper prints:

Code
OutOfMemoryError
Root Cause
Default heap is too small on some Windows setups.

Fix
Edit:

Code
C:\Kafka\bin\windows\zookeeper-server-start.bat
Add:

Code
set KAFKA_HEAP_OPTS=-Xmx512M -Xms512M
Restart Zookeeper.

10. Zookeeper Starts Successfully (Healthy Indicators)
Expected Healthy Output
Code
binding to port 0.0.0.0/0.0.0.0:2181
Started AdminServer on address 0.0.0.0:8080
If you see these → Zookeeper is healthy.

11. Full Startup Sequence (Correct Order)
1. Start Zookeeper
Code
zookeeper-server-start.bat ..\..\config\zookeeper.properties
2. Start Kafka Broker
Code
kafka-server-start.bat ..\..\config\server.properties
3. Create Topic
Code
kafka-topics.bat --create --topic test --bootstrap-server localhost:9092
4. Producer
Code
kafka-console-producer.bat --broker-list localhost:9092 --topic test
5. Consumer
Code
kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic test --from-beginning
12. Repo‑Ready Conclusion
This troubleshooting guide covers all deterministic Zookeeper failures on Windows:

Port conflicts

Missing data directories

PATH/JAVA_HOME issues

Log rotation failures

IPv6 resolution

CRLF corruption

Heap memory issues

Startup order mistakes

This is the complete RCA + fix set for Zookeeper in Kafka 3.x on Windows.
