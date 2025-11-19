# Day 2 - Load Balancing

**Date**: November 17, 2025
**Category**: System Design
**Difficulty**: Intermediate

---

## 📚 What is Load Balancing?

**Load Balancing** is the process of distributing incoming network traffic across multiple servers to ensure no single server becomes overwhelmed. A **load balancer** acts as a traffic cop, routing client requests to the most appropriate server.

**Simple analogy**: Think of a supermarket with multiple checkout lines. The person at the entrance directs customers to different cashiers to prevent one line from getting too long - that's load balancing!

---

## 🎯 Why is it Important?

- **Real-world use case 1**: **Netflix** - Uses Elastic Load Balancing (ELB) to distribute millions of concurrent video stream requests across thousands of servers
- **Real-world use case 2**: **Google Search** - Every search query hits a load balancer that routes to one of thousands of web servers
- **Real-world use case 3**: **E-commerce sites** - Amazon/eBay use load balancers to handle Black Friday traffic spikes (10x-100x normal load)

- **Interview relevance**: Load balancing appears in **EVERY** system design interview. Questions like:
  - "Design Twitter" → How do you handle millions of tweets/sec?
  - "Design URL shortener" → How do you scale to billions of redirects?
  - **Answer always involves load balancers!**

---

## 🔍 Deep Dive

### Why Do We Need Load Balancers?

**Without Load Balancer:**
```
All traffic → Single Server → Overloaded!
- Server crashes when traffic spikes
- Single point of failure
- Can't scale horizontally
```

**With Load Balancer:**
```
Traffic → Load Balancer → Distributes to:
                          - Server 1 (33% traffic)
                          - Server 2 (33% traffic)
                          - Server 3 (33% traffic)

✓ No single server overwhelmed
✓ High availability (one fails, others continue)
✓ Can add more servers as needed
```

---

## 🏗️ Load Balancer Types

### 1. Layer 4 (L4) - Transport Layer Load Balancing

**What it does:**
- Routes traffic based on **IP address and TCP/UDP port**
- Works at the **Transport Layer** (OSI Layer 4)
- Doesn't inspect packet contents (just headers)

**How it works:**
```
Client request arrives → Load balancer checks:
- Source IP: 192.168.1.100
- Destination IP: 10.0.0.5
- Port: 443 (HTTPS)

→ Routes to Server based on IP + Port ONLY
```

**Advantages:**
- ✓ **Very fast** - No deep packet inspection
- ✓ **Low latency** - Simple routing decision
- ✓ **Protocol agnostic** - Works with any TCP/UDP traffic
- ✓ **Less resource intensive**

**Disadvantages:**
- ✗ **No content-based routing** - Can't route based on URL path
- ✗ **No SSL termination** - Must pass encrypted traffic through
- ✗ **Less intelligent** - Can't make decisions based on HTTP headers

**Use cases:**
- High-performance applications (gaming, real-time communication)
- Non-HTTP protocols (databases, SMTP, FTP)
- When you need maximum throughput

**Example:**
```
AWS Network Load Balancer (NLB)
NGINX (in stream mode)
HAProxy (TCP mode)
```

---

### 2. Layer 7 (L7) - Application Layer Load Balancing

**What it does:**
- Routes traffic based on **application-level data** (HTTP headers, cookies, URL paths)
- Works at the **Application Layer** (OSI Layer 7)
- Inspects packet contents

**How it works:**
```
Client request arrives → Load balancer inspects:
- URL: /api/users
- HTTP Method: GET
- Headers: Accept: application/json
- Cookies: session_id=abc123

→ Routes based on APPLICATION LOGIC
```

**Advantages:**
- ✓ **Intelligent routing** - Route /api/* to API servers, /static/* to CDN
- ✓ **SSL termination** - Decrypt traffic at load balancer, send plain HTTP to backends
- ✓ **Content-based decisions** - Route based on user type, geography, etc.
- ✓ **Request manipulation** - Add/remove headers, rewrite URLs

**Disadvantages:**
- ✗ **Slower than L4** - Deep packet inspection takes time
- ✗ **More CPU intensive** - SSL termination, header parsing
- ✗ **HTTP/HTTPS only** - Doesn't work with other protocols

**Use cases:**
- Web applications (most modern apps)
- Microservices architectures (route to different services)
- A/B testing, canary deployments
- SSL offloading

**Example:**
```
AWS Application Load Balancer (ALB)
NGINX (HTTP mode)
HAProxy (HTTP mode)
Envoy
```

---

## 📊 L4 vs L7 Comparison

| Feature | Layer 4 (L4) | Layer 7 (L7) |
|---------|-------------|--------------|
| **OSI Layer** | Transport (TCP/UDP) | Application (HTTP/HTTPS) |
| **Routing based on** | IP + Port | URL, headers, cookies, content |
| **Speed** | Very fast | Slower (deep inspection) |
| **SSL Termination** | No | Yes |
| **Content Awareness** | No | Yes |
| **Use case** | High throughput, non-HTTP | Web apps, microservices |
| **Example** | AWS NLB | AWS ALB |
| **Latency** | ~1ms | ~5-10ms |

---

## 🎯 Load Balancing Algorithms

### 1. Round Robin

**How it works:**
```
Request 1 → Server A
Request 2 → Server B
Request 3 → Server C
Request 4 → Server A (cycle repeats)
```

**Pros:**
- Simple, fair distribution
- Works well when all servers are equal

**Cons:**
- Doesn't account for server load
- Doesn't handle different server capacities

**When to use:**
- Servers have equal capacity
- Requests have similar processing time
- Stateless applications

---

### 2. Least Connections

**How it works:**
```
Server A: 5 active connections
Server B: 3 active connections
Server C: 7 active connections

New request → Routes to Server B (least connections)
```

**Pros:**
- Better for long-lived connections (WebSockets, DB connections)
- Accounts for server load

**Cons:**
- More complex to track
- Doesn't account for connection complexity

**When to use:**
- Long-lived connections
- Requests have varying processing times
- Unequal server loads

---

### 3. Weighted Round Robin / Weighted Least Connections

**How it works:**
```
Server A: Weight 3 (powerful server)
Server B: Weight 2
Server C: Weight 1

Distribution:
A gets 50% traffic (3/6)
B gets 33% traffic (2/6)
C gets 17% traffic (1/6)
```

**Pros:**
- Handles servers with different capacities
- Gradual rollouts (canary deployments)

**Cons:**
- Need to know server capacities
- Manual weight configuration

**When to use:**
- Heterogeneous server fleet (different CPU/RAM)
- Gradual feature rollouts
- Migrating to new infrastructure

---

### 4. IP Hash (Session Persistence / Sticky Sessions)

**How it works:**
```
Client IP: 192.168.1.100
Hash(192.168.1.100) mod 3 = 1 → Always routes to Server B

Same client always goes to same server
```

**Pros:**
- Session persistence (user always hits same server)
- No need for distributed session storage

**Cons:**
- Uneven distribution if clients are behind NAT
- Server failure breaks sessions

**When to use:**
- Stateful applications
- Shopping carts, user sessions
- When distributed cache is not available

---

### 5. Least Response Time

**How it works:**
```
Server A: Avg response time 50ms, 5 connections
Server B: Avg response time 100ms, 3 connections
Server C: Avg response time 30ms, 10 connections

New request → Routes to Server C (fastest response despite more connections)
```

**Pros:**
- Routes to fastest server
- Best user experience

**Cons:**
- Requires active monitoring
- More complex

**When to use:**
- Geographically distributed servers
- Servers with varying performance
- Low-latency requirements

---

## 💻 Code Example: Simple Round Robin Load Balancer

```java
import java.util.List;
import java.util.concurrent.atomic.AtomicInteger;

public class RoundRobinLoadBalancer {
    private final List<String> servers;
    private final AtomicInteger currentIndex;

    public RoundRobinLoadBalancer(List<String> servers) {
        this.servers = servers;
        this.currentIndex = new AtomicInteger(0);
    }

    public String getNextServer() {
        if (servers.isEmpty()) {
            throw new IllegalStateException("No servers available");
        }

        // Get current index and increment atomically (thread-safe)
        int index = currentIndex.getAndUpdate(i -> (i + 1) % servers.size());
        return servers.get(index);
    }
}

// Usage
List<String> servers = Arrays.asList(
    "server-1.example.com",
    "server-2.example.com",
    "server-3.example.com"
);

RoundRobinLoadBalancer lb = new RoundRobinLoadBalancer(servers);

// Simulate 10 requests
for (int i = 0; i < 10; i++) {
    System.out.println("Request " + i + " → " + lb.getNextServer());
}

// Output:
// Request 0 → server-1.example.com
// Request 1 → server-2.example.com
// Request 2 → server-3.example.com
// Request 3 → server-1.example.com
// Request 4 → server-2.example.com
// ...
```

**Why AtomicInteger?**
- Thread-safe without synchronization
- Multiple threads can call `getNextServer()` concurrently
- **Connection to your PMC work:** Similar to how BackupThreadPoolService manages thread pools!

---

## 💻 Code Example: Weighted Round Robin

```java
import java.util.*;

public class WeightedRoundRobinLoadBalancer {
    private final Map<String, Integer> serverWeights;
    private final List<String> expandedServerList;
    private int currentIndex = 0;

    public WeightedRoundRobinLoadBalancer(Map<String, Integer> serverWeights) {
        this.serverWeights = serverWeights;
        this.expandedServerList = new ArrayList<>();

        // Expand servers based on weights
        // Server A (weight 3) appears 3 times in list
        for (Map.Entry<String, Integer> entry : serverWeights.entrySet()) {
            for (int i = 0; i < entry.getValue(); i++) {
                expandedServerList.add(entry.getKey());
            }
        }
    }

    public synchronized String getNextServer() {
        if (expandedServerList.isEmpty()) {
            throw new IllegalStateException("No servers available");
        }

        String server = expandedServerList.get(currentIndex);
        currentIndex = (currentIndex + 1) % expandedServerList.size();
        return server;
    }
}

// Usage
Map<String, Integer> weights = new HashMap<>();
weights.put("powerful-server", 3);
weights.put("medium-server", 2);
weights.put("weak-server", 1);

WeightedRoundRobinLoadBalancer lb = new WeightedRoundRobinLoadBalancer(weights);

// Simulate 12 requests
for (int i = 0; i < 12; i++) {
    System.out.println("Request " + i + " → " + lb.getNextServer());
}

// Output (order varies but distribution is 3:2:1):
// powerful-server appears 6 times (50%)
// medium-server appears 4 times (33%)
// weak-server appears 2 times (17%)
```

---

## 🏥 Health Checks

**Problem:** What if a server crashes? Load balancer shouldn't send traffic to dead servers!

**Solution:** Health checks

### Active Health Checks

Load balancer periodically pings servers:

```java
public class HealthCheck {
    public boolean isServerHealthy(String serverUrl) {
        try {
            HttpResponse response = httpClient.get(serverUrl + "/health");
            return response.getStatusCode() == 200;
        } catch (Exception e) {
            return false;  // Server is down
        }
    }
}

// Health check loop
ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(1);
scheduler.scheduleAtFixedRate(() -> {
    for (String server : servers) {
        boolean healthy = healthCheck.isServerHealthy(server);
        if (!healthy) {
            removeServerFromPool(server);
        } else {
            addServerToPool(server);
        }
    }
}, 0, 10, TimeUnit.SECONDS);  // Check every 10 seconds
```

**Types:**
- **HTTP health check**: `GET /health` returns 200 OK
- **TCP health check**: Can establish TCP connection
- **Custom health check**: Database connectivity, disk space, etc.

---

## 🏢 Real-World Examples

### Company 1: Netflix

**Challenge**: Stream to 230M+ subscribers, handle traffic spikes during new releases

**Load Balancing Strategy:**
- **AWS Elastic Load Balancer (ELB)** at edge
- **Zuul** (Netflix's custom L7 load balancer) for microservices
- **Eureka** for service discovery (knows which servers are available)
- **Ribbon** for client-side load balancing

**Result**: Can handle millions of concurrent streams

---

### Company 2: Uber

**Challenge**: Route ride requests to nearest drivers in real-time

**Load Balancing Strategy:**
- **Geographic load balancing** - Route to data center closest to user
- **Ring Pop** (consistent hashing) for service-to-service communication
- **NGINX** as L7 load balancer for API gateway

**Result**: Sub-second response times globally

---

### Company 3: Google

**Challenge**: Handle billions of search queries per day

**Load Balancing Strategy:**
- **Maglev** (Google's custom L4 load balancer)
- **Consistent hashing** to minimize disruption when servers added/removed
- **Health checks** every few seconds
- **Anycast** for geographic distribution

**Result**: 99.99% uptime, handles massive scale

---

## 🎤 Common Interview Questions

### Question 1:
**Q**: What's the difference between L4 and L7 load balancing? When would you use each?

**A**:
**L4 (Transport Layer):**
- Routes based on IP + Port
- Very fast, low latency
- Protocol agnostic
- Use for: High throughput, non-HTTP protocols, when speed is critical

**L7 (Application Layer):**
- Routes based on URL, headers, cookies
- Can do SSL termination
- Content-aware routing
- Use for: Web apps, microservices, when you need intelligent routing

**Example:**
- L4 for database load balancing (PostgreSQL master-replica)
- L7 for routing `/api/users` to user service, `/api/orders` to order service

---

### Question 2:
**Q**: How would you implement session persistence (sticky sessions)?

**A**:
**Three approaches:**

1. **IP Hash** - Hash client IP, always route to same server
   - Pros: Simple
   - Cons: NAT issues, uneven distribution

2. **Cookie-based** - Load balancer sets cookie, routes based on it
   ```
   Client → Load Balancer sets cookie: server=A
   Next request → Load Balancer reads cookie → Routes to Server A
   ```
   - Pros: Accurate, works with NAT
   - Cons: Requires L7 load balancer

3. **Session Store** - Use Redis/Memcached for shared session storage
   ```
   Client → Any Server → Reads session from Redis
   ```
   - Pros: No sticky sessions needed, best for scale
   - Cons: Extra infrastructure

**Best practice**: Use shared session store (Redis) for scalability

---

### Question 3:
**Q**: A server crashes. How does the load balancer detect this and stop sending traffic?

**A**:
**Health Checks:**

1. **Active health checks** - Load balancer pings servers periodically
   - HTTP: `GET /health` every 10 seconds
   - TCP: Try to establish connection
   - If fails 3 consecutive times → Mark server as unhealthy

2. **Passive health checks** - Monitor actual traffic
   - If server returns 5xx errors → Reduce traffic
   - If continues → Mark unhealthy

3. **Circuit breaker pattern**
   - After X failures, stop sending traffic
   - Retry after cooldown period

**In practice:**
```
Every 10 seconds:
  For each server:
    Send GET /health
    If 200 OK → Healthy
    If timeout/5xx → Increment failure count
    If failure count >= 3 → Remove from pool

Every 60 seconds:
  Retry unhealthy servers
  If healthy → Add back to pool
```

---

### Question 4:
**Q**: Design a load balancer for a microservices architecture with 100 services.

**A**:

**Architecture:**
```
Client
  ↓
API Gateway (L7 Load Balancer)
  ↓
  ├→ User Service (3 instances) ← Internal L4 LB
  ├→ Order Service (5 instances) ← Internal L4 LB
  ├→ Payment Service (4 instances) ← Internal L4 LB
  └→ ... 97 more services
```

**Strategy:**
1. **API Gateway (L7)**:
   - Routes based on path: `/users/*` → User Service
   - SSL termination
   - Authentication/Authorization
   - Rate limiting

2. **Service-level (L4)**:
   - Round-robin across service instances
   - Health checks every 5 seconds
   - Fast failover

3. **Service Discovery**:
   - Consul/Eureka to register services
   - Load balancer queries service registry

4. **Monitoring**:
   - Track request rates, latencies
   - Auto-scale services based on load

**Technologies:**
- NGINX/Envoy for L7
- Kubernetes Service for L4
- Consul for service discovery

---

## 🔗 Related Concepts

- **Horizontal Scaling** - Load balancers enable adding more servers
- **High Availability** - Multiple servers + health checks = no single point of failure
- **Service Discovery** - How load balancers know which servers exist (Consul, Eureka)
- **Circuit Breaker** - Stop sending traffic to failing services
- **CDN (Content Delivery Network)** - Geographic load balancing for static content
- **Consistent Hashing** - Advanced technique for distributed caching (Redis, Cassandra)

---

## 📝 Hands-On Exercise

**Exercise**: Implement a load balancer with health checks

```java
import java.util.*;
import java.util.concurrent.*;

public class LoadBalancerWithHealthCheck {
    private final List<String> allServers;
    private final Set<String> healthyServers;
    private final AtomicInteger currentIndex;
    private final ScheduledExecutorService healthCheckScheduler;

    public LoadBalancerWithHealthCheck(List<String> servers) {
        this.allServers = new ArrayList<>(servers);
        this.healthyServers = ConcurrentHashMap.newKeySet();
        this.healthyServers.addAll(servers);  // Initially all healthy
        this.currentIndex = new AtomicInteger(0);
        this.healthCheckScheduler = Executors.newScheduledThreadPool(1);

        // Start health checks
        startHealthChecks();
    }

    private void startHealthChecks() {
        healthCheckScheduler.scheduleAtFixedRate(() -> {
            for (String server : allServers) {
                boolean healthy = checkHealth(server);
                if (healthy) {
                    healthyServers.add(server);
                } else {
                    healthyServers.remove(server);
                    System.out.println("Server " + server + " is DOWN");
                }
            }
        }, 0, 10, TimeUnit.SECONDS);
    }

    private boolean checkHealth(String server) {
        // Simulate health check (replace with actual HTTP call)
        // For demo: randomly mark servers as healthy/unhealthy
        return Math.random() > 0.2;  // 80% chance of being healthy
    }

    public String getNextServer() {
        List<String> healthy = new ArrayList<>(healthyServers);

        if (healthy.isEmpty()) {
            throw new IllegalStateException("No healthy servers available!");
        }

        int index = currentIndex.getAndUpdate(i -> (i + 1) % healthy.size());
        return healthy.get(index);
    }

    public void shutdown() {
        healthCheckScheduler.shutdown();
    }
}
```

---

## 📖 Resources & References

- **AWS**: https://aws.amazon.com/elasticloadbalancing/
- **NGINX Load Balancing**: https://www.nginx.com/resources/glossary/load-balancing/
- **Book**: "Designing Data-Intensive Applications" - Chapter on Partitioning
- **Video**: System Design Interview - Load Balancing

---

## ✅ Concept Mastery Checklist

- [ ] Can explain L4 vs L7 load balancing
- [ ] Understand all major algorithms (Round Robin, Least Connections, etc.)
- [ ] Know how health checks work
- [ ] Can design load balancing strategy for system design question
- [ ] Understand sticky sessions and trade-offs
- [ ] Can answer interview questions confidently

---

## 🧠 Key Takeaways (In Your Own Words)

1. **Load Balancers distribute traffic** - Prevent any single server from being overwhelmed
2. **L4 vs L7** - Fast (IP+Port) vs Intelligent (Content-aware)
3. **Algorithms matter** - Round Robin for equal servers, Least Connections for varying loads
4. **Health checks are critical** - Don't send traffic to dead servers
5. **Every system design needs load balancing** - It's the foundation of horizontal scaling

**For interviews:**
- "I'd use an L7 load balancer to route /api/* to backend services"
- "Health checks every 10 seconds ensure we don't route to failed servers"
- "Round robin works here because all servers have equal capacity"

---

## 💭 Personal Notes

**Connection to PMC work:**
- Your BackupThreadPoolService distributes backup tasks across threads
- **This is like load balancing at the thread level!**
- Realtime pool vs Scan pool = weighted load balancing (priority-based)

**For Google interview:**
- "At my current company, I designed a task distribution system similar to load balancing"
- "I used separate thread pools with different priorities - like weighted load balancing"
- "I implemented health checks via semaphores to prevent resource exhaustion"

**Next concept:** Caching (complements load balancing - reduces backend load)

---

## 📅 Review Schedule

- [ ] Review after 1 day (Nov 18)
- [ ] Review after 1 week (Nov 24)
- [ ] Review after 1 month (Dec 17)
- [ ] Use in mock system design interview (Month 3)
