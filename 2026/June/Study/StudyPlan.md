# System Design Study Plan — 5 Weeks (FAANG L5+)

## Weekly Structure

Aim for ~1.5–2 hours/day. Each week has a theme.

---

## Week 1: Sharpen Fundamentals + Framework

**Goal:** Rebuild your mental toolbox so you can pull concepts fluently mid-interview.

| Day | Focus | Local File | External Sources |
|-----|-------|-----------|-----------------|
| 1 | Interview framework & delivery | `14` | [Interviewing.io System Design Guide](https://interviewing.io/guides/system-design-interview) (free, 4-part guide with "Don't say / Do say" examples); [Hello Interview "In a Hurry"](https://www.hellointerview.com/learn/system-design/in-a-hurry/introduction) (free, created by Meta/Amazon hiring managers) |
| 2 | Load balancing, caching | `03`, `04` | [System Design Primer — Load Balancing](https://github.com/donnemartin/system-design-primer#load-balancer) (L4/L7, algorithms); [System Design Primer — Caching](https://github.com/donnemartin/system-design-primer#cache) (cache-aside, write-through, write-behind, refresh-ahead) |
| 3 | Sharding, indexes, consistent hashing | `05`, `06`, `12` | DDIA Ch. 6 (Partitioning) — deepest treatment of range vs hash partitioning and rebalancing; [Hello Interview — Consistent Hashing](https://www.hellointerview.com/learn/system-design/deep-dives/consistent-hashing) (hash ring, virtual nodes, hot spots, real-world usage in Cassandra/DynamoDB) |
| 4 | CAP theorem, SQL vs NoSQL | `11`, `10` | DDIA Ch. 5 & 9 (Replication + Consistency & Consensus) — goes beyond CAP to PACELC; [Hello Interview — CAP Theorem](https://www.hellointerview.com/learn/system-design/deep-dives/cap-theorem) (when to choose C vs A, with examples); [System Design Primer — SQL or NoSQL](https://github.com/donnemartin/system-design-primer#sql-or-nosql) (decision criteria + database types) |
| 5 | Queues, redundancy/replication | `08`, `09` | DDIA Ch. 11 (Stream Processing) — Kafka, event sourcing, exactly-once semantics; [LinkedIn — "The Log: What Every Software Engineer Should Know"](https://engineering.linkedin.com/distributed-systems/log-what-every-software-engineer-should-know-about-real-time-datas-unifying) |
| 6 | Proxies, long-polling/WebSockets/SSE | `07`, `13` | [System Design Primer — Communication](https://github.com/donnemartin/system-design-primer#communication) (HTTP, TCP, UDP, RPC, REST, WebSockets) |
| 7 | **Practice:** Design a URL shortener end-to-end in 35 min (then compare with `15` and Alex Xu Vol. 1 chapter) | |

**L5+ depth check for each concept:**
- Can you name *when* you'd pick one option over another? (not just what it is)
- Can you give back-of-envelope numbers? (QPS, storage, bandwidth)
- Can you explain failure modes and mitigations?

**Book to read alongside Week 1:** *Designing Data-Intensive Applications* (Martin Kleppmann) — Chapters 1–9. The undisputed top resource for understanding the "why" behind design choices.

---

## Week 2: Real Systems Deep Dive

**Goal:** Understand the internals of production systems you'll reference in every design. Knowing *how* Redis actually works lets you say "we'd use a sorted set here for O(log N) ranked retrieval" instead of just "we'd add a cache."

**Why now:** You have the fundamentals from Week 1. Before jumping into full system designs, grounding yourself in real implementations gives you a concrete vocabulary. When you design Twitter's feed, you'll think in terms of Kafka topics and Redis sorted sets — not abstract boxes.

### Day 1: Redis

**What to understand:** In-memory data structures (strings, hashes, sorted sets, HyperLogLog, streams), persistence (RDB snapshots vs AOF), replication (async replica), Redis Cluster (hash slots, resharding), pub/sub, eviction policies (LRU, LFU, volatile-ttl).

**When you'd reference it in interviews:** Caching layer, session store, rate limiting (INCR + EXPIRE), leaderboards (ZRANGEBYSCORE), pub/sub for real-time notifications, distributed locks (Redlock).

| Resource | Why |
|----------|-----|
| [Redis University — "Get started with Redis"](https://redis.io/dev/) | Free official courses + tutorials (Dev Hub). 3-hour intro learning path |
| [Redis Internals Documentation](https://redis.io/docs/latest/operate/oss_and_stack/reference/internals/) | How hash tables resize, how sorted sets use skip lists + zip lists |
| [ByteByteGo — "Redis Explained"](https://bytebytego.com/) | Visual architecture overview |
| [Antirez (Salvatore Sanfilippo) blog](http://antirez.com/) | Creator's design decisions: why skip lists over balanced trees, Redlock controversy |
| DDIA Ch. 5 — Replication section on leader-based replication | Applies directly to Redis replica model |

### Day 2: Apache Kafka

**What to understand:** Append-only log abstraction, topics/partitions/consumer groups, offset management, exactly-once semantics (idempotent producers + transactions), ISR (in-sync replicas), compacted topics, Kafka Connect, Kafka Streams.

**When you'd reference it in interviews:** Event-driven architectures, decoupling producers/consumers, activity feeds, change data capture (CDC), event sourcing, real-time stream processing.

| Resource | Why |
|----------|-----|
| [Kafka: The Definitive Guide (Confluent — free O'Reilly book)](https://www.confluent.io/resources/kafka-the-definitive-guide-v2/) | Comprehensive, free, covers internals + operational concerns |
| [LinkedIn — "The Log"](https://engineering.linkedin.com/distributed-systems/log-what-every-software-engineer-should-know-about-real-time-datas-unifying) | Jay Kreps (Kafka creator) explains the log abstraction that motivated Kafka |
| DDIA Ch. 11 — Stream Processing | Academic treatment of exactly-once, event time vs processing time |
| [Confluent blog — "How Kafka Works"](https://developer.confluent.io/courses/architecture/get-started/) | Free course on partition leader election, ISR, replication |
| [ByteByteGo — "Kafka Explained"](https://bytebytego.com/) | Visual overview of producers, brokers, consumers, ZooKeeper/KRaft |

### Day 3: Elasticsearch

**What to understand:** Inverted index (terms → posting lists), analyzers/tokenizers, sharding (primary + replica shards), near-real-time search (refresh interval), relevance scoring (BM25), aggregations, cluster coordination (primary node election).

**When you'd reference it in interviews:** Full-text search, typeahead/autocomplete, log analytics, Twitter search, e-commerce product search, geospatial queries.

| Resource | Why |
|----------|-----|
| [Elasticsearch: The Definitive Guide (free on elastic.co)](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html) | Official docs, excellent "Getting Started" and "Inside a Shard" sections |
| [Elasticsearch from the Bottom Up](https://www.elastic.co/blog/found-elasticsearch-from-the-bottom-up) | How Lucene segments work, merge policies, why near-real-time not real-time |
| DDIA Ch. 3 — Storage Engines (LSM trees, B-trees, and inverted indexes) | Theoretical foundation for how ES stores data |
| [ByteByteGo — "How Search Works"](https://bytebytego.com/) | Visual explanation of inverted index + distributed search |
| [Uber Engineering — "How Uber Uses Elasticsearch"](https://www.uber.com/blog/engineering/) | Real-world scaling challenges: multi-tenant indexing, query routing |

### Day 4: Cassandra / DynamoDB (Wide-Column / Key-Value at Scale)

**What to understand:** LSM-tree storage engine, SSTables + memtable, partition key → consistent hashing → ring, clustering key for sort order within partition, tunable consistency (quorum reads/writes), compaction strategies (size-tiered vs leveled), anti-entropy (Merkle trees, read repair, hinted handoff).

**When you'd reference it in interviews:** High-write-throughput systems (IoT, time-series, activity logs), systems needing AP over CP, wide-column access patterns (chat messages by conversation, feed items by user).

| Resource | Why |
|----------|-----|
| [DynamoDB Paper — "Dynamo: Amazon's Highly Available Key-Value Store"](https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf) | Foundational paper: consistent hashing, vector clocks, sloppy quorum, hinted handoff |
| [Apache Cassandra Architecture Documentation](https://cassandra.apache.org/doc/latest/cassandra/architecture/) | Ring topology, gossip protocol, tunable consistency |
| DDIA Ch. 3 (LSM-trees, SSTables) + Ch. 5 (Leaderless replication) | The theory behind Cassandra's storage + replication model |
| [ByteByteGo — "Cassandra Explained"](https://bytebytego.com/) | Visual architecture |
| [AWS DynamoDB Deep Dive (re:Invent talks)](https://www.youtube.com/results?search_query=dynamodb+deep+dive+reinvent) | How DynamoDB evolved beyond the original Dynamo paper (adaptive capacity, DAX, global tables) |

### Day 5: ZooKeeper / Consensus + Service Discovery

**What to understand:** ZAB protocol (ZooKeeper Atomic Broadcast), znodes (ephemeral + sequential), watches, leader election pattern, distributed locks, configuration management. Also: how modern systems replace ZooKeeper (Kafka KRaft, etcd/Raft for Kubernetes).

**When you'd reference it in interviews:** Leader election, distributed locking, service discovery, configuration management, barrier synchronization.

| Resource | Why |
|----------|-----|
| [ZooKeeper Paper — "ZooKeeper: Wait-free coordination for Internet-scale systems"](https://www.usenix.org/legacy/event/atc10/tech/full_papers/Hunt.pdf) | Short, readable paper explaining the coordination primitive |
| DDIA Ch. 9 — Consensus section (Raft, Paxos, ZAB) | Theoretical underpinning of why consensus is hard and what ZK solves |
| [The Raft Consensus Algorithm (thesecretlivesofdata.com)](https://thesecretlivesofdata.com/raft/) | Interactive visualization — best way to internalize leader election + log replication |
| [Confluent — "KRaft: Kafka Without ZooKeeper"](https://developer.confluent.io/learn/kraft/) | Why Kafka moved away from ZK and what that means architecturally |
| [etcd Documentation](https://etcd.io/docs/) | Modern alternative (Raft-based) used by Kubernetes for service discovery |

### Day 6: CDN + Object Storage (S3 / Blob Stores) + Load Balancers (Nginx/Envoy)

**What to understand:**
- **CDN:** Edge caching, cache invalidation (TTL, purge), origin shielding, DNS-based routing (anycast), TLS termination at edge.
- **S3/Blob Storage:** Object storage vs block storage vs file storage, eventual consistency model (now strong in S3), multipart upload, lifecycle policies, cross-region replication.
- **Nginx/Envoy:** Reverse proxy, L4 vs L7 load balancing, connection pooling, circuit breaking, service mesh (Envoy sidecar pattern).

**When you'd reference it in interviews:** Any system serving static content (images, video, files), upload pipelines, API gateways, traffic management.

| Resource | Why |
|----------|-----|
| [Cloudflare Learning Center — "What is a CDN?"](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/) | Best free explanation of CDN architecture and cache hierarchies |
| [AWS re:Invent — "S3 Deep Dive"](https://www.youtube.com/results?search_query=s3+deep+dive+reinvent) | How S3 achieves 11 nines of durability, request routing, partitioning |
| [Nginx Architecture (aosabook.org)](https://aosabook.org/en/v2/nginx.html) | Event-driven architecture, worker processes, why it handles 10k+ connections |
| [Envoy Proxy Docs — Architecture Overview](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/arch_overview) | L4/L7 filters, service discovery integration, observability |
| [ByteByteGo — "CDN Explained"](https://bytebytego.com/) | Visual push vs pull CDN, cache invalidation strategies |

### Day 7: Synthesis + Cheat Sheet

Spend this day creating your own one-page reference that maps:

```
Problem Pattern → Real System → Key Mechanism → Interview Phrasing

Example:
"High-throughput ordered event stream"  → Kafka    → Append-only log + partitions    → "I'd use a partitioned log (like Kafka) because we need ordering within a user's events but can parallelize across users"
"Sub-millisecond key-value lookup"      → Redis    → In-memory hash table + skip list → "A Redis sorted set gives us O(log N) ranked retrieval with TTL-based eviction for memory management"
"Full-text search across millions"      → ES       → Inverted index + BM25 scoring    → "An inverted index (like Elasticsearch) with sharding by document ID gives us horizontal read scaling"
"High write throughput, eventual read"  → Cassandra → LSM-tree + tunable quorum       → "A wide-column store with partition key = user_id gives us single-digit-ms writes and range queries within a partition"
"Leader election / distributed lock"    → ZooKeeper → ZAB consensus + ephemeral nodes → "We'd use a consensus service for leader election — ephemeral nodes detect failures via session timeout"
"Static asset delivery at global scale" → CDN + S3  → Edge caching + origin shielding → "Static assets go to object storage behind a CDN — edge nodes serve cache hits, origin shield collapses cache misses"
```

---

## Week 3: Core Designs (Breadth)

**Goal:** Cover the highest-frequency interview questions. Time-box each to 40 min of self-design *before* reading the reference.

| Day | System | Local File | External Sources |
|-----|--------|-----------|-----------------|
| 1 | URL Shortener | `15` | Alex Xu Vol. 1 (URL shortener chapter); [Hello Interview — Bitly](https://www.hellointerview.com/learn/system-design/problem-breakdowns/bitly) (free); [System Design Primer — Pastebin](https://github.com/donnemartin/system-design-primer#design-pastebincom-or-bitly) |
| 2 | Twitter / News Feed | `20`, `26` | Alex Xu Vol. 1 (News Feed chapter); [Hello Interview — FB News Feed](https://www.hellointerview.com/learn/system-design/problem-breakdowns/fb-news-feed) (free); [System Design Primer — Twitter timeline](https://github.com/donnemartin/system-design-primer#design-the-twitter-timeline-and-search-or-facebook-feed-and-search) |
| 3 | Instagram / Photo Sharing | `17` | Grokking Modern SDI (updated Instagram design); Alex Xu Vol. 1; [Meta Engineering Blog](https://engineering.fb.com/) — search for photo infrastructure posts |
| 4 | Messenger / Chat | `19` | [Hello Interview — WhatsApp](https://www.hellointerview.com/learn/system-design/problem-breakdowns/whatsapp) (free); Meta Engineering Blog — "Facebook Messenger Optimisations" (real production insights on NYE scaling); DDIA Ch. 11 for message ordering guarantees |
| 5 | Dropbox / File Storage | `18` | [Hello Interview — Dropbox](https://www.hellointerview.com/learn/system-design/problem-breakdowns/dropbox) (free); Alex Xu Vol. 2 (Google Drive chapter) |
| 6 | YouTube / Netflix | `21` | [Netflix Tech Blog](https://netflixtechblog.com/) — "Video Encoding at Scale," "Shot-based Encoding"; [Hello Interview — YouTube](https://www.hellointerview.com/learn/system-design/problem-breakdowns/youtube) (free); Alex Xu Vol. 1 |
| 7 | **Mock interview:** Pick one design, talk through it aloud in 35 min | Use [Pramp](https://www.pramp.com/) (free peer mock) or record yourself |

**After each design, write down:**
1. The 2–3 key trade-offs you'd discuss
2. One deep-dive you could sustain for 10 min if asked
3. The bottleneck and how you'd scale past it

**How to use Week 2 knowledge here:** When you reach the "deep dive" portion of each design, consciously map to the real systems:
- URL Shortener → Redis for caching hot URLs, Cassandra/DynamoDB for the mapping store
- Twitter Feed → Kafka for fanout events, Redis sorted sets for timeline cache
- Messenger → Kafka for message queuing, Cassandra for message storage (partition by conversation_id)
- YouTube → S3/blob store for video, CDN for delivery, Kafka for processing pipeline events

---

## Week 4: Advanced Designs + Deep Dives

**Goal:** Cover less common but high-signal questions, and build deep-dive muscles.

| Day | System | Local File | External Sources | Deep-dive angle |
|-----|--------|-----------|-----------------|-----------------|
| 1 | Rate Limiter | `23` | Alex Xu Vol. 1 (progresses through algorithms to distributed environments); Cloudflare blog on rate limiting; [Uber's Go rate limiter](https://github.com/uber-go/ratelimit); Martin Fowler — Circuit Breaker pattern | Sliding window vs token bucket, distributed rate limiting with Redis INCR |
| 2 | Typeahead / Autocomplete | `22` | Alex Xu Vol. 1 (search autocomplete); Grokking SDI typeahead chapter | Trie vs Elasticsearch prefix queries, ranking freshness vs popularity |
| 3 | Web Crawler | `25` | Alex Xu Vol. 1 (web crawler); [Hello Interview — Web Crawler](https://www.hellointerview.com/learn/system-design/problem-breakdowns/web-crawler) (free); [System Design Primer — Web Crawler](https://github.com/donnemartin/system-design-primer#design-a-web-crawler) | Politeness, dedup (URL + content), Kafka as URL frontier |
| 4 | Twitter Search | `24` | [System Design Primer — Twitter Search](https://github.com/donnemartin/system-design-primer#design-the-twitter-timeline-and-search-or-facebook-feed-and-search); Elasticsearch/Lucene architecture docs | Inverted index (ES), real-time indexing, sharding strategy |
| 5 | Uber Backend | `28` | [Uber Engineering Blog](https://www.uber.com/blog/engineering/) — delivery search platform, real-time traffic forecasting; [Hello Interview — Uber](https://www.hellointerview.com/learn/system-design/problem-breakdowns/uber) (free); Google S2 library docs for geospatial | Geospatial indexing (quad-tree, geohash, S2 cells), dispatch, ETA |
| 6 | Yelp / Nearby | `27` | Google S2 Library documentation (geometry, cells, Hilbert curves); [Hello Interview — Yelp](https://www.hellointerview.com/learn/system-design/problem-breakdowns/yelp) | Quad-tree, geohash, proximity search at scale |
| 7 | **Mock interview:** Unseen prompt, 45 min | [Interviewing.io](https://interviewing.io/) (paid, with actual FAANG interviewers) or [Pramp](https://www.pramp.com/) (free) |

**L5+ differentiators to practice:**
- Capacity estimation (do it in <3 min at the start — only when it influences a design decision)
- API design (define endpoints, not just say "REST API")
- Data model (schema + access patterns → justify DB choice)
- Failure scenarios ("what happens when X goes down?")

---

## Week 5: Polish + Simulate

**Goal:** Build consistency and confidence under time pressure.

| Day | Activity | Resources |
|-----|----------|-----------|
| 1 | Re-do your weakest design from Week 3 with stricter time (35 min) | Compare against engineering blog for that system |
| 2 | Google Docs / Collaborative editing (unseen) | OT vs CRDT — search "Operational Transformation" and "CRDTs for real-time collaboration"; [Hello Interview — Google Docs](https://www.hellointerview.com/learn/system-design/problem-breakdowns/google-docs) |
| 3 | Payment system (unseen — focus on idempotency, exactly-once) | [Airbnb Engineering — "Avoiding Double Payments in a Distributed Payments System"](https://medium.com/airbnb-engineering); Alex Xu Vol. 2 |
| 4 | Full mock interview #1 — 45 min | [Pramp](https://www.pramp.com/) or [Exponent](https://www.tryexponent.com/) (peer matching by level) |
| 5 | Review mock feedback, drill weak spots | Re-read relevant DDIA chapters for depth |
| 6 | Full mock interview #2 — different topic | [Interviewing.io](https://interviewing.io/) for FAANG-quality feedback |
| 7 | Light review of framework + rest | Skim [ByteByteGo newsletter](https://bytebytego.com/) recent posts |

---

## The FAANG L5+ Interview Template (35–45 min)

Use this skeleton every time you practice:

```
[0–3 min]  Clarify requirements: functional + non-functional (latency, consistency, scale)
[3–7 min]  Back-of-envelope: DAU, QPS (peak 3x avg), storage, bandwidth
[7–12 min] API design: key endpoints, params, response shape
[12–25 min] High-level design: components, data flow, DB schema
[25–35 min] Deep dive: scaling bottleneck, trade-offs, failure handling
[35–40 min] Wrap-up: monitoring, analytics, future extensions
```

---

## Key Numbers to Memorize

Sources: [Interactive Latency Numbers](https://colin-scott.github.io/personal_website/research/interactive_latency.html), Jeff Dean's "Numbers Every Engineer Should Know", Alex Xu Vol. 1 Ch. 2

| Metric | Value |
|--------|-------|
| 1 char | 1 byte |
| UUID | 128 bits = 16 bytes |
| 1 image (compressed) | ~200–300 KB |
| 1 min video (720p) | ~5 MB |
| 1 day | 86,400 sec ≈ 10^5 sec |
| 1 month | ~2.5 × 10^6 sec |
| 1 year | ~3 × 10^7 sec |
| 1 million requests/day | ~12 QPS |
| 1 billion requests/day | ~12,000 QPS |
| L1 cache reference | ~1 ns |
| Main memory reference | ~100 ns |
| SSD random read | ~100 μs |
| SSD sequential read (1 MB) | ~1 ms |
| Disk seek | ~10 ms |
| Disk sequential read (1 MB) | ~20 ms |
| Memory read (1 MB) | ~0.25 ms |
| Network send (1 MB, 1 Gbps) | ~10 ms |
| Network round trip (same DC) | ~0.5 ms |
| Network round trip (cross-continent) | ~150 ms |

---

## Top Resources (Ranked)

### Books
1. **Designing Data-Intensive Applications** (Martin Kleppmann) — Deep understanding of distributed systems theory. The "why" behind every trade-off.
2. **System Design Interview Vol. 1 & 2** (Alex Xu) — 4.6/5 on Amazon, 4000+ reviews. Pragmatic, diagram-heavy, consistent framework applied to 15+ designs.
3. **Building Microservices** (Sam Newman) — Service decomposition, migration strategies.

### Courses / Platforms
1. **[Hello Interview](https://www.hellointerview.com/)** — Free core content, 29 system designs with level-specific expectations (mid/senior/staff+). Modern advice.
2. **[ByteByteGo](https://bytebytego.com/)** — By Alex Xu. Visual, text-based. #1 tech newsletter on Substack (1M+ subscribers).
3. **[Grokking the System Design Interview](https://www.designgurus.io/course/grokking-the-system-design-interview)** — 83 lessons, 176k+ learners, 4.7/5. Structured and repeatable.
4. **[Grokking Modern System Design](https://www.educative.io/)** — 204 lessons, includes AI-era systems (ChatGPT design). RESHADED framework.

### YouTube
1. **ByteByteGo** (1M+ subs) — Professional visual explanations
2. **Gaurav Sen** — Accessible fundamentals (consistent hashing, sharding)
3. **Jordan Has No Life** — Deeper dives, recommended on Reddit/Blind for L5+
4. **MIT 6.824** (2020 playlist) — Graduate-level distributed systems (consensus, replication, fault tolerance)

### GitHub Repos
1. [donnemartin/system-design-primer](https://github.com/donnemartin/system-design-primer) (351k stars) — Comprehensive reference + Anki flashcards
2. [karanpratapsingh/system-design](https://github.com/karanpratapsingh/system-design) (43.8k stars) — Visual diagrams
3. [InterviewReady/system-design-resources](https://github.com/InterviewReady/system-design-resources) (18.2k stars) — Curated engineering blog links by topic

### Engineering Blogs (read 2–3 posts/week)
1. **[Meta Engineering](https://engineering.fb.com/)** — Data ingestion at hyperscale, recommendation systems, search
2. **[Uber Engineering](https://www.uber.com/blog/engineering/)** — Real-time systems, geospatial, rate limiting, matching
3. **[Netflix Tech Blog](https://netflixtechblog.com/)** — Video encoding, resilience patterns, multi-region architecture
4. **[High Scalability](https://highscalability.com/)** — Aggregated architecture case studies from multiple companies
5. **[AWS Architecture Blog](https://aws.amazon.com/blogs/architecture/)** — Multi-tenancy, event-driven patterns, disaster recovery

### Mock Interview Platforms
1. **[Pramp](https://www.pramp.com/)** — Free peer-to-peer mocks, matched by experience
2. **[Interviewing.io](https://interviewing.io/)** — Anonymous mocks with actual FAANG engineers (paid)
3. **[Exponent](https://www.tryexponent.com/)** — Peer matching by level + 1:1 coaching with senior FAANG engineers

---

## Extras: Additional Systems Deep Dives

These are bonus topics to study if you have extra time, or to slot in on lighter days. They come up in specific interview scenarios and strengthen your ability to speak precisely about production systems.

---

### PostgreSQL Internals

**What to understand:** B-tree indexes (structure, page splits, fillfactor), MVCC (tuple versioning, xmin/xmax, vacuum), WAL (write-ahead log for crash recovery + replication), query planner (seq scan vs index scan vs bitmap scan, EXPLAIN ANALYZE), connection pooling (PgBouncer), partitioning (range, list, hash), and replication (streaming replication, logical replication, read replicas).

**When you'd reference it in interviews:** Any OLTP system where you pick a relational DB — you need to justify *why* SQL and explain how it scales (read replicas, partitioning, connection pooling). Also when discussing transactions, ACID guarantees, or "what happens under concurrent writes."

**L5+ edge:** Being able to say "PostgreSQL's MVCC means readers never block writers, so our read-heavy workload won't contend with writes — but we'll need to tune autovacuum for tables with high update rates" is the kind of depth that distinguishes L5+ from generic "we'll use PostgreSQL."

| Resource | Why |
|----------|-----|
| [PostgreSQL Internals (postgrespro.com — free book)](https://postgrespro.com/community/books/internals) | Full-length, free book covering buffer cache, WAL, MVCC, indexes, query execution. The most thorough single resource. |
| [The Internals of PostgreSQL](https://www.interdb.jp/pg/) | Free online book — excellent chapters on MVCC, concurrency control, buffer manager, and WAL |
| [Use The Index, Luke (use-the-index-luke.com)](https://use-the-index-luke.com/) | Best resource for understanding SQL indexing across databases — B-tree traversal, covering indexes, partial indexes |
| [PgBouncer Documentation](https://www.pgbouncer.org/) | Connection pooling modes (session, transaction, statement) and why you need it at scale |
| [Citus Blog — "Scaling PostgreSQL"](https://www.citusdata.com/blog/) | Distributed PostgreSQL — sharding strategies, when to shard vs replicate |
| DDIA Ch. 3 (B-trees, storage engines) + Ch. 7 (Transactions) | Theoretical underpinning for PostgreSQL's storage engine and isolation levels |

---

### RabbitMQ / SQS (Traditional Message Brokers)

**What to understand:** Exchange types (direct, fanout, topic, headers), queues + bindings, consumer acknowledgments, dead-letter queues (DLQ), prefetch/QoS, message TTL, priority queues. For SQS: standard vs FIFO queues, visibility timeout, long polling, DLQ redrive.

**When you'd reference it in interviews:** Task queues (email sending, image processing), work distribution with retries, when you need point-to-point delivery guarantees without Kafka's complexity. The key differentiator: know *when NOT to use Kafka* — if you don't need ordering, replay, or high throughput, a simpler broker is the right call.

**Kafka vs RabbitMQ decision framework:**
- Need replay / event sourcing / ordering → Kafka
- Need routing logic / priority / per-message ack + retry → RabbitMQ/SQS
- Need simple task distribution with DLQ → SQS

| Resource | Why |
|----------|-----|
| [RabbitMQ Tutorials (official)](https://www.rabbitmq.com/tutorials) | 6 progressively complex tutorials covering all exchange types |
| [CloudAMQP — "RabbitMQ for Beginners"](https://www.cloudamqp.com/blog/part1-rabbitmq-for-beginners-what-is-rabbitmq.html) | Clear visual explanations of the AMQP model |
| [AWS SQS Developer Guide](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/) | Standard vs FIFO, visibility timeout, DLQ patterns |
| [ByteByteGo — "Kafka vs RabbitMQ"](https://bytebytego.com/) | Side-by-side comparison with use-case mapping |

---

### Apache Spark / Flink (Batch + Stream Processing)

**What to understand:**
- **Spark:** RDD abstraction → DataFrames/Datasets, lazy evaluation + DAG, narrow vs wide transformations (shuffle), partitioning strategies, Spark SQL, structured streaming (micro-batch), memory management (tungsten).
- **Flink:** True stream processing (event-at-a-time), event time vs processing time, watermarks, exactly-once via checkpointing (Chandy-Lamport), state backends (RocksDB), windowing (tumbling, sliding, session).

**When you'd reference it in interviews:** "Design a recommendation engine," "Design analytics for YouTube," "Design a fraud detection system," "Design a data pipeline." Any system needing batch ETL or real-time stream processing at scale.

| Resource | Why |
|----------|-----|
| [Apache Spark Documentation — Overview & Programming Guides](https://spark.apache.org/docs/latest/) | Official docs covering RDDs, DataFrames, Spark SQL, and Structured Streaming. (Book: *Spark: The Definitive Guide* if you want a deeper read) |
| [Flink Training (official Apache)](https://nightlies.apache.org/flink/flink-docs-stable/docs/learn-flink/overview/) | Hands-on exercises covering event time, watermarks, state, checkpoints |
| DDIA Ch. 10 (Batch Processing) + Ch. 11 (Stream Processing) | MapReduce → Spark lineage, exactly-once semantics, stream-table duality |
| [Netflix — "Keystone Real-Time Stream Processing Platform"](https://netflixtechblog.com/) | How Netflix processes trillions of events/day |
| [Uber — "AthenaX: Uber's Stream Analytics Platform"](https://www.uber.com/blog/engineering/) | Real-world Flink deployment at scale |

---

### AWS Glue / Data Lakes / Lakehouse Architecture

**What to understand:**
- **Data Lake concept:** Schema-on-read vs schema-on-write, raw/curated/consumed zones, separation of storage and compute.
- **AWS Glue:** Crawlers (schema discovery), Data Catalog (Hive metastore compatible), ETL jobs (PySpark under the hood), job bookmarks (incremental processing), Glue Studio (visual ETL).
- **Storage formats:** Parquet (columnar, predicate pushdown, stats in footer), ORC, Avro (row-based, schema evolution). Why columnar matters for analytics.
- **Table formats:** Apache Iceberg / Delta Lake / Hudi — ACID transactions on data lakes, time travel, schema evolution, partition evolution.
- **Query engines:** Athena (serverless Presto/Trino over S3), Redshift Spectrum, EMR.

**When you'd reference it in interviews:** "Design an analytics platform," "Design a data warehouse," "How would you build reporting for 10TB+ daily data?" Any system where OLAP workloads sit alongside OLTP, or when you need to explain how raw events become queryable insights.

| Resource | Why |
|----------|-----|
| [AWS Glue Developer Guide](https://docs.aws.amazon.com/glue/latest/dg/) | Official — crawlers, catalog, ETL authoring, job bookmarks |
| [AWS Big Data Blog — Data Lake posts](https://aws.amazon.com/blogs/big-data/) | Real architectures: S3 → Glue → Athena/Redshift pipelines |
| [Databricks — "Lakehouse Architecture"](https://www.databricks.com/glossary/data-lakehouse) | Why lakehouse emerged (data lake + warehouse unification), Delta Lake mechanics |
| [Apache Iceberg Documentation](https://iceberg.apache.org/docs/latest/) | Table format that solves data lake consistency — hidden partitioning, time travel, snapshot isolation |
| [Martin Kleppmann — "Turning the Database Inside-Out"](https://www.confluent.io/blog/turning-the-database-inside-out-with-apache-samza/) | Conceptual bridge between streaming (Kafka) and materialized views (data lake) |
| [ByteByteGo — "Data Lake vs Data Warehouse"](https://bytebytego.com/) | Visual comparison of architectures and when to use which |

---

### GraphQL / gRPC / API Gateway Patterns

**What to understand:**
- **GraphQL:** Schema-first, resolver pattern, N+1 problem (DataLoader), subscriptions for real-time, batching, persisted queries, federation (multiple services behind one graph).
- **gRPC:** Protocol Buffers (schema + binary serialization), HTTP/2 (multiplexing, header compression), streaming (unary, server, client, bidirectional), service mesh integration, deadline propagation.
- **API Gateway:** Request routing, rate limiting, auth (JWT validation), request/response transformation, circuit breaking, canary routing.

**When you'd reference it in interviews:** Internal service-to-service communication (gRPC), client-facing flexible APIs (GraphQL), or when asked "how do clients talk to your system?" with more depth than "REST."

| Resource | Why |
|----------|-----|
| [gRPC Official Docs — Core Concepts](https://grpc.io/docs/what-is-grpc/core-concepts/) | Streaming types, deadlines, metadata, interceptors |
| [GraphQL Best Practices (graphql.org)](https://graphql.org/learn/best-practices/) | Official guide — pagination, caching, auth patterns |
| [Netflix — "How Netflix Scales its API with GraphQL Federation"](https://netflixtechblog.com/) | Real-world federation at scale |
| [Kong API Gateway — Architecture Patterns](https://konghq.com/blog) | Gateway patterns: rate limiting, auth, transformation |
| [ByteByteGo — "gRPC vs REST"](https://bytebytego.com/) | Visual comparison of protocol overhead, streaming, use cases |

---

### Monitoring / Observability Stack (Prometheus, Grafana, OpenTelemetry)

**What to understand:** The three pillars — metrics (Prometheus, time-series DB, PromQL, pull-based scraping), logs (ELK stack or Loki, structured logging), traces (distributed tracing, span propagation, OpenTelemetry). Also: alerting philosophy (symptom-based > cause-based), SLO/SLI/SLA, cardinality explosion in metrics.

**When you'd reference it in interviews:** The "wrap-up" phase of any design — "how would you know this is healthy?" Also relevant when discussing failure detection, debugging latency, or capacity planning.

| Resource | Why |
|----------|-----|
| [Google SRE Book — Monitoring Distributed Systems (free chapter)](https://sre.google/sre-book/monitoring-distributed-systems/) | The "four golden signals" framework (latency, traffic, errors, saturation) |
| [Prometheus Documentation](https://prometheus.io/docs/introduction/overview/) | Pull model, PromQL, histogram vs summary, recording rules |
| [OpenTelemetry Documentation](https://opentelemetry.io/docs/) | Vendor-neutral instrumentation — traces, metrics, logs unified |
| [Grafana Labs — "Introduction to Observability"](https://grafana.com/docs/grafana/latest/) | How metrics + logs + traces come together in dashboards and alerts |
| [Charity Majors — "Observability Engineering" (O'Reilly)](https://www.oreilly.com/library/view/observability-engineering/9781492076438/) | Modern observability philosophy — high-cardinality, wide events, exploration over dashboards |

---

### Container Orchestration (Kubernetes Concepts)

**What to understand:** Pod abstraction, Service (ClusterIP, NodePort, LoadBalancer), Deployment (rolling update, rollback), StatefulSet (stable network IDs, ordered scaling for DBs), HPA (Horizontal Pod Autoscaler), Ingress (L7 routing), ConfigMap/Secrets, etcd as the control plane store, scheduler (bin-packing, affinity/anti-affinity).

**When you'd reference it in interviews:** Deployment strategy ("how would you deploy this?"), auto-scaling ("how does your system handle traffic spikes?"), service discovery ("how do services find each other?"). You don't need to design Kubernetes, but you should be able to reference its patterns fluently.

| Resource | Why |
|----------|-----|
| [Kubernetes Documentation — Concepts](https://kubernetes.io/docs/concepts/) | Official and well-structured — Pods, Services, Deployments, StatefulSets |
| [The Kubernetes Book (Nigel Poulton)](https://www.amazon.com/Kubernetes-Book-Version-November-2018-ebook/dp/B072TS9ZQZ) | Accessible, diagram-heavy walkthrough of all core concepts |
| [Google Borg Paper](https://research.google/pubs/large-scale-cluster-management-at-google-with-borg/) | The system that inspired Kubernetes — scheduling, resource allocation, fault tolerance |
| [ByteByteGo — "Kubernetes Explained"](https://bytebytego.com/) | Visual overview of control plane, data plane, and how a request flows |
| [Envoy + Istio (service mesh)](https://istio.io/latest/docs/concepts/) | How K8s services communicate with mTLS, retries, circuit breaking — relevant for "how do microservices talk?" |

---

## Tracking

Mark each day done as you go. If you miss a day, skip rather than compress — depth matters more than breadth at L5+.

| Week | Day 1 | Day 2 | Day 3 | Day 4 | Day 5 | Day 6 | Day 7 |
|------|-------|-------|-------|-------|-------|-------|-------|
| 1    | [ ]   | [ ]   | [ ]   | [ ]   | [ ]   | [ ]   | [ ]   |
| 2    | [ ]   | [ ]   | [ ]   | [ ]   | [ ]   | [ ]   | [ ]   |
| 3    | [ ]   | [ ]   | [ ]   | [ ]   | [ ]   | [ ]   | [ ]   |
| 4    | [ ]   | [ ]   | [ ]   | [ ]   | [ ]   | [ ]   | [ ]   |
| 5    | [ ]   | [ ]   | [ ]   | [ ]   | [ ]   | [ ]   | [ ]   |
