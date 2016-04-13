## A state-of-the art ticket selling system like TicketMaster or Fandango

### Supported Operations
1. List: A user should be able to see a list of all currently running shows
2. Details: A user should be able to click on a show and see its details, show-times/seats/sections/prices
3. Buy: A user should be able to buy a particular ticket
4. Login, Profile History for a user
5. Seller: List a show/movie for a seller?

### Scale of the problem
1. About 50 active movie listings at a time with 6 shows per day per movie on an average. Assume a movie plays at around 1000 locations throughout the country
2. About 1M ticket sales per day ~ 10 sales per second
3. 10M views per day

### Abstract Architecture
1. Application layer behind a load balancer
  * Takes user requests of multiple form: list, login, Credit-Card info and the directs them to various service
2.Database Layer: Replicated NoSQL Database
  * User object: Contains names, password hashes, history (MongoDB's arraytypes as a starting point)
  * Movie Objects: (name, showtime, theater, expiry) of a movie
3. Transaction Layer:
  * Memcached to return an accurate count of available number of tickets for a Movie object. Design decision: Should these be dedicated application servers or can the computation be done using MemCached?
  * This layer also takes care of a ticket purchase through transaction lock. 
4. Caching:
  * Obvious caching for currently active movie-showtime. Never have to hit database in fast-path during transaction for current movies. Instead use a fast cache to count/update tickets
  * LRU cache for users. User-data changes much less frequently than a movie data. 
  * Caching all movie-ids for listing them (Redis or Memcached)? A listing of movies should be served from this cache and should never query the databases.
5. Static contents: Very media-heavy like movie trailers, posters and cast info, Served through CDNs (Akamai, Amazon etc). 
   * Insert a movie-related static contents in CDN when a movie is added to the DB. These remain fixed as long as the movie is running.
