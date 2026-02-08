THE FOUR-WINDOW MODEL (MECHANICAL DESIGN)
Kafka on Windows requires four separate processes, each in its own CMD window:

Code
[Window 1] Zookeeper
[Window 2] Kafka Broker
[Window 3] Producer
[Window 4] Consumer
This is not optional — it is the canonical architecture for Kafka 3.x with Zookeeper.


WHAT EACH WINDOW DOES
1. Zookeeper (Window 1)
Manages metadata

Tracks broker registration

Handles controller election

Must be running before Kafka

2. Kafka Broker (Window 2)
Accepts producer messages

Stores logs

Serves consumer fetch requests

Talks to Zookeeper

3. Producer (Window 3)
Sends messages to the broker

You type:

Code
uzi
qzi
4. Consumer (Window 4)
Reads messages from the broker

Displays:

Code
uzi
qzi
WHY FOUR WINDOWS ARE REQUIRED (LOGIC)
Kafka is not a single program — it is a distributed system, even on one machine.

Each component is a separate JVM process:

Zookeeper JVM

Kafka Broker JVM

Producer JVM

Consumer JVM

Windows cannot run them in the same terminal, so each gets its own window.

This is normal, correct, and exactly how Kafka is designed.
