## Design a high-availability file sync system like Dropbox
An excellent talk is here: https://www.youtube.com/watch?v=PE4gwstWhmc

### Supported Operations
1. User should be able to upload/download any file from his/her pc to the service
2. Users should be able to sync their entire repo on the service with their pc
3. Undo: User should be able to restore the file at a previous stage
4. ACID operations: 
   * Atomic: File upload should be all or none 
   * Consistency: Both versions on pc and server must be same
   * Isolation: ?
   * Durability: Must be highly available

### Scale of the Problem
1. 10 million users
2. 100 million requests per day
3. Very high write/read ratio (almost 1:1)

### Abstract Architecture
1. Read-Write App Servers
   * A sync operation happens on read-write server
2. Database
   * NoSQL database to store metadata related to users, files
3. Memcache
   * All app servers periodically write their results to memcache
4. Load Balancers
   * All requests to app servers go through load balancers
5. Metadata servers
   * All logs of user interactions, user sessions etc stored in these
6. Distributed filesystem
   * Storing actual file objects

### Operations
1. A user's login goes through load-balancer to one of the metadata servers that performs authentication/session management work and sends a cookie back to user. THe cookie, along with usual session-related parameters, also contains internal id of read-write server that all subsequent requests from this user must be directed to corresponding read-write server
2. File download/upload: A read-write server communicates with distributed FS to retrieve or save corresponding file. The server also logs user's interaction, change delta and commits changes to Memcached. 
3. Memcache then stores these changes to NoSQL database.
4. Undo: The app server queries the change to database (through memcache) and then writes it out to filesystem through lock.

### Bottlenecks
