# Part 2: Containerized Microservices — Implementation Log

## 1. Lab Objective
Deploy a containerized order-processing system using Docker Compose, Kafka and PostgreSQL. The system includes an order producer, notification consumer, inventory consumer, Kafka brokers and a PostgreSQL database.

## 2. Environment
- Operating system: Windows
- Terminal: Git Bash
- Python: 3.12.10
- Docker: 28.4.0
- Docker Compose: v2.39.4
- Python virtual environment: `.venv`

## 3. Unit Testing
Executed the three required test suites using `python -m pytest`.

| Component | Tests passed | Result |
|---|---:|---|
| Producer | 3/3 | PASS |
| Notification consumer | 3/3 | PASS |
| Inventory consumer | 2/2 | PASS |
| **Total** | **8/8** | **PASS** |

All eight unit tests passed.

## 4. Container Deployment
Built and deployed the application services using Docker Compose. PostgreSQL initially reported an unhealthy status while its database was being initialized. Subsequent checks confirmed that PostgreSQL was healthy and accepting connections.

The producer successfully published order messages to Kafka. The inventory consumer processed orders and stored them in PostgreSQL.

## 5. PostgreSQL Verification
- Database: `lab_db`
- Table: `orders`
- Verified record count: **591 orders**

## 6. Kafka Replication Verification
The `orders` topic was verified with:
- Partition count: 3
- Replication factor: 3
- Minimum in-sync replicas: 2

All three replicas were initially present in the in-sync replica set.

## 7. Broker Fault-Tolerance Test
Stopped Kafka broker 3 using `docker stop kafka3`.

The topic description showed that all three partitions retained two in-sync replicas. This satisfied the configured minimum of two replicas.

Broker 3 was subsequently restarted. Final replica rejoining was not separately captured.

## 8. Consumer Group Verification
Inspected the `order-inventories` consumer group. Initially, one consumer was assigned all three partitions, with zero lag reported for each partition.

Consumer scaling to two and three instances was carried out during the lab. Detailed final partition-assignment output was not retained.

## 9. Conclusion
The lab demonstrated containerized deployment, Kafka message production and consumption, PostgreSQL persistence, Kafka replication and broker fault tolerance. All eight unit tests passed, and the database contained 591 orders at the time of verification.

## 10. Cleanup
Cleanup was deferred to preserve the running containers and database records pending final documentation and evidence collection.