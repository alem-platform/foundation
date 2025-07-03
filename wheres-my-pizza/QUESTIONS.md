## Project Setup and Compilation
### Does the program compile successfully with `go build -o restaurant-system .`?
- [ ] Yes
- [ ] No

### Does the code follow gofumpt formatting standards?
- [ ] Yes
- [ ] No

### Does the program handle runtime errors gracefully without crashing?
- [ ] Yes
- [ ] No

### Is the program free of external packages except for pgx/v5 and official AMQP client?
- [ ] Yes
- [ ] No

## Program functionality
### The Order Service accepts HTTP POST requests on /orders endpoint and validates input according to specified rules.
- [ ] Yes
- [ ] No

### The Order Service generates unique order numbers in format ORD_YYYYMMDD_NNN that reset daily.
- [ ] Yes
- [ ] No

### The Order Service calculates priority based on total amount (10 for >$100, 5 for $50-$100, 1 for others).
- [ ] Yes
- [ ] No

### The Order Service stores orders, order items, and status logs in database within a single transaction.
- [ ] Yes
- [ ] No

### The Order Service publishes order messages to RabbitMQ with correct routing keys and message properties.
- [ ] Yes
- [ ] No

### The Kitchen Worker registers itself in the workers table and handles duplicate worker name scenarios correctly.
- [ ] Yes
- [ ] No

### The Kitchen Worker consumes messages from kitchen queues and processes orders according to specialization.
- [ ] Yes
- [ ] No

### The Kitchen Worker updates order status to 'cooking', simulates cooking time, then updates to 'ready'.
- [ ] Yes
- [ ] No

### The Kitchen Worker publishes status update messages to notifications_fanout exchange.
- [ ] Yes
- [ ] No

### The Kitchen Worker handles graceful shutdown by stopping consumption and updating status to 'offline'.
- [ ] Yes
- [ ] No

### The Tracking Service provides read-only HTTP API for order status, history, and worker status.
- [ ] Yes
- [ ] No

### The Tracking Service returns proper HTTP status codes (404 for not found, 500 for server errors).
- [ ] Yes
- [ ] No

### The Notification Service subscribes to status updates and displays human-readable notifications.
- [ ] Yes
- [ ] No

### All services implement structured JSON logging with required fields (timestamp, level, service, action, message, hostname, request_id).
- [ ] Yes
- [ ] No

### All services handle RabbitMQ reconnection scenarios and implement proper error handling.
- [ ] Yes
- [ ] No

### All database operations are transactional where appropriate and handle connection failures.
- [ ] Yes
- [ ] No

### The system implements proper message acknowledgment (basic.ack, basic.nack) patterns.
- [ ] Yes
- [ ] No

### All services can be configured via YAML configuration file for database and RabbitMQ settings.
- [ ] Yes
- [ ] No

## Project presentation and code defense
### Can the team clearly explain their microservices architecture, message flow, and design choices during the presentation?
- [ ] Yes
- [ ] No

### Can the team effectively demonstrate the complete order lifecycle from creation to completion?
- [ ] Yes
- [ ] No

### Can the team explain how they handle concurrent processing, message ordering, and failure scenarios?
- [ ] Yes
- [ ] No

### Can the team show how different worker specializations and load balancing work in practice?
- [ ] Yes
- [ ] No

## Detailed Feedback

### What was great? What you liked the most about the program and the team performance?

### What could be better? How those improvements could positively impact the outcome?