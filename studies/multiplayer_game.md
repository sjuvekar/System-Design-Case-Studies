## An architecture for an online multiplayer game (chess, poker)

### Supported operations
1. Game Play: Most important operation. Allow for persistance (should resume from previous state after a connection drop)
2. Login/Profile: A user must be able to log in and save previous games
3. Audit/History: Save previous played games and reload them for a review

### Scale of the Problem
1. ~100 simultaneous games
2. ~100k users
3. A few power users (playing ~1000 games per month) and many small-time users

### Abstract Architecture
1. App Servers
  * One dedicated app-server per game? maybe too much? 
2. Session Managers
  * Session managers are probably the most important parts of the architecture. Where should a user's current session be stored? A Memcache layer to store the currently running game-session sounds a good idea.
3. Databases
  * (NoSQL?) database for completed games. Each object in database maybe looks like (game_id, move_id, player_who_made_move, timestamp)
  * (NoSQL?) database for registered users. A user object should at the least have basic attributes like id, name. Additional attributes can be a list of completed game ids, score etc.
4. Load balancers
  * base load-balancers to direct new user to an app server
  * Need more?
5. Worker queues
  * One queue can be used in slow path to update the game database after a player makes a move for a particular game. (Store player_id, move_id, game_id in database and also icrement move_id for that game)

### Operations
1. The load balancer keeps a list of currently running game and also sends to the app server serving the landing page. A user can do few things as soon as he/she visits the website
  * Join an existing game: Load balancer directs the user to the appropriate application server. The application server fetches its session information from memcache. Inserts user's information (browser, ip and a user_id etc) in the session. Also sends a cookie to user that contains standard fields along with an internal id representing app server and session info. Any subsequent request from this user must contain this cookie.
  * Create a new game: Load balancer directs the user to next 'free' app server. Most of the remaining steps are identical to above
  * Signup/Login: Presents a login form, updates the user database right away and sends login session cookie to the user. An entire architecture for user session management using memcached (http://highscalability.com/blog/2008/11/2/strategy-how-to-manage-sessions-using-memcached.html)
  * 

