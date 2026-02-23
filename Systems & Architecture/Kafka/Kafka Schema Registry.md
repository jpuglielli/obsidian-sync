https://docs.confluent.io/platform/current/schema-registry/fundamentals/schema-evolution.html
https://docs.confluent.io/platform/current/schema-registry/fundamentals/index.html#how-it-works
![[Pasted image 20260221111936.png]]
Schema Registry lives outside of and separately from your Kafka brokers.
- Consumers and producers talk directly to Kafka to publish/read messages from topics
- They talk to schema registry to send/receive schemas
