
# System Design Interview - Chapter 1 Summary

## Single Server Setup
- All services (web app, database, cache) run on a single server.
- Basic flow:  
  `DNS → IP → HTTP request → Server → HTML/JSON response`
- Traffic originates from:
  - **Web apps**: server-side logic (Java, Python) + client-side UI (HTML, JavaScript).
  - **Mobile apps**: use HTTP APIs, expect JSON responses.

## Database Tiering
As user load increases, isolate the **web tier** and **data tier** to scale each independently.

### Relational (SQL)
- Structured, tabular data. Supports JOINs.
- Examples: MySQL, PostgreSQL, Oracle.

### Non-Relational (NoSQL)
- Stores data in formats like key-value, document, column, or graph.
- Better for:
  - Low-latency access
  - Unstructured data
  - High serialization needs
  - Massive datasets
- Examples: CouchDB, Cassandra, Neo4j, DynamoDB.

## Scaling Approaches

### Vertical Scaling ("Scale Up")
- Add more RAM/CPU to one server.
- Simple, but introduces a **single point of failure (SPOF)**.
- High-performance servers are expensive.

### Horizontal Scaling ("Scale Out")
- Add more machines.
- Enables high availability and fault tolerance.
- Supports larger, distributed systems.

## Load Balancer
- Distributes incoming requests across multiple servers.
- Clients communicate with the load balancer, not directly with web servers.
- Improves availability, failover, and scalability.
- Uses private IPs for backend communication.

## Database Replication
### Master-Slave Model
- **Master** handles all **writes**.
- **Slaves** handle **reads**.

#### Benefits:
- Scalability (parallel read handling)
- High availability
- Failover: if a slave or master goes down, roles can be reassigned temporarily.

## Caching
- Stores frequently accessed or computationally expensive data for faster responses.

### Read-through cache:
1. Client requests data.
2. Cache is checked first.
3. If miss: DB fetch → cache is updated → return data.

### Cache properties:
- Use for: frequently read, infrequently modified data.
- Requires:
  - **Expiration policy** (TTL) to avoid stale data.
  - **Eviction policy** (LRU, LFU) to remove unused data.
  - **Consistency mechanism** to sync cache and DB.
  - Cache clustering to avoid SPOFs.

## Content Delivery Network (CDN)
- Distributed caching for **static content** (e.g., images, scripts).
- Hosted by third-party providers; cost based on bandwidth.
- Requires:
  - Cache control and expiry headers.
  - Fallback routing to origin server if CDN fails.

## Stateless Web Tier
- Move all session/state data to an external, shared data store (e.g., Redis, DB).
- Allows any server to handle any request (enables scaling and fault tolerance).

## Multi-Data Center Deployment
- Route traffic to geographically closest data center (using **GeoDNS** or similar).
- In case of regional failure, reroute to a healthy data center.

### Challenges:
- Syncing data across regions.
- Ensuring consistency across caches and databases.
- Requires testing across geographies for consistency.

## Message Queue (Asynchronous Processing)
- Producers push tasks to a **message queue** 
- Consumers pull and process them asynchronously.

### Benefits:
- Decouples producers from consumers.
- Each can scale independently.
- Tasks can be retried, queued, or delayed.

## Monitoring & Automation
- **Logging**: Capture errors and system behavior.
- **Metrics**: Track performance, uptime, throughput, etc.
- **Automation**: Crucial for deploying and managing large-scale systems efficiently (e.g., auto-scaling, CI/CD).

## Advanced Database Scaling (Sharding)
- **Horizontal partitioning** of a database into **shards**.
- Each shard has:
  - Same schema
  - Different subset of data

### Sharding details:
- Use a **sharding key** to determine data distribution.

### Risks:
- **Resharding** may be required when shards are unevenly loaded.
- **Celebrity problem**: a heavily accessed user/data item overloads one shard.
- **Join and denormalization**: hard to perform ops across shards, data must be renormalized.

## Summary for scaling
- keep web tier stateless
- build redundancy at every tier
- cache data as much as you can
- support multiple data centers
- host static assets in CDN
- scale data tier w/ sharding
- split tiers into individual services
- monitor system and use automation tools
