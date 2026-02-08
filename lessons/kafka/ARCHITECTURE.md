ARCHITECTURE.md
Kafka + Zookeeper Architecture
Author: Usman Zafar
Purpose: ASCII architecture diagram + component roles.

1. High‑Level Architecture (ASCII)
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
2. Component Roles
Zookeeper
Stores metadata

Tracks brokers

Manages controller

Coordinates topic configs

Kafka Broker
Accepts producer writes

Stores logs

Serves consumer reads

Manages partitions

Producer
Sends messages to broker

Consumer
Reads messages from broker

3. Four‑Window Operational Model
Code
Window 1 → Zookeeper
Window 2 → Kafka Broker
Window 3 → Producer
Window 4 → Consumer
End of ARCHITECTURE.md
