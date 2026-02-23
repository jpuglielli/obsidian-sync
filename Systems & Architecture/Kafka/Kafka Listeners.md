https://www.confluent.io/blog/kafka-listeners-explained/
TLDR: You need to set `advertised.listeners` (or `KAFKA_ADVERTIZED_LISTENERS` if using [[Docker]] images) to the external address (host/IP) so that clients can correctly connect to it. Otherwise, they will try to connect to the internal host address![[Pasted image 20260221102255.png]]
## Background: Kafka's Protocol 
Kafka communicates over a **custom TCP protocol** (not HTTP). When configuring listeners, you specify both the address to bind to *and* the security protocol to use, analogous to how HTTP vs HTTPS specifies security on top of TCP.
## Security Protocols 
The four protocols are a matrix of auth vs encryption: 

|                   | **No auth** | **Auth (SASL)** |
| ----------------- | ----------- | --------------- |
| **No Encryption** | PLAINTEXT   | SASL_PLAINTEXT  |
| **Encryption**    | SSL         | SASL_SSL        |

**SASL** = Simple Authentication and Security Layer — a framework for proving identity, separate from encryption. Production systems use `SASL_SSL`.
## Listeners vs Advertised Listeners
- LISTENERS are what interfaces Kafka binds to
- ADVERTISED_LISTENERS are how clients can connect
**In particular**: (for Docker config, equivalents are shown in the indented list)
```
KAFKA_LISTENERS: LISTENER_BOB://kafka0:29092,LISTENER_FRED://localhost:9092
KAFKA_ADVERTISED_LISTENERS: LISTENER_BOB://kafka0:29092,LISTENER_FRED://localhost:9092
KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: LISTENER_BOB:PLAINTEXT,LISTENER_FRED:PLAINTEXT
KAFKA_INTER_BROKER_LISTENER_NAME: LISTENER_BOB
```
- KAFKA_LISTENERS is a comma-separated list of listeners and the host/IP and Kafka port to which Kafka binds to for listening. For more complex networking, this might be an IP address associated with a given network interface on a machine. The default is 0.0.0.0, which means listening on all interfaces.
    - listeners
- KAFKA_ADVERTISED_LISTENERS is a comma-separated list of listeners with their host/IP and port. This is the metadata that’s passed back to clients.
    - advertised.listeners
- KAFKA_LISTENER_SECURITY_PROTOCOL_MAP defines key/value pairs for the security protocol to use per listener name.
    - listener.security.protocol.map
- KAFKA_INTER_BROKER_LISTENER_NAME is the listener used to communicate between brokers, usually on the same internal network. The host/IP must be accessible from the broker machine to others
	- inter.broker.listener.name
## Listener Security Protocol Map 
Since the listener *name* and *protocol* are separate concepts, you need to map them explicitly:
- ``` KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,CONTROLLER:PLAINTEXT ``` 
Format is `LISTENER_NAME:PROTOCOL`. The name and protocol can differ — e.g. you could name a listener `EXTERNAL` but map it to `SASL_SSL`.
## Local Dev vs Prod 
- Local dev: `PLAINTEXT` is fine, advertised listeners can use Docker hostnames 
- Prod: use `SASL_SSL`, advertised listeners must be real resolvable addresses