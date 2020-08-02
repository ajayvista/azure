# CosmosDB

- Cosmos DB container is a schema-free collection of JSON items.
- The relations within and across container items are implicitly
   captured by containment, not by primary key and foreign key
   relations.
- Azure Cosmos DB uses a pricing model that charges on provisioned
   throughput and the storage that you consume.
   
 - Provisioned throughput = specified as Request Units per second
   (RU/s), allows you to read from or write data into containers or
   databases.
   
 - Each database or a container is billed on an hourly basis for the
   throughput provisioned in the units of 100 RU/s, with a minimum of
   400 RU/s, and storage consumed in GBs.
   
  - Unlike provisioned throughput, storage is billed on a consumption
   basis. You are billed only for the storage you consume.

**Benefits**

The Cosmos DB offers the following benefits:
**Guaranteed throughput** - *The performance level of Cosmos DB can be scaled elastically by setting Request Units (RUs).*
**Global distribution and Always On** - *Cosmos DB enables 99.999% read/write availability around the world. The Cosmos DB multi-homing API is an additional feature to configure the application to point to the closest datacenter for low latency and better performance.*
**Multiple query model or API**  - *Cosmos DB supports many APIs to work with the data stored in your Cosmos database.* 
**Choices of consistency modes** - *The Azure Cosmos DB replication protocol offers five well-defined, practical, and intuitive consistency models. Each model has a trade-off between consistency and performance.*
**No schema or index management** - *The database engine is entirely schema agnostic. Cosmos DB automatically indexes all data for faster queries response.*

Azure Cosmos account is a logical construct that has a globally unique DNS name.
**Azure Cosmos DB—Multi-Write Replicas** *You cannot turn off or disable the multi-region writes after it’s enabled.*
- Cosmos DB also automatically back up your database every four hours and stores it in the GRS blob storage for disaster recovery (DR). 
- At any given time, at least last two backups are retained for 30 days.
- Queries that access data within a single logical partition are more cost-effective than queries that access multiple partitions. You must be very mindful when choosing a partition key to query data efficiently and avoid “Hot Spots“ within a partition.
- A single partition has an upper limit of 10 GB of storage.
- You can create 100 Azure Cosmos accounts under one Azure subscription
- Azure Cosmos DB containers have a minimum throughput of 400 request units per second (RU/s). RUs are a blended measure of computational cost (CPU, memory, disk I/O, network I/O). 1RU corresponds to the throughput of reading of a 1 KB document. All requests on the partition are charged in the form of RUs. If the request exceeds the provisioned RUs on the partition, Cosmos DB throws RequestRateTooLargeException or HTTP Status code 429.

**General**
- Globally distributed and add/remove regions w/o downtime
- Multimaster
- Automatic and manual failover
- Account is logical object and is container for DBs and ends with documents.azure.com
- Logical setup - Account -> Container -> Contents
- Increment 100 RU which each RU is 1KB
- API is determined at account level

**Consistency Level**
- **Strong**
	- A strong consistency level ensures no dirty reads, and the client always reads the latest version of committed data across the multiple read replicas in single or multi-regions. 
	- The trade-off going with the strong consistency option is the performance. 
	- When you write to a database, everyone waits for Cosmos DB to serve the latest writes after it has been saved across all read replicas.

	- To meet the requirements of the scenario, you should choose Strong consistency which satisfies the following requirements:
		- A linearizability guarantee. (Linearizability refers to serving requests concurrently).
		- The reads are guaranteed to return the most recent committed version of an item.
		- A client never sees an uncommitted or partial write.
		- Users are always guaranteed to read the latest committed write. 
- **Bounded Staleness** - you choose for # week or time
	- You can specify the lag by an x version of updates of an item or by time interval T by which read lags behind by a write. The typical use case for you to go with bounded staleness is to guarantee low latency for writes for globally distributed applications.
	
	- Boundless Staleness is dependant on the number of versions of an item and the time interval since the last update and therefore cannot consistently give an RPO of 15 minutes or less. 
	
- **Session** - consistent within a client session
	- Session ensures that there are no dirty reads on the write regions. A session is scoped to a client session, and the client can read what they wrote instead having to wait for data to be globally committed.
	
- **Consistent Prefix** - reads never see out of order writes
	- Consistent prefix guarantees that reads are never out of order of the writes. For example, if an item in the database was updated three times with versions V1, V2, and V3, the client would always see V1, V1V2, or V1V2V3. The client would never see out of order like V2, V1V3, or V2V1V3.
	
- **Eventual consistency**
	- You probably use eventual consistency when you’re least worried about the freshness of data across the read replicas and the order of writes over time, but you need the highest level of availability and low latency.


Session, Consistent Prefix and Eventual Consistency levels will provide a recovery point objective (RPO) or 15 minutes or less when configured in a Multi-Master Replication mode with two regions. 

Strong consistency and multi-master Cosmos accounts configured for Multi-Master Replication Mode cannot be configured for strong consistency as it is not possible for a distributed system to provide an RPO of zero and an RTO of zero.

With Azure Cosmos DB, developers can choose from five well-defined consistency models on the consistency spectrum. From strongest to more relaxed, the models include strong, bounded staleness, session, consistent prefix, and eventual consistency. The models are well-defined and intuitive and can be used for specific real-world scenarios. Each model provides availability and performance tradeoffs and is backed by the SLAs. The following image shows the different consistency levels as a spectrum.

The consistency levels are region-agnostic and are guaranteed for all operations regardless of the region from which the reads and writes are served, the number of regions associated with your Azure Cosmos account, or whether your account is configured with a single or multiple write regions.

**API**
- Core (SQL)
- Cassandra
- Gremlin - Graph
- Azure Table - returns as table

**Partitioning**
- Single partition limited to 10GB
- Partitions limited to 400 requests/s (RU)
- For unique key choose property you filter on
- Partition strategy - This scaling strategy is called scale out or horizontal scaling.
	- A partition key defines the partition strategy, it's set when you create a container and can't be changed. Selecting the right partition key is an important decision to make early in your development process. It should aim to evenly distribute operations across the database to avoid hot partitions. **A hot partition is a single partition that receives many more requests than the others, which can create a throughput bottleneck.**

Business Continuity and Disaster RecoveryFor high availability, it’s recommended that configure Cosmos DB with multiregion writes (at least two regions). In the event of a regional disruption, the failover is instantaneous, and the application doesn’t have to undergo any change; it transparently happens behind the scene. Also, if you’re using the default consistency level of strong, there will not be any data loss before and after the failover. For bounded staleness, you may encounter a potential data loss up to the lag (time or operations) you’ve set up. For the Session, Consistent Prefix, and Eventual consistency options, the data loss could be up to a maximum of five seconds.

Cosmos DB also gives you an ability to write server-side code using stored procedures, user-defined functions (UDFs), and triggers. These are essentially JavaScript functions written within the Cosmos DB database and scoped at the container level.
Session, Consistent Prefix and Eventual Consistency levels will provide a recovery point objective (RPO) or 15 minutes or less when configured in a Multi-Master Replication mode with two regions.

RTO is how quickly the applications will be online in the secondary site, and RPO is how much data loss would occur in the event of a failover. ... Azure Site Recovery can provide RTO and RPO within minutes out of the box – and at a comparatively low cost. "
"Stored procedures and triggers are scoped at partition key and must be supplied with an input parameter for the partition key, whereas UDFs are scoped at the database level.

■   Stored procedures and triggers guarantee atomicity (ACID) like in any relational database. Transactions are automatically rolled back by Cosmos DB in case of any exception; otherwise, they’re committed to the database as a single unit of work.
■   Queries using stored procedures and triggers are always executed on the primary replica as these are intended for write operations to guarantee strong consistency for secondary replicas, whereas UDFs can be written to the primary or secondary replica.
■   The server-side code must complete within the specified timeout threshold limit, or you must implement a continuation batch model for long-running code. If the code doesn’t complete within the time, Cosmos DB roll back the whole transaction automatically.

- There are two types of triggers you can set up:
	- **Pre-triggers** As the name defines, you can invoke some logic on the database containers before the items are created, updated, or deleted.
	- **Post-triggers** Like pre-triggers, these are also tied to an operation on the database; however, post-triggers are run after the data is written or updated in the database.

**Abbreviation**
RPO - Recovery Point
RTO - Recovery Time
RU - Request Unit
UDF - User-defined functions
GRS - Geo-redundant storage