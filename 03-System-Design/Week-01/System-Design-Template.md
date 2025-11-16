# Design [System Name]

**Date**: [Today's Date]
**Difficulty**: Easy / Medium / Hard
**Time Spent**: [X minutes]
**Category**: Social Media / Messaging / E-commerce / Telecom / etc.

---

## 1️⃣ Problem Statement

[Brief description of what we're building]

**Example**:
> Design a URL shortening service like bit.ly that takes long URLs and converts them to short URLs.

---

## 2️⃣ Requirements Gathering (RADIO - R)

### Functional Requirements
**What should the system do?**

1. [Requirement 1] - e.g., Users can shorten long URLs
2. [Requirement 2] - e.g., When users click short URL, redirect to original
3. [Requirement 3] - e.g., URLs expire after X years
4. [Requirement 4]
5. [Requirement 5]

### Non-Functional Requirements
**How should the system perform?**

- **Scalability**: [e.g., Handle 100M URLs, 10K requests/sec]
- **Availability**: [e.g., 99.9% uptime]
- **Latency**: [e.g., < 200ms redirect]
- **Consistency**: [e.g., Eventually consistent is OK]
- **Durability**: [e.g., URLs never lost]

### Out of Scope
**What we won't build (for this interview)**

- [ ] Analytics/tracking
- [ ] User accounts
- [ ] Custom short URLs
- [ ] [Other features]

---

## 3️⃣ Capacity Estimation

### Traffic Estimates
- **Daily Active Users (DAU)**: [e.g., 100M]
- **Writes (URL creations)**: [e.g., 1000 URLs/sec]
- **Reads (URL redirects)**: [e.g., 10K redirects/sec]
- **Read:Write Ratio**: [e.g., 10:1]

### Storage Estimates
- **Average URL length**: [e.g., 500 bytes]
- **URLs per day**: [calculations]
- **Storage per year**: [calculations]
- **Storage for 5 years**: [total]

### Bandwidth Estimates
- **Incoming**: [e.g., 500 KB/sec for writes]
- **Outgoing**: [e.g., 5 MB/sec for reads]

### Memory/Cache Estimates
- **Cache 20% of daily traffic**
- **Memory needed**: [calculations using 80/20 rule]

---

## 4️⃣ API Design (RADIO - A)

### API 1: Create Short URL
```
POST /api/v1/shorten
Request:
{
  "original_url": "https://example.com/very/long/url",
  "custom_alias": "optional",
  "expiration_date": "optional"
}

Response:
{
  "short_url": "https://short.ly/abc123",
  "created_at": "2024-01-01T00:00:00Z",
  "expires_at": "2025-01-01T00:00:00Z"
}
```

### API 2: Redirect Short URL
```
GET /{short_code}
Response: 302 Redirect to original URL
```

### API 3: [Additional APIs as needed]
```
[Define other APIs]
```

---

## 5️⃣ Data Model (RADIO - D)

### Database Choice
**Decision**: [SQL / NoSQL / Both]
**Reasoning**: [Why this choice?]

### Schema Design

#### Table 1: URLs
```sql
CREATE TABLE urls (
    id              BIGINT PRIMARY KEY AUTO_INCREMENT,
    short_code      VARCHAR(10) UNIQUE NOT NULL,
    original_url    TEXT NOT NULL,
    user_id         BIGINT,
    created_at      TIMESTAMP DEFAULT NOW(),
    expires_at      TIMESTAMP,
    click_count     INT DEFAULT 0,
    INDEX idx_short_code (short_code),
    INDEX idx_user_id (user_id)
);
```

#### Table 2: [Other tables if needed]
```sql
[Additional schemas]
```

### Data Partitioning Strategy
- **Sharding Key**: [e.g., hash(short_code)]
- **Number of Shards**: [based on capacity estimates]
- **Replication**: [Master-slave, Multi-master?]

---

## 6️⃣ High-Level Design (RADIO - I)

### Architecture Diagram
```
[ASCII diagram or description]

Client
  ↓
Load Balancer
  ↓
API Servers (Stateless)
  ↓
Cache (Redis) → Database (MySQL/PostgreSQL)
  ↓
URL Shortening Service
```

### Components

#### 1. **Load Balancer**
- **Purpose**: [Distribute traffic]
- **Technology**: [NGINX, HAProxy, AWS ELB]
- **Algorithm**: [Round-robin, Least connections]

#### 2. **API Servers**
- **Purpose**: [Handle requests]
- **Technology**: [Spring Boot, Node.js]
- **Scaling**: [Horizontal scaling]

#### 3. **Cache Layer**
- **Purpose**: [Reduce DB load]
- **Technology**: [Redis, Memcached]
- **Strategy**: [LRU cache, Cache-aside]
- **What to cache**: [Popular short URLs]

#### 4. **Database**
- **Purpose**: [Persistent storage]
- **Technology**: [PostgreSQL, MySQL]
- **Replication**: [Master-slave]

#### 5. **URL Generation Service**
- **Purpose**: [Create unique short codes]
- **Algorithm**: [Base62 encoding, MD5 hash]

---

## 7️⃣ Detailed Component Design

### URL Shortening Algorithm

**Option 1: Base62 Encoding**
```java
public class URLShortener {
    private static final String BASE62 = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";

    public String encode(long id) {
        StringBuilder sb = new StringBuilder();
        while (id > 0) {
            sb.append(BASE62.charAt((int)(id % 62)));
            id /= 62;
        }
        return sb.reverse().toString();
    }

    public long decode(String shortCode) {
        long id = 0;
        for (char c : shortCode.toCharArray()) {
            id = id * 62 + BASE62.indexOf(c);
        }
        return id;
    }
}
```

**Pros**: [Deterministic, Reversible]
**Cons**: [Predictable, Sequential]

**Option 2: MD5 Hash**
[Alternative approach]

**Decision**: [Which to use and why]

---

### Handling Redirects
```java
@GetMapping("/{shortCode}")
public ResponseEntity<Void> redirect(@PathVariable String shortCode) {
    // 1. Check cache
    String url = cache.get(shortCode);

    // 2. If not in cache, query DB
    if (url == null) {
        url = urlRepository.findByShortCode(shortCode);
        cache.put(shortCode, url);
    }

    // 3. Return 302 redirect
    return ResponseEntity
        .status(HttpStatus.FOUND)
        .location(URI.create(url))
        .build();
}
```

---

### Database Sharding Strategy
[How to partition data across multiple databases]

---

## 8️⃣ Optimization & Trade-offs (RADIO - O)

### Bottlenecks Identified
1. **Database writes** - Could be slow with high write throughput
2. **Cache misses** - Cold URLs not in cache
3. **Single point of failure** - Database

### Optimizations

#### 1. **Pre-generate Short Codes**
- **Problem**: [Generating codes on-the-fly is slow]
- **Solution**: [Pre-generate millions of codes, store in pool]
- **Implementation**: [Background job generates codes]

#### 2. **CDN for Static Content**
- **Problem**: [Redirect latency]
- **Solution**: [Use CDN to cache redirects geographically]

#### 3. **Database Optimization**
- **Indexing**: [Index on short_code column]
- **Read Replicas**: [Scale reads]
- **Connection Pooling**: [Reduce connection overhead]

#### 4. **Rate Limiting**
- **Why**: [Prevent abuse]
- **How**: [Token bucket algorithm, per user/IP]

---

### Trade-offs Discussion

| Decision | Pro | Con | Chosen? |
|----------|-----|-----|---------|
| Base62 vs MD5 | Shorter codes | Predictable | Base62 ✓ |
| SQL vs NoSQL | ACID guarantees | Less scalable | SQL ✓ |
| Cache all vs 20% | Faster | More expensive | 20% ✓ |

---

## 9️⃣ Additional Considerations

### Security
- [ ] Rate limiting per user/IP
- [ ] Validate URLs before shortening
- [ ] Prevent malicious redirects
- [ ] DDoS protection

### Monitoring & Alerting
- [ ] Track API latency
- [ ] Monitor cache hit rate
- [ ] Database connection pool
- [ ] Error rates

### Failure Scenarios
- **Database down**: [Use cache, queue writes]
- **Cache down**: [Fall back to DB]
- **API server down**: [Load balancer redirects]

---

## 🎤 Interview Discussion Points

### Questions Interviewer Might Ask:
1. **Q**: "How would you handle duplicate URLs?"
   **A**: [Your answer]

2. **Q**: "What if we need to support custom short URLs?"
   **A**: [Your answer]

3. **Q**: "How would you implement analytics?"
   **A**: [Your answer]

4. **Q**: "What if short code collisions occur?"
   **A**: [Your answer]

---

## 📊 Final Architecture Diagram

```
[Detailed architecture with all components]

                    ┌─────────────┐
                    │   Clients   │
                    └──────┬──────┘
                           │
                    ┌──────▼──────┐
                    │Load Balancer│
                    └──────┬──────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
   ┌────▼────┐       ┌────▼────┐       ┌────▼────┐
   │API Srv 1│       │API Srv 2│       │API Srv 3│
   └────┬────┘       └────┬────┘       └────┬────┘
        │                  │                  │
        └──────────────────┼──────────────────┘
                           │
                    ┌──────▼──────┐
                    │    Cache    │
                    │   (Redis)   │
                    └──────┬──────┘
                           │
                    ┌──────▼──────┐
                    │  Database   │
                    │  (Sharded)  │
                    └─────────────┘
```

---

## ✅ Review Checklist
- [ ] All functional requirements addressed
- [ ] Non-functional requirements met
- [ ] Capacity estimates calculated
- [ ] APIs clearly defined
- [ ] Data model designed
- [ ] High-level architecture drawn
- [ ] Bottlenecks identified and optimized
- [ ] Trade-offs discussed
- [ ] Interview questions prepared

---

## 🧠 Key Takeaways
1. [Main learning point 1]
2. [Main learning point 2]
3. [Main learning point 3]

---

## 📚 Related Systems
- [Similar system 1] - [How it's related]
- [Similar system 2] - [How it's related]

---

## 📝 Notes for Review
[Additional notes, confusions, or insights for future review]
