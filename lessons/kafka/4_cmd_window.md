THE FOUR-WINDOW MODEL (MECHANICAL DESIGN)
Kafka on Windows requires four separate processes, each in its own CMD window:

Code
[Window 1] Zookeeper
[Window 2] Kafka Broker
[Window 3] Producer
[Window 4] Consumer
This is not optional — it is the canonical architecture for Kafka 3.x with Zookeeper.
