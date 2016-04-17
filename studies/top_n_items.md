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

### Scaling
