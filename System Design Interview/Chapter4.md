
# System Design Interview - Chapter 4

## Design a Rate Limiter

### What is a Rate Limiter?
- Limits number of client requests over a specified period
- Excess calls to API are blocked after some threshold
- Prevents resource starvation (DoS attack)
- Reduces costs (third party APIs $$$)
- Prevents server overload (bots, user misbehavior)

## Step 1

### Questions
- Client-side or server side?
- Throttle on IP, user ID or something else?
- Scale?
- Distributed environment?
- Separate service or in application code?
- Inform throttled users?

### Example requirements
- Accurately limit excessive requests
- Low latency, dont slow down HTTP response time
- Minimal memory used
- Distributed
- Inform throttled users
- High fault tolerance

## Step 2

### Where to put rate limiter?
- client side is unreliable, client requests are easily forged
- rate limiter could be put AT the API servers
- we can create middleware option instead that throttles requests to our API servers
- **API gateway** - middleware that supports rate limiting, usually 3rd party

### Rate limiter implemented server side or in gateway?
- Evaluate current tech stack
- Identify rate limiting algorithm that fits your needs
- API gateway for authentification may need a rate limiter

### Algorithms for rate limiting
**Token bucket**

- Bucket has tokens for requests, refilled periodically
- Requests dropped when no tokens
- parameters: bucket size and refill rate
- Simple algorithm, memory efficient
- Causes bursts of traffic, only two parameters is hard to tune

**Leaking bucket**

- Bucket with queue, requests pushed into queue
- Requests are pulled from queue and processed
- Memory efficient, stable outflow
- Recent requests could be dropped, only two parameters

**Fixed window counter algorithm**

- Divides timeline into windows, each window has counters
- Requests dropped after hitting counter threshold
- Memory efficient, easy to understand, resets quota nciely
- Spikes in traffect at edges of window can bypass quota

**Sliding window log algorithm**

- Similar to fixed window counter
- Tracks each request timestamp in a log
- New request -> remove timestamps older than the defined window size 
- Allows request if the number of current requests is below the limit, then adds its timestamp to log
- Very accurate rate limiting
- Consumes lots of memory

**Sliding window counter algorithm**

- Based on a "rolling minute" model
- Requests allowed based on position in current minute
- Smooths out spikes, memory efficient
- Assumes evenly distributed requests, only works for not-so-strict look back window

### High-level architecture
- Basic idea is simple, count requests and stop them after certain threshold
- Use in-memory cache (Redis), not database (too slow)
- Redis offers two commands:
    - INCR: increase counter by 1
    - EXPIRE: sets timeout for counter, deletes when expired

## Step 3

### Rate limiting rules
- Can be based on domain and type (marketing messages)
- Limit based on x per minute/day
- Rules generally stored in config files on disk

### Exceeding rate limit
- Rate limited requests are given HTTP response 429
- Rate limiters return HTTP headers
- X-Ratelimit-remaining, X-Ratelimit-Limit, X-Ratelimit-Retry-After

### Detailed design
- Workers pull rules from disk and store in cache
- Requests go to rate limiter first
- Rate limiter loads rules from cache, counters from Redis

### Challenges of Rate Limiters in distributed environment
**Race condition**
- Happens in concurrent environments
- Two requests concurrently read counter value before either writes it back
- Locks are simple solution but slow the system down immensely
- Solutions: Lua script, sorted sets data structure in Redis

**Synchronization**
- Multiple rate limiters may be needed for traffic
- Counters and rules need to be synch'd between them
- Best to just use centralized data store like Redis

### Performance and Monitoring
- Multi-data center setup is crucial because latency is high for far away users
- Synchronize data with an eventual consistency model
- Need to check if limiting rules and algorithm is effective
- If rules are too strict, many valid requests are dropped
- Algorithm may be poorly suited for typical use of app

## Step 4
### Overview
- Discussed some algorithms of rate limiting
- System arch, distributed env, performance and monitoring

### Additional talking points
- Hard vs soft rate limiting
- Rate limiting at different levels (7 OSI layers)
- Design client with best practices to avoid rate limiting
- (Client cache for API calls, code to catch exceptions, add back off time to retry logic)
