Kafka on Windows – Broken Java/PATH Recovery Story
1. Initial symptom
Problem: Kafka CLI scripts (kafka-console-producer.bat, etc.) were failing on Windows due to Java issues.

Kafka scripts depended on java being correctly resolved on PATH.

JAVA_HOME and PATH were not aligned with the real JDK.

2. First check – environment mismatch
Commands:

cmd
echo %JAVA_HOME%
where java
Findings:

JAVA_HOME was correct:

text
C:\Program Files\Eclipse Adoptium\jdk-17.0.17.10-hotspot
But where java showed:

text
C:\Program Files\Common Files\Oracle\Java\javapath\java.exe
Root cause (phase 1):

Windows PATH still pointed to the old Oracle javapath symlink.

That symlink overrode the correct JDK, causing Kafka to use the wrong Java.

3. Fix 1 – remove Oracle javapath from System PATH
Action:

Open System Properties:

text
Win + R → sysdm.cpl → Enter
Go to Advanced → Environment Variables….

Under System variables, select Path → Edit….

Remove entries like:

text
C:\Program Files\Common Files\Oracle\Java\javapath
C:\Program Files\Common Files\Oracle\Java\javapath_target_*
Ensure the correct JDK bin is present:

text
C:\Program Files\Eclipse Adoptium\jdk-17.0.17.10-hotspot\bin
Keep it near the top (Windows may keep a couple of system entries above it—that’s fine).

4. New problem – PATH corruption
After editing PATH, a new error appeared:

text
'where' is not recognized as an internal or external command,
operable program or batch file.
Root cause (phase 2):

The System PATH variable itself was effectively broken/missing.

Critical Windows directories (like C:\Windows\System32) were no longer in PATH.

Result: even core tools like where stopped working.

5. Fix 2 – System PATH was missing and had to be recreated
Observation:

Under System variables, there was no Path variable at all.

Only other variables like ComSpec, DriverData, PSModulePath, PATHEXT, etc.

Action – recreate System PATH from scratch:

Open:

text
Win + R → sysdm.cpl → Advanced → Environment Variables…
Under System variables, click New….

Create a new variable:

Variable name:

text
Path
Variable value:

text
C:\Windows\System32;
C:\Windows;
C:\Windows\System32\Wbem;
C:\Windows\System32\WindowsPowerShell\v1.0\;
C:\Program Files\Eclipse Adoptium\jdk-17.0.17.10-hotspot\bin;
Click OK → OK → OK to save.

This restored:

Core Windows tools (where, ping, ipconfig, etc.).

PowerShell.

Correct JDK on PATH.

6. Verification – mechanical closure
Open a new CMD and run:

cmd
where where
where java
java -version
Expected and achieved:

text
C:\Windows\System32\where.exe
C:\Program Files\Eclipse Adoptium\jdk-17.0.17.10-hotspot\bin\java.exe

openjdk version "17.0.17" 2025-10-21
OpenJDK Runtime Environment Temurin-17.0.17+10 (build 17.0.17+10)
OpenJDK 64-Bit Server VM Temurin-17.0.17+10 (build 17.0.17+10, mixed mode, sharing)
This confirmed:

System32 is back on PATH.

Correct JDK is resolved.

Java is functional.

7. Kafka CLI – final validation
From a CMD window:

cmd
cd C:\Kafka\bin\windows
.\kafka-console-producer.bat --help
Result:

Full help text printed successfully.

Windows Defender Firewall prompted for access → Allowed on Private networks.

Kafka CLI is now fully operational.

Summary for repo
Problem:  
Kafka on Windows failed due to:

PATH pointing to Oracle javapath instead of the real JDK.

System PATH later becoming corrupted/missing, removing C:\Windows\System32 and breaking core commands.

Fix sequence:

Verified mismatch between JAVA_HOME and where java.

Removed Oracle javapath entries from System PATH.

Ensured correct JDK bin path was present.

Discovered System PATH was missing → recreated Path under System variables with:

C:\Windows\System32

C:\Windows

C:\Windows\System32\Wbem

C:\Windows\System32\WindowsPowerShell\v1.0\

C:\Program Files\Eclipse Adoptium\jdk-17.0.17.10-hotspot\bin

Verified with where where, where java, java -version.

Confirmed Kafka CLI works via kafka-console-producer.bat --help.
