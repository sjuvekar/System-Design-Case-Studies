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

### Bottlenecks
