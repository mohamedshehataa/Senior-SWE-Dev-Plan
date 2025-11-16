# System Design Study Plan

## Goal
Master system design for senior-level interviews at top tech companies

## Focus Areas
1. **Fundamentals** (Months 1-2): Building blocks of distributed systems
2. **Case Studies** (Months 3-6): Design real-world systems (Twitter, WhatsApp, etc.)
3. **Domain-Specific** (Months 7-8): Telecom, messaging, VOIP systems

---

## Study Schedule
- **Mon/Wed/Fri**: 45-60 minutes system design study
- **Saturday**: 2 hours deep dive into case study

---

## 📚 System Design Framework (RADIO)

Use this framework for every system design problem:

### R - Requirements
- **Functional Requirements**: What should the system do?
- **Non-Functional Requirements**: Scale, performance, availability, consistency
- **Out of Scope**: What we won't build

### A - API Design
- Define core APIs
- Request/Response formats
- Authentication/Authorization

### D - Data Model
- Database schema
- SQL vs NoSQL choice
- Relationships

### I - Infrastructure
- High-level architecture diagram
- Component interactions
- Technology choices

### O - Optimize & Deep Dive
- Bottlenecks
- Scalability improvements
- Trade-offs

---

## Month 1-2: Fundamentals (8 Topics)

| Week | Topic | Status | Notes |
|------|-------|--------|-------|
| 1 | Scalability Basics | ⬜ | Vertical vs Horizontal |
| 1 | Load Balancing | ⬜ | L4 vs L7, Algorithms |
| 2 | Caching | ⬜ | Cache strategies, Redis |
| 2 | Database Sharding | ⬜ | Horizontal partitioning |
| 3 | Replication | ⬜ | Master-slave, Multi-master |
| 3 | CAP Theorem | ⬜ | Consistency vs Availability |
| 4 | Consistent Hashing | ⬜ | Distribution algorithm |
| 4 | Rate Limiting | ⬜ | Token bucket, Leaky bucket |

---

## Month 3-4: Intermediate + Message Queues (8 Topics)

| Week | Topic | Status | Notes |
|------|-------|--------|-------|
| 5 | Message Queues Basics | ⬜ | Queue vs Topic |
| 5 | RabbitMQ Architecture | ⬜ | Exchanges, Queues, Bindings |
| 6 | Kafka Deep Dive | ⬜ | Partitions, Consumers |
| 6 | Pub/Sub Patterns | ⬜ | Event-driven design |
| 7 | Microservices Architecture | ⬜ | Service boundaries |
| 7 | Service Discovery | ⬜ | Eureka, Consul |
| 8 | API Gateway | ⬜ | Routing, Auth |
| 8 | Circuit Breaker | ⬜ | Resilience patterns |

---

## Month 5-6: Case Studies - Popular Systems (8 Topics)

| Week | System | Status | Key Concepts |
|------|--------|--------|--------------|
| 9 | Design Twitter/X | ⬜ | Timeline, Followers, Tweets |
| 10 | Design WhatsApp | ⬜ | Real-time messaging, Presence |
| 11 | Design Uber | ⬜ | Location services, Matching |
| 12 | Design Netflix | ⬜ | CDN, Streaming, Recommendations |
| 13 | Design TinyURL | ⬜ | URL shortening, Hashing |
| 14 | Design Instagram | ⬜ | Image storage, Feed |
| 15 | Design Amazon | ⬜ | E-commerce, Inventory |
| 16 | Design Google Docs | ⬜ | Collaborative editing, OT/CRDT |

---

## Month 7-8: Domain-Specific - Telecom & Advanced (8 Topics)

| Week | System | Status | Key Concepts |
|------|--------|--------|--------------|
| 17 | Design SMS Gateway | ⬜ | Message routing, SMPP |
| 18 | Design VOIP Service | ⬜ | SIP, WebRTC, Signaling |
| 19 | Design Slack | ⬜ | Channels, Real-time messaging |
| 20 | Design Zoom | ⬜ | Video streaming, WebRTC |
| 21 | Design Notification System | ⬜ | Push, Email, SMS |
| 22 | Design Rate Limiter (Advanced) | ⬜ | Distributed rate limiting |
| 23 | Design Search Engine | ⬜ | Crawling, Indexing, Ranking |
| 24 | Design Distributed Cache | ⬜ | Consistency, Eviction |

---

## System Design Template

Each case study includes:
1. **Problem Statement**
2. **Requirements Gathering** (RADIO - R)
3. **Capacity Estimation** (Users, Storage, Bandwidth)
4. **API Design** (RADIO - A)
5. **Data Model** (RADIO - D)
6. **High-Level Design** (RADIO - I)
7. **Detailed Component Design**
8. **Optimization & Trade-offs** (RADIO - O)
9. **Interview Discussion Points**

See: `Templates/System-Design-Template.md`

---

## Resources

### Books
- [ ] "Designing Data-Intensive Applications" by Martin Kleppmann
- [ ] "System Design Interview" by Alex Xu (Vol 1 & 2)

### Online Courses
- [ ] Grokking the System Design Interview
- [ ] System Design Primer (GitHub)

### Practice Platforms
- [ ] mock.io
- [ ] interviewing.io

---

## Progress Summary
- **Fundamentals Completed**: 0 / 8
- **Intermediate Topics**: 0 / 8
- **Case Studies Completed**: 0 / 8
- **Domain-Specific**: 0 / 8
- **Total**: 0 / 32
