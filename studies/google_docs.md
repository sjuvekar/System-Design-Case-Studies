## Design a realtime document collaboration system

This is a quick overview of different techniques and algorithms: https://neil.fraser.name/writing/sync/. Synchronization is a key here.

### Supported Operations
1. Synchronous Edits: pretty much the only operation required from user. However, editing should adhere to some requirements
  * Maximum concurrency: Allow as many users as possible to edit same document
  * Consistency: user's edits to a common section of a file should not be interleaved, only one user's commit should be finalized in the main document
  
### Scale of the Problem
1. millions of total users.
2. Thousands of active users at any given time 
3. Some reasonable number of users possibly modifying same document (maybe 10?)

### Abstract Architecture
1. App Servers
 * A single document is edited on one app server. The app server must use some policy to resolve conflicts between documents. Incremental merge (like git/svn) is one good strategy for that. The user's browser periodically sends deltas in document being edited to the app server (after every keystroke maybe). The app server maintains a queue of recent edits and merges them one at a time by locking over a document. A successful edit is accepted silently. An unsuccessful edit ('merge conflict') triggers an HTTP-message sent back to the user's browser. Browser's javascript has a responsibility to restore the document to the unmerged state and also flash an error message to the editing environment.
2. Load Balancer
 * For directing users to appropriate app server
3. Caching
 * Memcached to periodically cache (app-server id, edited document states) pairs, in case app server goes down
4. Storage
  * Save edited document

### Operations
1. A user starts editing a document. Load balancers check for document's id and check if it is currently being updated. In that case, it routers the user to the corresponding app server. Also sends a cookie back to the user with the unique internal id of the app server for session maintainance. 
2. If the document is not currently edited, load balancer selects an app-server using its scheduling policy. The app server performs a DB query to fetch the document.
2. The app server then uses its incremental merge algorithm to edit the document. It also periodically saves the snapshot of the document to memcached every 1 second.
3. Memcached saves its own documents to a relational database periodically.

### Bottlenecks
1. App server could crash: In that case, load balancer restores most recent copy of document from Memcached to a fresh app server, updates its state, sends a fresh cookie to the user. All intermediate changes are lost
2. Long queue at an app server resulting in delayed response to the user: (can't think of a solution)
3. DB/cache failure: Use standard replicated architecture.
4. One user getting constant merge conflicts because of slow connection
