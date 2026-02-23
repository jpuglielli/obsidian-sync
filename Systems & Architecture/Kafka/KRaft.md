KRaft replaces Zookeeper as Kafka's metadata and consensus layer — moving cluster coordination *inside* Kafka rather than depending on an external system (zookeeper).
### What KRaft manages 
- Which brokers are alive 
- Which broker is the leader for each partition 
- Topic and partition metadata 
- ACLs and configs
### How it works 
A subset of brokers act as **controllers** and run the Raft consensus algorithm to agree on cluster state. One controller is the active leader; others are followers ready to take over.
### Process roles 
Nodes can be assigned roles via `KAFKA_PROCESS_ROLES`: 
- `broker` — stores and serves messages 
- `controller` — manages cluster metadata via Raft 
- `broker,controller` — both (fine for single-node dev, not recommended for prod)
