
# System Design Interview - Chapter 5

## Design Consistent Hashing
Consistent hashing is used to distribute requests/data across servers

### The rehashing problem
- Common method: server_index = hash(key) % N
- N is the size of the server pool
- Problem arises when N changes (pool increase, server down)
- Requests are sent to wrong servers, huge mess

### Consistent hashing
When a hash table is re-sized, only k/n keys need to be remapped

**hash space** - hash function f has an output from x0->xn
**hash ring** - if we connected both ends of x0 and xn as a circle

### How it works
- Map servers based on IP onto the ring
- Cache keys are hashed onto the hash ring
- To find server a key is stored on, go clockwise from key position until server found

### Adding/Removing a server
- Adding a new server only redistributes a fraction of keys
- If 1 server is added, just closet keys on ring redistributed
- Rest of keys are unchanged
- Same with removing a server
- If 1 server is added, the keys are redistributed to next server on ring
- Again, rest of keys are unchanged

### Issues
- Impossible to keep same size of partitions (hash space between servers)
- Its possible to have a non-uniform key distribution
- Virtual nodes/replicas can solve these issues

### Virtual nodes
- virtual node refers to real node
- Each server represented by multiple virtual nodes
- Each server responsible for multiple partitions (edges)
- Find keys the same as before
- As number of virtual nodes increase, distribution becomes more balanced
- But more spaces are needed to store data about virtual nodes


