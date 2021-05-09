## Eventual consistency
Microservices can duplicate data from other services. If data is changed, sync event should be initialized. The speed of propogation of changes depends on criticality of consistency and should be clear and configurable.

When choosing a NoSQL system, it is important to understand whether a choice of consistency policy is
available. If the database system only supports eventual consistency, then the application will need to handle
the possibility of reading stale (inconsistent) data.

## CAP and ACID
A system that is not tolerant to network partitions can achieve data consistency and availability, and often does so by using transaction protocols. To make this work, client and storage systems must be part of the same environment; they fail as a whole under certain scenarios, and as such, clients cannot observe partitions. An important observation is that in larger distributed-scale systems, network partitions are a given; therefore, consistency and availability cannot be achieved at the same time. This means that there are two choices on what to drop: relaxing consistency will allow the system to remain highly available under the partitionable conditions, whereas making consistency a priority means that under certain conditions the system will not be available.

In principle the consistency property of transaction systems as defined in the ACID properties (atomicity, consistency, isolation, durability) is a different kind of consistency guarantee. In ACID, consistency relates to the guarantee that when a transaction is finished the database is in a consistent state; for example, when transferring money from one account to another the total amount held in both accounts should not change. In ACID-based systems, this kind of consistency is often the responsibility of the developer writing the transaction but can be assisted by the database managing integrity constraints.

## Service Mesh
We have two services, service A that wants to call service B, and it can be in any language. Consider that this is our application workload. A service mesh uses sidecar controllers and injects a proxy next to our service. You will end up with two containers in the pod. The proxy is a transparent one, and your application is completely unaware that there is a proxy - that is intercepting all incoming and outgoing traffic. Furthermore, the proxy also acts as a data firewall

## Backends for Frontends
Agregation of services in several gateway services. Each for different platform: mobile, web for clients, admin panel, etc. These BFFs should only contain behavior specific to delivering a particular user experience.

## The Reasons to Split the Monolith
- Pace of Change: changes can be done faster
- Team Structure: full ownership of the code on which each team works on the most
- Security: additional protections to some services in terms of monitoring, protection of data at transit, and protection of data at rest
- Technology: we can use different stack for different services

## Notes
- If you find yourself repeatedly changing two services together, that's a sign that they should be merged
- Вead letter queue - a separate queue where messages got sent if they failedabd ran out of retries
- Postel’s Law (robustness principle): “Be conservative in what you do, be liberal in what you accept from others.” Be a tolerant reader
- Prefer choreography over orchestration.