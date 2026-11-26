# EC7 — Troubleshooting Log

## Part 2: Containerized Microservices

### Issue 1: PostgreSQL initially reported unhealthy
**Symptom:** Docker Compose reported PostgreSQL as unhealthy during startup.

**Investigation:** PostgreSQL logs showed an initial message that database `lab_db` did not exist, followed by successful database creation and execution of `init.sql`.

**Resolution:** Allowed initialization to complete and checked the container health status.

**Result:** PostgreSQL subsequently reported healthy.

### Issue 2: Inventory consumer had not started
**Symptom:** The inventory consumer container was in the `Created` state and its logs were empty.

**Resolution:** Started the inventory consumer after PostgreSQL became healthy.

**Result:** The consumer processed orders, and PostgreSQL subsequently contained 591 records.

### Issue 3: Kafka broker fault-tolerance test
**Test:** Stopped Kafka broker 3.

**Observation:** All three partitions retained two in-sync replicas, satisfying the configured `min.insync.replicas=2`.

**Result:** Fault-tolerance test passed. Broker 3 was subsequently restarted.

### Prevention notes
- Allow PostgreSQL initialization to finish before starting dependent services.
- Check container health before diagnosing application failures.
- Use Docker Compose service names for inter-container communication.
- Verify the current working directory before running project commands.
- Avoid deleting Docker volumes before database evidence has been collected.
