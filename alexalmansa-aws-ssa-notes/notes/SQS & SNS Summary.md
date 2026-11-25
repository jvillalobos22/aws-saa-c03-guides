### **SQS**

**Message size:** max 256 KB
**Retention:** 60 sec to 14 days (default: 4 days)
**Visibility timeout:** 0 to 12 hours (default: 30 sec)
**Delivery:**
- Standard: at-least-once, unordered
- FIFO: exactly-once, ordered by Message Group ID

**Throughput:**
- Standard: unlimited
- FIFO: 300 msg/sec (without batching)
- FIFO: 3,000 msg/sec (with batching)
- Can increase limits via support ticket


**Deduplication (FIFO only):** 5-min interval; via MessageDeduplicationId or content hash
**Encryption:** KMS supported
**DLQ (Dead-Letter Queue):**
- Captures messages that fail processing after maxReceiveCount
- Helps isolate and debug “poison messages” (e.g. malformed, unprocessable)

---

### **SNS**

**Message size:** max 256 KB
**Targets:** SQS, Lambda, HTTP/S, Email, SMS, mobile push
**Retries:**
- HTTP/S: exponential backoff for ~23 days
- Lambda: 2 retries, then DLQ (if configured)
**Fan-out:** publish once, notify multiple targets
**Encryption:** KMS supported
**DLQ support:** only for Lambda

---
## **When to Use**

| **Use Case**                          | **Service**   |
| ------------------------------------- | ------------- |
| Ordered + exactly-once                | SQS FIFO      |
| High throughput + unordered           | SQS Standard  |
| One-to-many notification              | SNS           |
| Buffer requests between systems       | SQS           |
| Event fan-out with queueing           | SNS + SQS     |
| DB or service ingest with retry logic | SNS           |
| Legacy alerting (email/SMS)           | SNS           |
| Trigger Lambda with delay             | SQS           |
| Retry + capture failed events         | SQS/SNS + DLQ |
