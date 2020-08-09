# Azure Service Bus

Azure Service Bus allows you to send information between applications and services and is designed for high value transactional applications. Microsoft Azure Service Bus is a fully managed enterprise integration message broker.  Service Bus is most commonly used to decouple applications and services from each other, and is a reliable and secure platform for asynchronous data and state transfer.  Data is transferred between different applications and services using messages.

It is a messaging solution and provides enterprise messaging capabilities, including queuing, publish/subscribe, and an advanced integration patterns model for an application hosted on the cloud or on-premises. It has the following key features:
- Message persistence and duplicate detection
- First-in-first-out order of message delivery
- Poison message handling
- High availability, geo-replication, and built-in disaster recovery
- Transactional integrity, an ability to have queued read or write messages as part of a single transaction
- Supports advanced messaging protocol like HTTP, AMQP 1.0, and significant industry languages (.NET, Java, Python, Go, Node.js, and Ruby)

**General**
Basic -> shared capacity, 256KB message size, queues, variable pricing
Standard -> same as basic but topics, message operations
Premium -> standard and dedicated capacity, 1024KB message size, georecovery

**Queue**
Max size 1GB - 5GB
Message TTL (default 14 days)
Lock duration (default 60 sec)
Duplicate detection
Dead letter queue
Sessions (FIFO)
High throughput (partitioning)

**Topic**
Max size 1GB - 5GB
Message TTL
Duplicate detection
Partitioning
Subscriber can be filtered to certain operations in a topic
