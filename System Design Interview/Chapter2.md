
# System Design Interview - Chapter 2
---
**Back of the envolope estimation**: estimates based on thought experiments and common performance numbers to get feel of designs which meet requirements.

## Power of two
Data volume units are bytes (8 bits)

---

## Latency
Length of typical computer operations (in 2010)

### Examples
- L1 cach reference         0.5 ns
- Main memory reference     100 ns
- Send 2K bytes over 1 Gbps 20,000 ns
- Disk seek                 10 ms
- Send packet CA->NL->CA    150 ms

1 ns = 10^-9 seconds
1 us = 10^-6 seconds = 1,000 ns
1 ms = 10^-3 seconds = 1,000 us = 1,000,000 ns

### Latency Insights
- Memory is fast, disk is slow
- Avoid disk seeks if possible
- Simple compression algos are fast
- Compress data before sending over net
- Takes time to send between data centers

---

## Availability numbers
Ability of a system to be continuosly operational for a desirably long period of time

- **SLA**: Service level agreement, measurement of uptime ("in 9s")
- **99%** = 3.65 days of downtime per year
- **99.9999%** = 31.56 secs of downtime per year

---

## Ex: Estimate Twitter QPS and storage reqs
### Assumptions
- 300 million MAUs
- 50% of users active daily
- Users tweet 2 per day avg
- 10% tweets contain media
- Data store for 5 years

### Estimations:
QPS (Query per second)
**DAU** = 300M * 50% = 150M
**Tweets QPS** = 150M * 2 tweets / 24h / 3600s = ~3500
**Peak QPS** = 2 * QPS = ~7000
Storage
Average tweet size: ~1MB
**Media storage** = 150M * 2 * 10% * 1MB = 30TB per day!
**5 year media storage** = 30TB * 365 * 5 = ~55PB

---

## Tips
- Round and approximate
- Write down assumptions
- Label units
- Common estimations: QPS, peak QPS, storage, cache, number of servers

---
