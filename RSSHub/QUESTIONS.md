## Project Architecture & Clean Code

### The project follows Clean Architecture principles (`domain`, `app`, `adapter`, `internal`, `cli` separation)
- [] Yes
- [ ] No

### All core logic is separated from infrastructure and frameworks
- [] Yes
- [ ] No

### Business logic is implemented in the internal layer via use cases
- [ ] Yes
- [ ] No

### Domain layer contains only pure models and interfaces
- [ ] Yes
- [ ] No

## CLI Functionality

### CLI provides commands to `add`, `list`, and `delete` RSS feeds
- [ ] Yes
- [ ] No

### CLI provides commands to `articles` show articles
- [ ] Yes
- [ ] No

### Have all the CLI command options described in the task been completed
- [ ] Yes
- [ ] No

### CLI allows changing the fetch interval at runtime (`set-interval`)
- [ ] Yes
- [ ] No

### CLI allows resizing the worker pool at runtime (`set-workers`)
- [ ] Yes
- [ ] No

### CLI supports listing articles by feed name
- [ ] Yes
- [ ] No

## Background Aggregation Mechanism

### The application fetches and stores articles from RSS feeds periodically
- [ ] Yes
- [ ] No

### Old or never-updated feeds are prioritized
- [ ] Yes
- [ ] No

### The aggregation interval can be updated during runtime without stopping the service
- [ ] Yes
- [ ] No

## Worker Pool & Concurrency

### A worker pool is implemented to fetch feeds in parallel
- [ ] Yes
- [ ] No

### The worker pool is resizable at runtime
- [ ] Yes
- [ ] No

### Shared state is protected from race conditions (`sync.Mutex` or `atomic`)
- [ ] Yes
- [ ] No

### Goroutines are properly stopped via `context.Context` or `done` channel
- [ ] Yes
- [ ] No

## Error Prevention

### Duplicate ticker creation is avoided by stopping the previous ticker
- [ ] Yes
- [ ] No

### Channels are closed only once and by one routine
- [ ] Yes
- [ ] No

### Ticker is never reset after being stopped
- [ ] Yes
- [ ] No

### `jobs` channel is always consumed to prevent deadlocks
- [ ] Yes
- [ ] No

## Storage & Caching

### PostgreSQL is used for storing feeds and articles
- [ ] Yes
- [ ] No

### Migrations are implemented
- [ ] Yes
- [ ] No

## Docker & Deployment

### A `docker-compose.yml` file is present and runs the required services
- [ ] Yes
- [ ] No

### All services (RSSHub CLI, PostgreSQL) run and interact correctly in Docker
- [ ] Yes
- [ ] No
