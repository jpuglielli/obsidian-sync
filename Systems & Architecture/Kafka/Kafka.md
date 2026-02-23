Yes, Kafka is fundamentally just a Java application — a JAR file that you run on a JVM.

What that process contains depends on the `KAFKA_PROCESS_ROLES`. For a `broker,controller` node like yours, the JVM process is running roughly:

**As a broker:**
- A network layer listening on configured ports, implementing Kafka's custom TCP protocol
- A request handler that processes produce/fetch/metadata requests from clients
- A log manager that reads/writes partition data to disk (those segment files in your `kafka-data` volume)
- A replica manager that handles replication between brokers (in a multi-node cluster)

**As a controller:**
- The Raft consensus implementation — managing an event log of all cluster metadata changes
- A metadata cache that other brokers pull from to stay up to date on partition leadership, topic configs, etc.