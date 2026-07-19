# Kafka Interview Questions for .NET Core Developers
## Travel & Hospitality Industry Focus
### Intermediate to Expert Level (3+ Years Experience)

**Document Created**: 2026-07-19  
**Target Audience**: .NET Core developers with 3+ years Kafka experience  
**Industry Context**: Travel and Hospitality Sector

---

## Table of Contents
- [Section 1: Intermediate Level (Questions 1-25)](#section-1-intermediate-level)
- [Section 2: Advanced Level (Questions 26-65)](#section-2-advanced-level)
- [Section 3: Expert Level (Questions 66-100)](#section-3-expert-level)

---

## Section 1: Intermediate Level

### Fundamentals & Architecture

1. **Explain the producer-consumer model in Kafka. How is it different from traditional message queues like RabbitMQ?**
   - Expected Answer: Should mention Kafka's distributed nature, partitioning, consumer groups, offset management, and how it's better for high-throughput scenarios. Mention differences in scalability, replayability, and ordering guarantees.

2. **What is a Kafka topic and how does it relate to partitions? Can you explain with a travel booking example?**
   - Expected Answer: Topic as channel (e.g., "booking-events"), partitions for parallelism (by user ID for booking orders), mentioning offset, replicas, and how booking events from different users can be processed in parallel.

3. **In your .NET Core application, how would you configure a Kafka producer to send booking confirmation events?**
   - Expected Answer: Should include BootstrapServers, Acks setting, compression, batching, error handling, and code example showing ProducerConfig setup.

4. **What is an offset and why is it critical for Kafka consumers?**
   - Expected Answer: Offset as unique position, tracks message position in partition, enables replay, prevents message loss/duplicates, automatic/manual commit strategies.

5. **Explain consumer groups and how they enable horizontal scaling. Give a hospitality booking scenario.**
   - Expected Answer: Multiple consumers reading same topic, automatic rebalancing, each partition assigned to one consumer, scaling by adding consumers for order processing.

### Partitioning & Message Ordering

6. **How would you ensure all booking messages from the same customer are processed in order?**
   - Expected Answer: Use customer ID as partition key, ensures all messages from customer go to same partition, ordered consumption within partition.

7. **What happens when you publish a Kafka message without a partition key?**
   - Expected Answer: Round-robin distribution across partitions, no ordering guarantee, messages spread randomly.

8. **In a travel booking system processing 100K bookings/hour, how many partitions would you recommend and why?**
   - Expected Answer: Recommend 10-20 partitions for scalability, consider consumer count, peak load, network bandwidth, replication factor (3 for production).

9. **What is rebalancing in consumer groups? How does it affect your application during deployments?**
   - Expected Answer: Rebalancing triggered when consumer joins/leaves, partitions reassigned, processing temporarily stops (stop-the-world), should handle gracefully in code.

10. **How do you handle message ordering across multiple partitions in .NET Core?**
    - Expected Answer: Acknowledge it's not possible globally, must use single partition if needed (sacrifices throughput), or application-level ordering logic.

### Consumer Configuration & Offset Management

11. **What's the difference between auto-commit and manual offset commit in a .NET consumer?**
    - Expected Answer: Auto-commit periodically commits, simpler but risky; manual gives control, can commit after processing success, better for exactly-once semantics.

12. **Explain at-least-once, at-most-once, and exactly-once delivery semantics. Which is most appropriate for booking confirmations?**
    - Expected Answer: At-least-once for bookings (handle duplicates), manual commit after DB write, idempotent processing with booking ID.

13. **How would you handle a consumer lag of 10 minutes in a real-time hotel booking system? What are the causes?**
    - Expected Answer: Monitor lag, identify causes (slow processing, downstream DB bottleneck), scale consumers, optimize processing logic, may need to increase partitions.

14. **What does `AutoOffsetReset = AutoOffsetReset.Earliest` do? When would you use Earliest vs Latest?**
    - Expected Answer: Earliest reads from start, Latest from end; use Earliest for new consumers catching up, Latest for new applications joining.

15. **Implement a .NET Core consumer that handles offset commit failures gracefully.**
    - Expected Answer: Should show try-catch around commit, retry logic, fallback strategy, logging, not crashing application on offset commit failure.

### Error Handling & Resilience

16. **How would you handle a failed booking event processing in Kafka? Should you use a Dead Letter Queue (DLQ)?**
    - Expected Answer: Yes, implement DLQ topic for failed messages, retry with exponential backoff, log errors, separate topic for audit trail, manual intervention process.

17. **What's the difference between transient and permanent failures in a Kafka consumer? How do you handle each?**
    - Expected Answer: Transient (network timeout - retry), permanent (invalid data - DLQ); implement circuit breaker, monitoring, alerting.

18. **In a .NET Core consumer, how do you implement retry logic for failed hotel reservation updates?**
    - Expected Answer: Exponential backoff, max retries, Circuit Breaker pattern, Polly library integration, fallback strategies, logging.

19. **What happens when your Kafka broker goes down during message processing? How does your consumer recover?**
    - Expected Answer: Consumer detects broker down (heartbeat timeout), tries to reconnect with backoff, may trigger rebalancing, should handle gracefully.

20. **How would you implement a timeout mechanism for processing messages if downstream services are slow?**
    - Expected Answer: Set processing timeout, catch TimeoutException, implement async/await properly, use CancellationToken, log timeouts for monitoring.

### Travel & Hospitality Scenarios

21. **Design a Kafka solution for processing booking confirmations (50K/hour). What topics would you create?**
    - Expected Answer: Topics: booking-events, payment-processed, confirmation-sent, user-notified; partition by customer_id; multiple consumer groups for different services.

22. **How would you implement real-time hotel availability updates across your system using Kafka?**
    - Expected Answer: Topic for room-availability-changes, broadcast to all services, consumers update local cache, quick room inventory updates.

23. **In a flight booking system with multiple airlines, how would you handle different booking formats/schemas from different airlines using Kafka?**
    - Expected Answer: Schema Registry, separate topics per airline or schema versioning, consumer handles multiple schema versions, compatibility checks.

24. **Explain how you'd build a notification system using Kafka for multi-channel alerts (email, SMS, push) in travel booking.**
    - Expected Answer: Single topic (booking-events), multiple consumer groups (email-sender, sms-sender, push-notifier), each processes independently.

25. **What monitoring metrics would you track for a production Kafka cluster in a travel booking platform?**
    - Expected Answer: Consumer lag, throughput (msgs/sec), error rates, broker health, topic replication status, end-to-end latency, memory/CPU usage.

---

## Section 2: Advanced Level

### Performance Optimization

26. **How do you optimize Kafka producer throughput in .NET Core for sending 1M booking events per second?**
    - Expected Answer: Batching (linger ms), compression (Snappy), buffer size, async production, partition key strategy, acks=1, connection pooling.

27. **Explain the trade-offs between Acks.All vs Acks.Leader vs Acks.None in your hospitality booking producer.**
    - Expected Answer: All (safest, slowest), Leader (balanced), None (fastest, risky); for bookings use All/Leader to prevent loss.

28. **How would you implement batch processing in a consumer to improve throughput by 10x?**
    - Expected Answer: Consume multiple records, batch insert to DB, MaxPollRecords tuning, reduce commit frequency, async processing with Task.WhenAll.

29. **What's the impact of compression (Snappy, LZ4, Gzip) on message throughput and latency? Which would you choose?**
    - Expected Answer: Snappy (good balance), LZ4 (fast), Gzip (best compression); for booking events choose Snappy (CPU efficient, good compression).

30. **How do you tune FetchMinBytes and FetchMaxWaitMs in a consumer for low-latency booking notifications?**
    - Expected Answer: Low FetchMinBytes (1024B) for latency, higher for throughput; FetchMaxWaitMs 100-500ms balance; explain fetch batching.

31. **Implement producer pooling for multiple .NET Core services publishing to Kafka in parallel.**
    - Expected Answer: Connection pool pattern, ObjectPool<IProducer>, lifecycle management, avoid creating new producers per request.

32. **How would you implement request/response pattern with Kafka using correlation IDs for hotel availability queries?**
    - Expected Answer: Producer sends query with correlation ID, consumer responds to reply-to topic, producer waits for response with timeout, useful for synchronous-like operations.

33. **What are the impacts of increasing partition count from 10 to 100 on a production cluster handling booking traffic?**
    - Expected Answer: Higher parallelism, more coordinator work, rebalancing time increases, storage overhead, network bandwidth increases, file handles increase.

34. **How do you handle message ordering with Kafka while maintaining high throughput in a travel system?**
    - Expected Answer: Partition by customer ID (ordering per customer), sacrifice global ordering, process similar customers' bookings sequentially, different customers in parallel.

35. **Explain the difference between Throughput and Latency optimization. Which is more important for flight bookings?**
    - Expected Answer: Latency more critical (customers expect instant confirmation), use lower batching, prioritize end-to-end latency, but still optimize throughput.

### Consumer Group Management & Rebalancing

36. **What is stop-the-world during rebalancing? How long does it take and how can you minimize it?**
    - Expected Answer: All consumers pause processing during rebalancing, typically 5-30 seconds; minimize by: optimizing onPartitionsAssigned, avoiding long-running logic, proper heartbeat config.

37. **How would you implement graceful shutdown of a .NET Core consumer to prevent message loss during rebalancing?**
    - Expected Answer: CancellationToken, allow in-flight messages to complete, commit offsets, call consumer.Close() properly, handle InterruptedException.

38. **Explain static membership in consumer groups. When would you use it in a travel booking system?**
    - Expected Answer: Static member ID prevents rebalancing when consumer restarts, useful for stateful consumers, reduces stop-the-world time, set group.instance.id.

39. **How do you debug consumer lag issues in production? What tools would you use?**
    - Expected Answer: kafka-consumer-groups CLI, Confluent Control Center, custom metrics, lag monitoring alerts, trace consumer offset vs latest offset.

40. **In a scenario with 5 consumer instances but 20 partitions, how are partitions distributed? What happens if you add a 6th consumer?**
    - Expected Answer: First distribution 4-4-4-4-4; adding 6th triggers rebalance, new distribution 4-3-3-3-3-4; explain round-robin assignment strategy.

41. **How would you implement partition-specific consumer groups for specific booking workflows?**
    - Expected Answer: Assign specific partitions using consumer.assign(), lose automatic rebalancing benefit, useful for targeted processing (priority bookings vs regular).

42. **What is consumer session timeout and heartbeat interval? How do you tune them for a stable booking service?**
    - Expected Answer: Session timeout 30s (detect failure), heartbeat 10s (prevent false positives); tune based on processing time, GC pauses, network latency.

43. **Explain the scenario where a partition is assigned to no consumer. How does this happen and what's the solution?**
    - Expected Answer: Fewer consumers than partitions (ok, partition idle), consumer crash and not restart (problem), solution: add consumer or ensure one consumer per partition.

44. **How do you handle partition assignment changes when a consumer crashes and restarts with a new host IP?**
    - Expected Answer: GroupCoordinator detects heartbeat timeout, triggers rebalancing, new assignment created, consumer rejoins with same group.instance.id if static.

45. **What happens to offsets when you delete a consumer group? How would you recover in a travel booking scenario?**
    - Expected Answer: Offsets deleted, no recovery unless backed up externally; prevent by: keeping active consumers, using group.id carefully, implement backup strategy.

### Schema & Data Format Management

46. **How would you evolve the schema of booking events without breaking existing consumers?**
    - Expected Answer: Schema Registry, backward/forward compatibility, version management, Avro or Protocol Buffers, allow adding optional fields, deprecate old fields gradually.

47. **Implement schema validation in a .NET Core consumer using Confluent Schema Registry.**
    - Expected Answer: AvroDeserializer with schema registry URL, handles schema evolution, versioning, consumer automatically adapts to new schema versions.

48. **What are the differences between Avro, JSON, and Protobuf for Kafka message serialization? Which is best for hotel booking events?**
    - Expected Answer: Avro (compact, schema enforced, best for Kafka), JSON (human readable, larger), Protobuf (fast, language-neutral); use Avro for booking events.

49. **How do you handle partial schema updates in a booking event without recreating the entire topic?**
    - Expected Answer: Schema Registry handles versioning, consumers handle both old and new schemas, use compatibility modes (backward, forward, full).

50. **What problems can occur if you don't have schema validation in your Kafka pipeline for flight booking data?**
    - Expected Answer: Data corruption, inconsistent formats, consumer crashes on unexpected fields, silent failures, data quality issues, downstream service failures.

### Multi-Topic Patterns

51. **Design a multi-topic architecture for a complete travel booking workflow (search → book → pay → confirm → notify).**
    - Expected Answer: Topics: search-events, booking-requests, payment-processed, booking-confirmed, customer-notified; fan-out consumers for each stage.

52. **How would you implement topic fan-out in .NET Core for sending booking confirmations to email, SMS, and push notification services?**
    - Expected Answer: One topic (booking-confirmed), three consumer groups (email-consumers, sms-consumers, push-consumers), independent processing, each can scale independently.

53. **Explain topic branching. When would you use it in a hotel reservation system?**
    - Expected Answer: Route messages to different topics based on content, e.g., premium-bookings, budget-bookings, different processing rules, different consumer priorities.

54. **How would you implement request/response pattern between microservices using Kafka topics?**
    - Expected Answer: Requester publishes to request-topic with reply-to, processor subscribes to request-topic, sends response to reply-to topic, requester waits with timeout.

55. **Design a Kafka pipeline for processing refund requests in a travel booking system with multiple approval stages.**
    - Expected Answer: Topics: refund-requested, refund-approved, refund-completed, multiple consumer groups for approval stage, DLQ for rejected refunds.

### Kafka Cluster & Operations

56. **What's the minimum recommended Kafka cluster setup for production travel booking system? Explain the trade-offs.**
    - Expected Answer: Minimum 3 brokers, replication factor 3, 2 ZooKeeper nodes minimum (3 recommended); trade-off between cost and reliability.

57. **How do you monitor broker health and disk usage in a production Kafka cluster? What alerting thresholds would you set?**
    - Expected Answer: Monitor disk usage (80% alert, 90% critical), broker CPU, memory, network I/O, replication lag, under-replicated partitions; use Prometheus + Grafana.

58. **What is ISR (In-Sync Replicas) and why is it important? How would you respond to ISR shrinkage?**
    - Expected Answer: ISR = replicas in sync with leader, if ISR shrinks, some replicas are lagging, indicates broker problems, investigate broker health, networking issues.

59. **Explain topic retention policies. How would you configure retention for booking audit logs vs real-time booking events?**
    - Expected Answer: Audit logs: time-based retention (1 year, cost trade-off), booking events: shorter retention (7 days), size-based retention also available.

60. **How do you perform a rolling broker restart without losing messages or causing downtime?**
    - Expected Answer: Restart one broker at a time, ensure replicas catch up before restarting next, monitor ISR and lag, PreferredLeaderElection, should be transparent to producers.

### Integration Patterns

61. **How would you integrate Kafka with a .NET Core hotel booking API? Show the architecture.**
    - Expected Answer: API receives booking, publishes to booking-events topic, consumer updates DB, another consumer sends confirmation, maintains separation of concerns.

62. **Implement a circuit breaker pattern in a .NET Core consumer handling payment service failures.**
    - Expected Answer: Use Polly library, detect repeated failures, open circuit to prevent cascading failures, retry after cooldown, fallback to DLQ, implement exponential backoff.

63. **How would you implement a Kafka-based event store for maintaining booking history?**
    - Expected Answer: Topic as event log, immutable booking events, consumer rebuilds current state, enables event sourcing, audit trail, state reconstruction.

64. **Design a caching strategy for a .NET Core consumer processing real-time flight prices from Kafka.**
    - Expected Answer: Distributed cache (Redis), update cache on each message, TTL based refresh, cache invalidation on topic messages, balance consistency vs performance.

65. **How would you implement outbox pattern using Kafka for transactional consistency in booking service?**
    - Expected Answer: Booking service writes to DB + outbox table in same transaction, consumer reads outbox, publishes to Kafka, deletes from outbox, ensures no lost events.

---

## Section 3: Expert Level

### Advanced Architecture & Design Patterns

66. **Design a complete Kafka architecture for a global travel booking platform handling 10 million bookings/day across multiple regions.**
    - Expected Answer: Multi-cluster setup, geo-replication, topic hierarchy (regional/global), consumer groups per region, centralized control center, latency optimization per region.

67. **Explain Kafka MirrorMaker. How would you use it for disaster recovery in a travel booking system?**
    - Expected Answer: Replicates topics/consumer groups between clusters, active-passive setup for DR, active-active for load distribution, handles offset sync, important for backup clusters.

68. **How would you implement exactly-once semantics in a .NET Core Kafka consumer for financial transactions (flight payments)?**
    - Expected Answer: Use idempotent key (booking ID + payment attempt), Transactional API, read_committed isolation level, atomic writes to DB, handle duplicate transactions.

69. **Design a multi-tenancy model using Kafka for a travel platform serving multiple travel agencies.**
    - Expected Answer: Topics per tenant or shared topic with tenant ID in key, separate consumer groups per tenant, quotas per tenant, isolation, dedicated resources for large tenants.

70. **Explain Kafka Streams vs Kafka Connect. When would you use each in a hotel booking platform?**
    - Expected Answer: Streams for stream processing (derive new topics, aggregations), Connect for data integration (DB to Kafka, Kafka to data warehouse); use both for complete pipeline.

71. **Implement Kafka Streams topology for real-time booking aggregation (bookings per hour per hotel).**
    - Expected Answer: Source from booking-events, apply TimeWindow(1 hour), aggregate by hotel ID, output to aggregated-bookings topic, handle late arrivals.

72. **How would you implement CQRS pattern with Kafka for a flight search and booking service?**
    - Expected Answer: Command topic (search, book, cancel), Event topic (searched, booked, cancelled), read model built from events, eventual consistency model.

73. **Design a Kafka-based workflow orchestration system for complex multi-step booking processes.**
    - Expected Answer: Workflow states as topics, state machine implemented in consumers, compensation logic for failed steps, saga pattern for distributed transactions.

74. **How would you implement end-to-end encryption for sensitive booking data (payment info, passport) in Kafka?**
    - Expected Answer: Encrypt at producer before publishing, decrypt at authorized consumers only, key management via external service, audit logging, compliance (PCI-DSS).

75. **Explain the lambda architecture using Kafka as the immutable log. How would you apply it to travel analytics?**
    - Expected Answer: Speed layer (real-time with Kafka Streams), Batch layer (periodic with Spark), Serving layer (merged results), booking analytics dashboard updated in real-time and batch.

### Performance & Scalability at Extreme Scale

76. **You need to handle 1 billion booking events per day. Design a Kafka solution that scales to this volume.**
    - Expected Answer: Thousands of partitions, hundreds of brokers, distributed consumer groups, topic tiering, compression, retention policies, separate clusters for different data types.

77. **How do you diagnose and resolve producer bottlenecks when throughput drops by 50% unexpectedly?**
    - Expected Answer: Check broker metrics (disk I/O, CPU), consumer lag, partition leadership, producer config, batch size, compression overhead, network saturation.

78. **Implement producer throttling to prevent overwhelming downstream consumers in peak booking hours.**
    - Expected Answer: Rate limiting at producer (token bucket), backpressure handling, queue depth monitoring, graceful degradation, circuit breaker to downstream.

79. **How would you handle a scenario where a single Kafka topic becomes a bottleneck with 100K messages/sec?**
    - Expected Answer: Split topic by region/type, increase partitions (with rebalancing strategy), add consumer groups for different processing, archive old data, shard at application level.

80. **Explain partition leadership re-election. How does it impact a high-throughput booking system?**
    - Expected Answer: When leader broker fails, new leader elected from ISR, causes brief unavailability, impacts produce latency, consumer offset commit delays; minimize with proper replication.

### Monitoring, Observability & Troubleshooting

81. **Design a comprehensive monitoring and alerting system for production Kafka infrastructure supporting 1M bookings/hour.**
    - Expected Answer: Prometheus metrics, Grafana dashboards, lag alerts, broker health checks, partition replication monitoring, end-to-end latency tracking, SLA monitoring.

82. **How would you implement distributed tracing across Kafka producers, consumers, and downstream services in .NET Core?**
    - Expected Answer: OpenTelemetry instrumentation, trace context propagation via message headers, correlation IDs, APM tool integration, end-to-end booking flow tracing.

83. **Explain how to identify and resolve consumer lag without metrics infrastructure (bootstrapped scenario).**
    - Expected Answer: kafka-consumer-groups CLI, check consumer offset vs log-end offset, look at consumer code (blocking calls, slow processing), check broker health.

84. **How do you debug a scenario where a consumer is consuming messages but not processing them (silent failures)?**
    - Expected Answer: Add detailed logging, implement health checks, monitor commit frequency, check exception handling, trace offset advancement, implement error counters.

85. **Design a chaos engineering test plan for Kafka infrastructure in production travel booking system.**
    - Expected Answer: Kill random brokers, introduce network latency, partition brokers, fill disks, monitor system resilience, verify data consistency, test failover procedures.

86. **How would you implement anomaly detection on Kafka consumer lag to automatically alert on issues?**
    - Expected Answer: Time-series analysis (moving average), statistical deviation detection, machine learning models, alert on lag spikes, correlate with deployments/errors.

87. **Explain how to trace a booking event from initial booking API call through all Kafka topics to final confirmation email.**
    - Expected Answer: Correlation ID propagation through topics (message headers), distributed tracing, log aggregation, trace visualization, identify bottlenecks in pipeline.

88. **How do you perform capacity planning for a Kafka cluster based on booking forecasts?**
    - Expected Answer: Analyze peak load patterns, calculate required throughput (msgs/sec), determine partition count (10x consumer count), estimate storage (retention * throughput), factor in replication.

### Advanced Security & Compliance

89. **Implement SSL/TLS encryption and SASL authentication for a .NET Core Kafka producer in production environment.**
    - Expected Answer: Configure security protocol, certificate management, SASL/PLAIN or SCRAM authentication, code example with Confluent.Kafka configuration.

90. **How would you ensure PCI-DSS compliance for a Kafka cluster handling payment information in travel bookings?**
    - Expected Answer: Encrypt data in transit (SSL/TLS) and at rest, access control (RBAC), audit logging, secure key management, data retention policies, compliance monitoring.

91. **Design a role-based access control (RBAC) system for Kafka topics in a travel organization with multiple departments.**
    - Expected Answer: Admin (topic management), Producer (publishing), Consumer (reading), Guest (limited read), implement via ACLs, Principal mapping to groups.

92. **How would you handle a data breach scenario where booking data was exposed in Kafka logs?**
    - Expected Answer: Response plan: identify exposure, encrypt logs, purge exposed data, audit trail review, notify customers, implement PII redaction in logs, encryption at rest.

93. **Explain Kafka's authentication and authorization mechanisms. Which is most secure for microservices architecture?**
    - Expected Answer: SASL/SCRAM preferred over PLAIN, client certificates with mutual TLS, service principals for microservices, topic-level ACLs, keycloak/OIDC integration for centralized auth.

### Disaster Recovery & High Availability

94. **Design a multi-region disaster recovery plan for a Kafka cluster serving critical travel booking traffic.**
    - Expected Answer: Active-passive setup with async replication, MirrorMaker for topic replication, DNS failover, consumer offset management, RPO/RTO targets, failback procedure.

95. **How would you implement zero-data-loss failover from primary Kafka cluster to backup cluster during a regional outage?**
    - Expected Answer: Dual-write pattern (temporarily), MirrorMaker offset sync, consumer offset backup, instant failover logic in producers, verify offset continuity.

96. **Explain the trade-offs between RPO (Recovery Point Objective) and RTO (Recovery Time Objective) in Kafka disaster recovery.**
    - Expected Answer: RPO = acceptable data loss (lower = more frequent sync, higher cost), RTO = acceptable downtime (lower = more infrastructure, higher cost); for bookings: RPO minutes, RTO seconds.

97. **How would you handle a scenario where primary Kafka cluster is down but consumers are still running and accumulating lag?**
    - Expected Answer: Circuit breaker in consumers, pause processing, switch to backup cluster, resume from saved offset, avoid duplicate processing, implement manual intervention procedure.

98. **Design a chaos engineering test to verify disaster recovery procedures for Kafka in travel booking system.**
    - Expected Answer: Simulate broker failures, network partitions, verify failover, monitor data consistency, test consumer group recovery, measure RTO/RPO, document lessons learned.

### Future Technologies & Evolution

99. **Discuss Kafka's future roadmap. What features (as of 2026) would benefit a travel booking platform?**
    - Expected Answer: KRaft (removing ZooKeeper), Tiered Storage (separate hot/cold data), Exactly-Once Semantics improvements, Stream processing enhancements, better security, multi-tenancy support.

100. **Design a hypothetical next-generation event streaming platform combining Kafka with emerging technologies (event mesh, serverless, edge computing) for global travel bookings.**
     - Expected Answer: Event mesh for service communication, serverless consumers (AWS Lambda), edge nodes for latency reduction, AI-powered anomaly detection, real-time ML models for price optimization, global load balancing, autonomous failover.

---

## Answer Guide & Expected Competencies

### Intermediate Level (Q1-25)
**Expected Competencies:**
- Understand Kafka fundamentals and architecture
- Configure producers and consumers correctly
- Implement basic error handling
- Grasp partitioning and ordering concepts
- Apply to real-world travel scenarios

**Evaluation Criteria:**
- Correct explanation with examples
- Understands trade-offs
- Can implement basic code
- Links to travel/hospitality context

### Advanced Level (Q26-65)
**Expected Competencies:**
- Deep performance tuning knowledge
- Consumer group management expertise
- Schema evolution and data format handling
- Multi-topic patterns
- Cluster operations and monitoring

**Evaluation Criteria:**
- Architectural thinking
- Production readiness considerations
- Performance optimization specifics
- Monitoring and observability
- Complex scenario handling

### Expert Level (Q66-100)
**Expected Competencies:**
- Design enterprise-scale Kafka solutions
- Implement advanced patterns (CQRS, Event Sourcing, Saga)
- Multi-region and disaster recovery
- Security and compliance expertise
- Anomaly detection and automated remediation
- Following industry best practices

**Evaluation Criteria:**
- Strategic thinking and vision
- Complex problem-solving
- Enterprise-scale considerations
- Emerging technologies knowledge
- Thought leadership demonstration

---

## Scoring Guide

### For Interviews
**Intermediate Level:**
- 18-25 Correct: Ready for intermediate role
- 13-17 Correct: Needs more practical experience
- Below 13: Requires training

**Advanced Level:**
- 38-65 Correct: Senior engineer ready
- 25-37 Correct: Progressing well, ready for more responsibility
- Below 25: Needs deeper engagement with production systems

**Expert Level:**
- 75-100 Correct: Expert level, ready for architecture roles
- 60-74 Correct: Advanced professional, can handle complex systems
- Below 60: Strong engineer, needs specialized training

---

## Real-World Scenario: Complete Travel Booking Pipeline

### Problem Statement
You're architecting Kafka for a travel platform processing:
- 50,000 searches/hour
- 10,000 bookings/hour
- 100+ hotel chains
- Real-time price updates
- Multi-channel notifications (email, SMS, push)

### Expected Architecture in Interview

```
┌─────────────────────────────────────────────────────┐
│         Booking API Layer                           │
│  (ASP.NET Core with Kafka Producer)                │
└─────────────────┬───────────────────────────────────┘
                  │
        ┌─────────▼──────────┐
        │ Kafka Topics:      │
        ├─────────────────────┤
        │ • search-events    │
        │ • booking-requests │
        │ • payment-events   │
        │ • confirmations    │
        │ • notifications    │
        └────────┬───────────┘
                 │
     ┌───────────┼───────────┐
     │           │           │
┌────▼────┐ ┌───▼────┐ ┌────▼────┐
│Booking  │ │Payment │ │Notify   │
│Service  │ │Service │ │Service  │
│Consumer │ │Consumer│ │Consumers│
└─────────┘ └────────┘ └─────────┘
     │           │           │
     └───────────┼───────────┘
                 │
        ┌────────▼────────┐
        │  Database       │
        │  (Booking Data) │
        └─────────────────┘
```

### Key Considerations in Answer:
1. **Partitioning Strategy**: By hotel ID, customer ID for booking events
2. **Consumer Groups**: Separate for each microservice
3. **Scaling**: Partition count = 2-3x peak consumer count
4. **Failure Handling**: DLQ for failed bookings, retry policy
5. **Monitoring**: Lag alerts, end-to-end latency tracking
6. **Security**: Encryption, authentication, audit logging
7. **Compliance**: Data retention, GDPR considerations

---

## Tips for Interview Success

### Before the Interview
- Study Kafka documentation thoroughly
- Practice designing systems on whiteboard
- Be ready with specific project examples
- Understand trade-offs between options
- Know when to use Kafka vs alternatives

### During the Interview
- Show reasoning for decisions
- Discuss trade-offs explicitly
- Ask clarifying questions
- Draw architectures clearly
- Mention monitoring and observability
- Relate to travel/hospitality context

### After the Interview
- Follow up with detailed designs
- Provide code samples
- Reference production learnings
- Demonstrate continuous learning
- Show excitement about challenges

---

## Additional Resources for Preparation

### Key Topics to Deep Dive:
1. Kafka Source Code (GitHub)
2. Confluent Documentation
3. Kafka: The Definitive Guide (Book)
4. Production Kafka configuration papers
5. Case studies from travel/hospitality companies

### Hands-On Practice:
1. Set up multi-broker Kafka cluster locally
2. Implement producers/consumers in .NET Core
3. Simulate failures and recovery
4. Build monitoring dashboard
5. Implement complete travel booking pipeline

---

*Document Version: 1.0*  
*Last Updated: 2026-07-19*  
*For: .NET Core Developers with 3+ Years Kafka Experience*  
*Industry Focus: Travel & Hospitality*
