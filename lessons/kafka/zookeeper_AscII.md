Kafka + Zookeeper Architecture (ASCII Diagram)
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
                                          | 2181 (ZK protocol)
                                          |
                   -------------------------------------------------
                   |                                               |
                   v                                               v
        +----------------------+                        +---------------------+
        |      BROKER 0        |                        |      BROKER 1       |
        |----------------------|                        |---------------------|
        | - ID: 0              |                        | - ID: 1             |
        | - Listens: 9092      |                        | - Listens: 9093     |
        | - Registers with ZK  |                        | - Registers with ZK |
        | - Stores partitions  |                        | - Stores partitions |
        +----------+-----------+                        +----------+----------+
                   |                                               |
                   |                                               |
                   |                                               |
                   |                                               |
                   |                                               |
                   |                                               |
                   v                                               v
        +----------------------+                        +----------------------+
        |   PARTITION LEADER   |                        |  PARTITION FOLLOWER  |
        |----------------------|                        |----------------------|
        | - Handles reads      |                        | - Replicates leader  |
        | - Handles writes     |                        | - Takes over on fail |
        +----------------------+                        +----------------------+

                   ^                                               ^
                   |                                               |
                   |                                               |
                   |                                               |
                   |                                               |
                   |                                               |
        +----------+-----------+                        +----------+-----------+
        |   PRODUCER CLIENT    |                        |   CONSUMER CLIENT    |
        |----------------------|                        |----------------------|
        | - Sends messages     |                        | - Reads messages     |
        | - Uses broker list   |                        | - Uses group coord.  |
        +----------------------+                        +----------------------+

```
Two‑Line Explanation (Mechanical)
Zookeeper maintains metadata, broker registration, controller election, and topic configs.
Kafka brokers store partitions, replicate data, and serve producers/consumers.

Business Analogy (Your Style)
Zookeeper is the city hall keeping official records.
Kafka brokers are the shops doing real business.
Producers deliver goods; consumers pick them up.
