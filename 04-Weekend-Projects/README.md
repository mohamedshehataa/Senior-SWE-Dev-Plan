# Weekend Projects

## Goal
Build 8 substantial projects over 8 months that demonstrate senior-level engineering skills

## Project Guidelines
- **Time Commitment**: 4-5 hours on Sundays
- **Tech Stack**: Java, Spring Boot, microservices architecture
- **Focus**: Distributed systems, message queues, real-world applications
- **Deliverable**: Production-ready code with documentation

---

## 🎯 Project Principles
Each project should demonstrate:
1. **Clean Architecture** - Separation of concerns, SOLID principles
2. **Scalability** - Horizontal scaling, stateless design
3. **Observability** - Logging, metrics, monitoring
4. **Testing** - Unit, integration, and end-to-end tests
5. **Documentation** - README, API docs, architecture diagrams
6. **DevOps** - Docker, CI/CD, deployment

---

## 📋 8-Month Project Roadmap

### Project 1: REST API with Spring Boot (Weeks 2-4)
**Duration**: 3 weekends
**Tech Stack**: Spring Boot, PostgreSQL, Spring Data JPA, Spring Security
**Description**: Build a complete REST API for a task management system

**Features**:
- User authentication & authorization (JWT)
- CRUD operations for tasks
- Pagination, filtering, sorting
- Input validation
- Exception handling
- API documentation (Swagger)
- Unit & integration tests

**Learning Goals**:
- Master Spring Boot fundamentals
- RESTful API design best practices
- Database design & JPA
- Security with JWT

**GitHub**: [Link when created]

---

### Project 2: Microservices E-commerce (Weeks 6-8)
**Duration**: 4 weekends
**Tech Stack**: Spring Cloud, RabbitMQ, Docker, PostgreSQL, Redis
**Description**: Build a microservices-based e-commerce platform

**Services**:
1. **User Service** - User management
2. **Product Service** - Product catalog
3. **Order Service** - Order processing
4. **Payment Service** - Payment handling
5. **Notification Service** - Email/SMS notifications

**Features**:
- Service discovery (Eureka)
- API Gateway (Spring Cloud Gateway)
- Inter-service communication (RabbitMQ)
- Distributed tracing (Sleuth + Zipkin)
- Circuit breaker (Resilience4j)
- Centralized configuration (Config Server)
- Docker Compose for local development

**Learning Goals**:
- Microservices architecture
- Service communication patterns
- Distributed systems challenges
- Message queues

**GitHub**: [Link when created]

---

### Project 3: Distributed Cache System (Weeks 10-13)
**Duration**: 4 weekends
**Tech Stack**: Java NIO, Custom Protocol, Clustering
**Description**: Build a distributed in-memory cache similar to Redis

**Features**:
- Key-value storage
- Basic data types (String, List, Set, Hash)
- Eviction policies (LRU, LFU)
- Persistence (AOF, RDB)
- Replication (Master-slave)
- Clustering (Consistent hashing)
- Custom protocol (RESP-like)
- Java client library

**Learning Goals**:
- Low-level Java (NIO, sockets)
- Cache algorithms
- Distributed consensus
- Custom protocol design

**GitHub**: [Link when created]

---

### Project 4: Message Queue System (Weeks 14-17)
**Duration**: 4 weekends
**Tech Stack**: Java, ZeroMQ, Persistence layer
**Description**: Build a message queue system similar to RabbitMQ

**Features**:
- Queues and Topics
- Pub/Sub pattern
- Message persistence
- Acknowledgments
- Dead letter queues
- Message routing
- Management API
- Java client SDK

**Learning Goals**:
- Message queue internals
- Persistence strategies
- Producer-consumer patterns
- Queue management

**GitHub**: [Link when created]

---

### Project 5: SMS Gateway Service (Weeks 18-21)
**Duration**: 4 weekends
**Tech Stack**: Spring Boot, Kafka, Twilio API, MongoDB
**Description**: Build an SMS gateway for sending/receiving messages

**Features**:
- SMS sending via Twilio
- Webhook for incoming SMS
- Message queue (Kafka) for async processing
- Rate limiting per sender
- Message templates
- Delivery reports
- Analytics dashboard
- Bulk SMS support

**Learning Goals**:
- Telecom domain knowledge
- Third-party API integration
- High-throughput systems
- Rate limiting strategies

**GitHub**: [Link when created]

---

### Project 6: Real-time Chat Application (Weeks 22-25)
**Duration**: 4 weekends
**Tech Stack**: Spring Boot, WebSockets, Redis Pub/Sub, React
**Description**: Build a real-time chat application

**Features**:
- One-on-one chat
- Group chat
- Online presence
- Typing indicators
- Message history
- File sharing
- Push notifications
- Read receipts
- WebSocket connections

**Learning Goals**:
- WebSocket protocol
- Real-time communication
- Presence management
- Horizontal scaling with Redis

**GitHub**: [Link when created]

---

### Project 7: VOIP Service (Weeks 26-29)
**Duration**: 4 weekends
**Tech Stack**: Spring Boot, WebRTC, SIP, Kurento Media Server
**Description**: Build a simple VOIP calling service

**Features**:
- Voice calling (WebRTC)
- Signaling server (SIP)
- Call routing
- Call history
- Voicemail
- Conference calling
- Call recording
- DTMF support

**Learning Goals**:
- VOIP fundamentals
- SIP protocol
- WebRTC
- Media servers
- Telecom signaling

**GitHub**: [Link when created]

---

### Project 8: Custom Load Balancer (Weeks 30-32)
**Duration**: 3 weekends
**Tech Stack**: Java NIO, Netty
**Description**: Build a Layer 7 load balancer

**Features**:
- Multiple balancing algorithms (Round-robin, Least connections, Weighted)
- Health checks
- SSL termination
- Connection pooling
- Sticky sessions
- Rate limiting
- Circuit breaking
- Metrics & monitoring

**Learning Goals**:
- Load balancing algorithms
- Network programming
- High-performance I/O
- Infrastructure components

**GitHub**: [Link when created]

---

## 📁 Project Template Structure

Each project should follow this structure:
```
project-name/
├── README.md
├── ARCHITECTURE.md
├── .gitignore
├── docker-compose.yml
├── src/
│   ├── main/
│   └── test/
├── docs/
│   ├── api/
│   └── diagrams/
└── scripts/
```

---

## ✅ Project Completion Checklist

For each project:
- [ ] Functional requirements met
- [ ] Code follows SOLID principles
- [ ] Unit tests (>80% coverage)
- [ ] Integration tests
- [ ] API documentation
- [ ] Architecture diagram
- [ ] README with setup instructions
- [ ] Docker configuration
- [ ] Deployed (Heroku, AWS, etc.)
- [ ] Code reviewed and refactored
- [ ] Added to portfolio

---

## 🚀 Deployment Platforms
- **Heroku** - Easy for small projects
- **AWS** - Free tier (EC2, RDS, S3)
- **DigitalOcean** - VPS hosting
- **Railway** - Modern deployment
- **Docker Hub** - Container registry

---

## 📚 Project Resources

### Spring Boot
- Spring Guides: https://spring.io/guides
- Baeldung: https://www.baeldung.com

### System Design
- System Design Primer
- Martin Fowler's Blog

### Best Practices
- 12 Factor App
- Clean Code by Robert Martin
- Effective Java by Joshua Bloch

---

## 📊 Progress Tracker

| Project | Status | Completion % | GitHub | Deployed |
|---------|--------|--------------|--------|----------|
| 1. REST API | ⬜ Not Started | 0% | - | ❌ |
| 2. E-commerce Microservices | ⬜ Not Started | 0% | - | ❌ |
| 3. Distributed Cache | ⬜ Not Started | 0% | - | ❌ |
| 4. Message Queue | ⬜ Not Started | 0% | - | ❌ |
| 5. SMS Gateway | ⬜ Not Started | 0% | - | ❌ |
| 6. Real-time Chat | ⬜ Not Started | 0% | - | ❌ |
| 7. VOIP Service | ⬜ Not Started | 0% | - | ❌ |
| 8. Load Balancer | ⬜ Not Started | 0% | - | ❌ |

---

## 💡 Tips for Success
1. **Start small** - Get MVP working first, then add features
2. **Test as you go** - Don't save testing for the end
3. **Document early** - Write README while building
4. **Commit often** - Small, focused commits
5. **Deploy early** - Find deployment issues sooner
6. **Refactor** - Leave code better than you found it
7. **Portfolio** - These are interview showcases!

---

**Remember**: Quality over quantity. One well-built project is better than three half-finished ones.
