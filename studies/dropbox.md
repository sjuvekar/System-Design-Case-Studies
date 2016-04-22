## Design a high-availability file sync system like Dropbox
An excellent talk is here: https://www.youtube.com/watch?v=PE4gwstWhmc

### Supported Operations
1. User should be able to upload/download any file from his/her pc to the service
2. Users should be able to sync their entire repo on the service with their pc
3. ACID operations: 
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
   * NoSQL database to store file objects
3. Memcache
   * All app servers periodically write their results to memcache
4. Load Balancers
   * All requests to app servers go through load balancers
5. File Stores for backup
6. Metadata servers
   * All logs of user interactions, user sessions etc stored in these

### Operations

### Bottlenecks
