## Design a high-availability file sync system like Dropbox
An excellent talk is here: https://www.youtube.com/watch?v=PE4gwstWhmc

### Supported Operations
1. User should be able to upload/download any file from his/her pc to the service
2. Users should be able to sync their entire repo on the service with their pc
3. Regular CRUD operations (Create, Read, Update, Delete) operations should be allowed on the repo. Results of these operations must be immediately available to syncing service (i.e. modifying a file on server and then syncing it must return modified version to the pc and overwirte the stale version).
4. ACID operations: 
   * Atomic: File upload should be all or none 
   * Consistency: Both versions on pc and server must be same
   * Isolation: ?
   * Durability: Must be highly available

### Scale of the Problem

### Abstract Architecture

### Operations

### Bottlenecks
