
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

## Step 4
