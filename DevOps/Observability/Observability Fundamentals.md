Derived from Control Theory, a system is _observable_ if you can determine its internal state by examining its outputs.

Translated to software, observability is about **asking arbitrary new questions of your system without deploying new code**.
## The Three Pillars 
Observability is commonly described through three pillars:
- **Logs**: discrete events 
- **Metrics**: numeric measurements over time (think Prometheus/Grafana)
- [[Traces]]: the journey of a request across services 
## Observability vs [[Monitoring Fundamentals|Monitoring]] vs [[APM Fundamentals|APM]]
##### Observability
- Tells you WHY something is wrong
- About debugging unknown unknowns
- E.g. Why are these specific requests slow, for these users, under these conditions?
	- ad-hoc, exploratory
- A highly observable system lets you ask arbitrary new questions without deploying new code or instrumentation
##### Monitoring 
- Tells you WHEN something is wrong
- About debugging known unknowns
- You define metrics and thresholds upfront 
	- CPU > 90%
	- error rate > 1%
	- Kafka consumer lag > 10k 
- Get alerted when those conditions are met. 
- It's inherently _reactive_ and based on questions you've already thought to ask.