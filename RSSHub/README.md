# RSSHub

## Learning Objectives

- Working with XML and RSS formats
- Concurrency and channels
- Worker Pool
- PostgreSQL
- Docker Compose

## Abstract

In this project, you will build a **CLI application — an [RSS](https://en.wikipedia.org/wiki/RSS) feed aggregator**, which:

- Provides a command-line interface (CLI)
- Fetches and parses [RSS](https://en.wikipedia.org/wiki/RSS) feeds
- Stores articles in PostgreSQL
- Aggregates RSS feeds using a worker pool in the background

This is a service that collects publications from various sources that provide RSS feeds (news sites, blogs, forums). It helps users stay informed in one place without the need to visit each website manually.

Such a tool is useful for journalists, researchers, analysts, and anyone who wants to stay updated on topics of interest without unnecessary noise. This kind of application makes information more accessible and centralized.

## Context

You are developing a CLI application that periodically fetches articles from user-added RSS feeds and stores them in `PostgreSQL`. You will also implement a mechanism responsible for background, parallel processing of RSS feeds. All services are deployed using `Docker Compose`.

## General Criteria

- Your code **MUST** be formatted according to [gofumpt](https://github.com/mvdan/gofumpt). If not, you will automatically receive a grade of `0`.
- Your program **MUST** compile successfully without errors.

- Your program **MUST NOT** exit unexpectedly (e.g., due to nil pointer dereference, index out of range, etc.). If it does, you will receive a grade of `0` during the defense.

- External packages are allowed only for working with `PostgreSQL`. Using any other external dependencies will result in a grade of `0`.

- If an error occurs during startup (e.g., invalid command-line arguments), the program **MUST**:

  - Exit with a non-zero status code
  - Display a clear and understandable error message

- The project **MUST** compile successfully using the following command from the project's root directory:

```sh
$ go build -o rsshub .
```

- The project **MUST NOT** produce any errors when run with the -race flag:

```sh
$ go run -race main.go
```

- The interval **MUST** be set dynamically via a terminal command. If not, you will automatically receive a grade of `0`.

- The number of workers **MUST** also be configurable dynamically via a terminal command. If not, you will automatically receive a grade of `0`.

## Mandatory Part

### Infrastructure

Include a `docker-compose.yml` file that runs the following services:

- RSSHub – a CLI application
- PostgreSQL – for storing articles

### RSS

The main goal of the `rsshub` program is to fetch a website's `RSS` feed and store its content in a structured format in our database. This allows us to display the data nicely in the CLI.

`RSS` stands for **"Really Simple Syndication"** — it's a way to receive fresh content from a website in a structured format. It is widely used on the internet: most content-driven websites provide an `RSS` feed.

### Structure of an RSS Feed

`RSS` is a specific `XML` structure. We will simplify the task and focus only on a few fields. Below is an example of the documents that need to be parsed:

```xml
<rss xmlns:atom="http://www.w3.org/2005/Atom" version="2.0">
<channel>
  <title>RSS Feed Example</title>
  <link>https://www.example.com</link>
  <description>This is an example RSS feed</description>
  <item>
    <title>First Article</title>
    <link>https://www.example.com/article1</link>
    <description>This is the content of the first article.</description>
    <pubDate>Mon, 06 Sep 2021 12:00:00 GMT</pubDate>
  </item>
  <item>
    <title>Second Article</title>
    <link>https://www.example.com/article2</link>
    <description>This is the content of the second article.</description>
    <pubDate>Tue, 07 Sep 2021 14:30:00 GMT</pubDate>
  </item>
</channel>
</rss>
```

_Then directly transform this kind of document into structures like this:_

```go
type RSSFeed struct {
	Channel struct {
		Title       string    `xml:"title"`
		Link        string    `xml:"link"`
		Description string    `xml:"description"`
		Item        []RSSItem `xml:"item"`
	} `xml:"channel"`
}

type RSSItem struct {
	Title       string `xml:"title"`
	Link        string `xml:"link"`
	Description string `xml:"description"`
	PubDate     string `xml:"pubDate"`
}
```

If there are additional fields in the `XML`, the parser will simply ignore them. If some fields are missing, they will retain their default (zero) values.

Accordingly, you will need to implement an `RSS` parser that performs an HTTP request using the feed URL stored in the database.

### Periodic Feed Aggregation

Feeds are essentially lists of publications. Each publication is a separate web page. The main goal of the `rsshub` program is to fetch actual publications using the URLs from RSS feeds and store them in the database. This allows us to display them nicely in the CLI.

You need to create a mechanism that regularly retrieves feed URLs from the database, compares them with new articles from the `RSS` feeds, and saves any changes to the database. Priority should be given to feeds that have not been updated for a long time or have never been updated.

This mechanism must run in the background at a specified interval. The default interval should be `3 minutes`. This interval should also be configurable using a CLI command. The command must be able to change the interval while the application is running, without stopping the background aggregation process.

**To improve performance, implement a worker pool for parallel processing of multiple articles:**

1. On each timer tick, retrieve the N most outdated or never-updated feeds from the database.
2. Distribute them across the worker pool (goroutines).
3. Each worker:

   - Downloads the feed by its URL
   - Parses new articles
   - Saves them to `Postgres`

4. The application should be able to change the ticker interval and size of workers without restarting the application.

**Default behavior:**

- Default settings must be defined in the application configuration
- Default interval: `3 minutes`
- Default number of workers: `3`

**Mechanism requirements:**

- **The ticker** must be controllable: it should be possible to stop and restart it with a new interval
- **The worker** pool must be scalable: workers can be stopped or added dynamically

**What to Avoid**

| Problem                      | How to Avoid                                                                                            |
| ---------------------------- | ------------------------------------------------------------------------------------------------------- |
| ❌ Data Race                 | Use `sync.Mutex` or `atomic` when accessing shared variables (e.g., `n`, `interval`)                    |
| ❌ Goroutine Leaks           | All spawned goroutines (ticker, workers) must be terminated using `context.Context` or a `done` channel |
| ❌ Duplicate Tickers         | When calling `SetInterval()`, always stop the old ticker before starting a new one                      |
| ❌ Closing Channel Twice     | Channels (e.g., `jobs`) must be closed by only one goroutine                                            |
| ❌ Panic on `ticker.Reset()` | Never call `Reset()` on a stopped ticker                                                                |
| ❌ Unconsumed `jobs` Channel | Workers must read from the `jobs` channel; otherwise, writing to the channel will cause a deadlock      |

**What Needs to Be Implemented**

- Structures for the ticker and worker pool
- Workers — read from the `jobs` channel and process articles
- The `jobs` channel must be created and closed correctly
- Using context cancellation to implement `graceful shutdown`
- The code should be structured as a service (in `internal/`)
- The interface is below:

```go
type YourAggregator interface {
    Start(ctx context.Context) error           // Starts background feed polling
    Stop() error                               // Graceful shutdown

    SetInterval(d time.Duration)               // Dynamically changes fetch interval
    Resize(workers int) error                  // Dynamically resizes worker pool
    ...
}
```

> ---
>
> # WARNING
>
> Do NOT DoS the servers you're fetching feeds from. Anytime you write code that makes a request to a third party server you should be sure that you are not making too many requests too quickly.
> That's why I recommend printing to the console for each request, and being ready with a quick `Ctrl+C` to stop the program if you see something going wrong.
>
> [DoS attack](https://en.wikipedia.org/wiki/Denial-of-service_attack#:~:text=attacks%20are%20distributed.-,Distributed%20DoS,of%20hosts%20infected%20with%20malware.) - read more.
>
> ---
>
> # WARNING

### Implement the following commands

#### Start background fetching

Starts the background process that periodically fetches and processes RSS feeds using a worker pool.

```sh
rsshub fetch
```

This command launches:

- The RSS fetcher loop (ticker-based)
- The worker pool for concurrent feed parsing and storage

After executing this command, the application must log a confirmation message like:

```sh
The background process for fetching feeds has started (interval = 3 minutes, workers = 3)
```

**Important:**
Only one instance of the background process can be running at a time.
If you try to start it again while it’s already active, the application must log:

```sh
Background process is already running
```

This is to prevent multiple fetchers from operating concurrently and duplicating work.

#### Add new RSS feed

Command adds a new RSS feed into PostgreSQL database.

```sh
rsshub add --name "tech-crunch" --url "https://techcrunch.com/feed/"
```

#### Set RSS Fetch Interval

Dynamically changes how often RSS feeds are fetched in the background.

```sh

rsshub set-interval 2m
```

_Example: fetch feeds every 2 minutes._

After executing this command, the application must log a confirmation message:

```sh
Interval of fetching feeds changed from 3 minutes to 2 minutes
```

- This command only works if the `rsshub fetch` command is running in another terminal
- This command updates the ticker interval without restarting the application.

#### Set Number of Workers

Dynamically resizes the background worker pool that processes RSS feeds concurrently.

```sh

rsshub set-workers 5
```

_Example: run 5 concurrent workers._

After executing this command, the application must log a confirmation message:

```sh
Number of workers changed from 3 to 5
```

- This command only works if the `rsshub fetch` command is running in another terminal
- This change takes effect immediately without restarting the application or interrupting the ongoing fetch loop.

#### List available RSS feeds

Command displays RSS feeds stored in the PostgreSQL database.

```sh
rsshub list --num 5
```

_Shows the 5 most recently added feeds. Without `--num`, shows all feeds._

Output format:

```sh
# Available RSS Feeds

1. Name: tech-crunch
   URL: https://techcrunch.com/feed/
   Added: 2025-06-10 15:34

2. Name: hacker-news
   URL: https://news.ycombinator.com/rss
   Added: 2025-06-10 15:37

3. Name: bbc-world
   URL: http://feeds.bbci.co.uk/news/world/rss.xml
   Added: 2025-06-11 09:15

4. Name: the-verge
   URL: https://www.theverge.com/rss/index.xml
   Added: 2025-06-12 13:50

5. Name: ars-technica
   URL: http://feeds.arstechnica.com/arstechnica/index
   Added: 2025-06-13 08:25
```

#### Delete RSS feed

Command removes a feed from the PostgreSQL database by name.

```sh
rsshub delete --name "tech-crunch"
```

_Example: delete the TechCrunch feed from storage._

#### Show latest articles

Command shows the latest articles from PostgreSQL by feed name.

```sh
rsshub articles --feed-name "tech-crunch" --num 5
```

_Shows 5 recent articles for the given feed. Default is 3 if `--num` is not provided._

Output format:

```sh
Feed: tech-crunch

1. [2025-06-18] Apple announces new M4 chips for MacBook Pro
   https://techcrunch.com/apple-announces-m4/

2. [2025-06-17] OpenAI launches GPT-5 with multimodal capabilities
   https://techcrunch.com/openai-launches-gpt-5/

3. [2025-06-16] Google unveils new privacy tools at I/O 2025
   https://techcrunch.com/google-privacy-io-2025/

4. [2025-06-15] TikTok introduces developer platform for integrations
   https://techcrunch.com/tiktok-developer-platform/

5. [2025-06-14] Microsoft Teams gets AI-powered meeting summarization
   https://techcrunch.com/microsoft-teams-ai-summary/
```

#### Show CLI help

Command prints usage instructions and descriptions of all available commands.

```sh
rsshub --help
```

_Example Usage:_

```sh
$ ./rsshub --help

  Usage:
    rsshub COMMAND [OPTIONS]

  Common Commands:
       add             add new RSS feed
       set-interval    set RSS fetch interval
       set-workers     set number of workers
       list            list available RSS feeds
       delete          delete RSS feed
       articles        show latest articles
       fetch           starts the background process that periodically fetches and processes RSS feeds using a worker pool
```

#### Gracefully Stopping the Aggregator

To stop the running background process:

Press `Ctrl+C` in the terminal where `rsshub` fetch is running

OR

Send a termination signal (e.g. `SIGINT`, `SIGTERM`) from another process

On shutdown, the application will:

- Cancel the background context
- Gracefully stop the ticker and all workers
- Log a confirmation message:

```sh
Graceful shutdown: aggregator stopped
```

### Example Workflow

In one terminal:

```sh
# Start the aggregator
$ ./rsshub fetch
$ The background process for fetching feeds has started (interval = 3 minutes, workers = 3)


# To stop: press Ctrl+C
$ Graceful shutdown: aggregator stopped
```

In another terminal: change settings

```sh
$ ./rsshub set-interval --duration 2m
$ Interval of fetching feeds changed from 3 minutes to 2 minutes

$ ./rsshub set-workers --count 4
$ Number of workers changed from 3 to 5
```

### Database

#### PostgreSQL

🗂 **Feeds Table** (`feeds`)
Stores metadata about each RSS feed added to the system.

| Field      | Type          | Description                     |
| ---------- | ------------- | ------------------------------- |
| id         | UUID (PK)     | Unique identifier for the feed  |
| created_at | TIMESTAMP     | When the feed was added         |
| updated_at | TIMESTAMP     | When the feed was last updated  |
| name       | TEXT (unique) | Human-readable name of the feed |
| url        | TEXT          | The RSS feed URL                |

📝 This table is used to track all RSS sources the aggregator is monitoring. The url is used to fetch data, while the name is used to reference feeds via CLI commands.

📰 **Articles Table** (`articles`)
Stores all articles parsed from the various RSS feeds.

| Field        | Type      | Description                                |
| ------------ | --------- | ------------------------------------------ |
| id           | UUID (PK) | Unique identifier for the article          |
| created_at   | TIMESTAMP | When the article was stored                |
| updated_at   | TIMESTAMP | When the article was last modified         |
| title        | TEXT      | Title of the article                       |
| link         | TEXT      | Canonical URL of the article               |
| published_at | TIMESTAMP | Original publication timestamp             |
| description  | TEXT      | Short description or summary from RSS feed |
| feed_id      | UUID      | Foreign key referencing feeds.id           |

📝 This table holds all fetched articles and links them to their corresponding RSS feed. It ensures deduplication and supports querying recent posts per feed.

> This is a standard instruction for tables. It's in your best interest to add fields.

#### Migrations

Migrations are a set of versioned files that describe changes to the database schema (DDL): creating tables, modifying columns, adding indexes, etc.

Example:

```sh
migrations/
├── create_feeds_table.up.sql
└── create_feeds_table.down.sql
```

- `*.up.sql` — The SQL that is used
- `*.down.sql` — SQL that is rolled back

`create_feeds_table.up.sql`:

```sql
CREATE TABLE feeds (
    //TODO
);
```

`create_feeds_table.down.sql`:

```sql
DROP TABLE IF EXISTS feeds;
```

### Initial Setup

#### Configuration

```env
# CLI App
CLI_APP_TIMER_INTERVAL=3m
CLI_APP_WORKERS_COUNT=3

# PostgreSQL
POSTGRES_HOST=localhost
POSTGRES_PORT=5432
POSTGRES_USER=postgres
POSTGRES_PASSWORD=changem
POSTGRES_DBNAME=rsshub
```

#### Docker Compose

Launches:

- RSSHub (CLI application)
- PostgreSQL (port: `5432`)
  - set username, password, db from `env`

## Recommendations from the Author

- Start with a single RSS feed and a CLI command to fetch it
- Implement an aggregation mechanism with a background timer
- Add a worker pool for parallel feed processing
- Use PostgreSQL for storage and configure database migrations
- Create a `docker-compose.yml` file for local development
- Finish by testing your application logic

**Here are some RSS feeds to get started:**

- TechCrunch: `https://techcrunch.com/feed/`
- Hacker News: `https://news.ycombinator.com/rss`
- UN News: `https://news.un.org/feed/subscribe/ru/news/all/rss.xml`
- BBC News: `https://feeds.bbci.co.uk/news/world/rss.xml`
- Ars Technica: `http://feeds.arstechnica.com/arstechnica/index`
- The Verge: `https://www.theverge.com/rss/index.xml`

## Optional _(Nice to Have)_

For your own growth _(Future opportunities)_:

- Implement a Web API for external access
- Add a Telegram bot to notify users about new articles

## Support

It's always hard to know where to start. Try breaking the task into smaller pieces, each solving one specific problem. Then connect and extend them — eventually, you'll have a complete solution.

Good luck, and enjoy the process :3

## Author

This project has been created by:

_@trech_

Contacts:

---

- [Email](mailto:amir.inkarov.01@gmail.com)
- [GitHub](https://github.com/Tr8ch/)
- [Discord](https://discordapp.com/users/394227156915322881/)
- [LinkedIn](https://www.linkedin.com/in/trech/)
