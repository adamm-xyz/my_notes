
# System Design Interview - Chapter 6

## Design a Key-Value Store
A non-relational database where each key is a unique ID with an associated value.
- Keys can be plain text or hashed values
- Values can be strings, list, objects, etc
- two ops: put(key,val) & get(key)

### Understand the problem, establish design scope
- Balance tradeoffs with read, write and mem usage
- Also between consistency and availability

Example in book has these characteristics:
- Size of key-val pair < 10 KB
- Ability to store big data
- High availability, scalability
- Automatic scaling
- Tunable consistency
- Low latency

### Single server key-value store
- Easy with in memory hash table but won't scale
- Memory access is fast but space is constrained
- Data compression, storing frequenty used data on disk can help
- Still need distributed system to support big data

## Distributed key-value store & CAP Theory
**CAP Theorem** - Consistency, Availability, Partition Tolerance theorem

**Consistency** - all clients see same data at same time, no matter the node connected to

**Availability** - even if a some nodes are down, any client which requests data gets a response

**Partition Tolerance** - system continues to run despite network break between nodes

The CAP theorem - You can choose 2. In real world applications, can't sacrifice **P**.

## Real world example
- We must choose between consistency and availability
- If a node goes down...
- Consistency means block all write operations to avoid data inconsistency
- This makes the system unavailable
- Availability means keep accepting reads even tho it might return stale data
- Keep accepting writes and sync data when node comes back online
- Choosing right CAP is crucial

### Data Partition
- Infeasible to fit complete data on single server
- Must split the data into smaller portions, store across servers
- Distribute across servers evenly (challenge)
- Minimize data movement when nodes added/removed (challenge)
- Consistent hashing (from Ch 5) solves these challenges

**Automatic scaling** - servers added/removed automatically based on load

**Heterogeneity** - number of virtual nodes for a server is proportional to server capacity

### Data Replication
- For high availability, reliability data must be replicated async over N servers
- After key mapped to position on hash ring
- walk clockwise from there
- choose first N servers to store data copies
- Only choose unique servers
- Replicas placed in distinct data centers connected thru high-speed networks

### Consistency
- 
