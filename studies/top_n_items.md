## Design a system that finds top N items in a stream of data

(This is an active area of research and There are many limited memory- or limited storage-based algorithms developed over the years: https://www.cs.berkeley.edu/~satishr/cs270/sp11/rough-notes/Streaming-two.pdf,  or http://dmac.rutgers.edu/Workshops/WGUnifyingTheory/Slides/cormode.pdf). Here, think about the problem in sense of an end-to-end system.

### Supported Operations
1. Query: A regular query sent by user. A sequence of such queries create a data stream
2. Top-N: A special query sent by the user that quickly returns top-N most frequent items searched so far (Can be approximate)

### Scale of the Problem
1. About ~10000 queries sent over a second (Order of Magnitude of Google)
2. About ~100 top-N queries perfomed per second
3. Most of the queries (90%) are repeat. That means ~1000 new words per second, with each word about 10 bytes long. ~300GB of storage required per year for storing new words.

### Abstract Architecture
1. Storage Layer: We have two options
  * Relational Database: Stores (word, count) tuples. Each new query requires two DB queries: One to find the word and second to insert updated count. Every top-N query performs a complex order-by query for the database.
  * Non-relational DataBase: Stores a word object: Every word object itself two attributes like relational database. Like relational database, reuires two queries to update a word on a new query and also complex query for top-N
2. Application Layer:
  * Dedicated app-servers for regular queries and top-N queries. Load-balancer routes requests to only relevant app-server (regular queries directed to regular app-server and vice-versa)
3. Worker Queues
  * A regular app server maintains a worker queue and pushes every new word to the queue. The queue has a responsibility to push new results in a bunch to the database. (Maybe update the cache first and then the cache updates the database).
4. Caching
  * Let's say that we decide hto go with a relational database. We maintain a memcahced t
### Scaling
