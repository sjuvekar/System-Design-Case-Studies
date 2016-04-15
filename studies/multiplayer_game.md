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


