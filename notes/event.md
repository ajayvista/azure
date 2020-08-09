# Event

## Event Hub

Azure Event Hubs is a big data pipeline. It facilitates the capture, retention, and replay of telemetry and event stream data. The data can come from many concurrent sources. Event Hubs allows telemetry and event data to be made available to a variety of stream-processing infrastructures and analytics services. It is available either as data streams or bundled event batches. This service provides a single solution that enables rapid data retrieval for real-time processing as well as repeated replay of stored raw data. It can capture the streaming data into a file for processing and analysis. It has the following characteristics: low latency capable of receiving and processing millions of events per second at least once delivery

Azure Event Hubs enables you to automatically capture the streaming data in Event Hubs in an Azure Blob storage or Azure Data Lake Storage Gen 1 or Gen 2 account of your choice.

Event Hubs provides a distributed stream-processing platform with low latency and seamless integration with other ecosystems like Apache Kafka to publish events to Event Hubs without intrusive configuration or code changes. It has a support for advanced messaging protocol like HTTP, AMQP 1.0, Kafka 1.0, and major industry languages (.NET, Java, Python, Go, Node.js). The partition consumer model of the Event Hub makes it massively scalable. It allows you to partition the big data stream of events among the different partitions and enable parallel processing with an ability to give consumers its own partitioned event stream.

- Big data streaming platform and event ingestion service
- Can receive messages from millions of producers
- Cars, fitness trackers, thermostat, etc
- Support HTTPS and AMQP 1.0 protocol
- Messages can be partioned - delivered in the same order by partition
- Event Hub is used to collect many events for processing.
- Allows big data to be ingested from multiple IoT devices.

**General**
- Stream data to analytics
- Throughput units are pre-allocated or set to a maximum
- End with .servicebus.windows.net
- **Basic SKU** - 1 consumer group, 100 connections
- **Standard SKU** - 20 consumer group, 1000 connections, AZ, geo-recovery
- Namespace -> Event Hub -> Consumer Group

**Namespace**
- Shared access policy
- Geo-recovery (paired region)
- Firewall (allow Vnet, IP)
- Create event hub

**Entity**
- Shared access policy
- Enable/disable hub
- Partition count
- Message retention (7 days by default)

**Choose Event Hubs if:**
- You need to support authenticating a large number of publishers.
- You need to save a stream of events to Data Lake or Blob storage.
- You need aggregation or analytics on your event stream.
- You need reliable messaging or resiliency.

As Event Hubs receives communications, it divides them into partitions. Partitions are buffers into which the communications are saved. Because of the event buffers, events are not completely ephemeral, and an event isn't missed just because a subscriber is busy or even offline. The subscriber can always use the buffer to "catch up." By default, events stay in the buffer for 24 hours before they automatically expire.

The buffers are called partitions because the data is divided amongst them. Every event hub has at least two partitions, and each partition has a separate set of subscribers.

It can handle data from concurrent sources and route it to a variety of stream-processing infrastructures and analytics services. It enables real-time processing and supports repeated replay of stored raw data.

## Event Grid

Event Grid and Event Hubs both offer some similar capabilities, but each is designed to address a specific business scenario. Event Grid isn’t meant for queuing the data or storing it for later use. Instead, because of its nature of integration with Function Apps and Logic Apps, it’s meant for distributing events instantaneously and trigging application logic to react to application metrics to take some actions.

The Azure Event Grid service is best suited for use cases that act as a message broker to tie one or more subscribers to discrete event notifications coming from event publishers. The service is fully managed and uses a server-less computing model. It’s massively scalable. Event Grid then pushes those events instantaneously to Event Handlers, and it guarantees at least one delivery of the event messages for each subscription. To receive an event and act on it, you define an event handler on the other hand and tell Event Grid which event on the Topic should be routed to which handler. This is done using Event Subscription.

To ensure the access keys for a topic are secured and kept safe, consider placing them in a Key Vault as secrets. This way, the application that needs them can refer to the secret endpoint and avoid storing the application keys for the topic in any code. This prevents the keys from being visible in plain text and only available to the application at runtime.

Event subscriptions can collect any and all information sent to a topic, or they can be filtered in the following ways:
	- By Subject Allows filtering by the subject of messages sent to the topic—for example, only messages with .jpg images in them
	- Advanced filter A key value pair one level deep

Note: Advanced filters are limited to five per subscription.

- Azure Event Grid is a fully-managed event routing service running on top of Azure Service Fabric. Event Grid distributes events from different sources, such as Azure Blob storage accounts or Azure Media Services

- Event Grid sends an event to indicate something has happened or changed. However, the actual object that was changed is not part of the event data. Instead, a URL or identifier is often passed to reference the changed object.

**Use Event Grid when you need these features:**

**Simplicity**: It is straightforward to connect sources to subscribers in Event Grid.
**Advanced filtering**: Subscriptions have close control over the events they receive from a topic.
**Fan-out**: You can subscribe to an unlimited number of endpoints to the same events and topics.
**Reliability**: Event Grid retries event delivery for up to 24 hours for each subscription.
**Pay-per-event**: Pay only for the number of events that you transmit.

**General**
- Similar to CloudWatch Events and Lambda
- Event Sources -> Event Grid -> Event Handlers
- React to state changes
- Publisher/Subscriber model
- Limit is 64KB per event

**Handlers**
- Azure Automation
- Azure Functions
- Event Hub
- Hybrid Connections
- Logic App
- Microsoft Flow
- Queue Storage
- Webhook

**Sources**
- Azure Subscription / Resource Group (Management)
- Container Registry
- Custom Topics
- Event Hub
- IoT Hub
- Media Services
- Service Bus
- Storage Blob
- Azure Maps

Event Grids can create events when resources are removed.  An Event Grid Trigger can be used to send an email alert. To enable this functionality, you will first need to create a Logic App.

To monitor and respond to specific events that happen in Azure resources or third-party resources, you can automate and run tasks as a workflow by creating a logic app that uses minimal code. These resources can publish events to an Azure event grid. In turn, the event grid pushes those events to subscribers that have queues, webhooks, or event hubs as endpoints. As a subscriber, your logic app can wait for those events from the event grid before running automated workflows to perform tasks.

When you use the Azure Event Grid service you can create, read, update, or delete a resource. For example, you can monitor changes that might incur charges on your Azure subscription and affect your bill.  If you want the system to send emails about those changes you will create a logic app with an event subscription for an Azure resource, events flow from that resource through an event grid to the logic app.

