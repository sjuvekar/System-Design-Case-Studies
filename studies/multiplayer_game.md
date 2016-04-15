## An architecture for an online multiplayer game (chess, poker)

### Supported operations
1. Game Play: Most important operation. Allow for persistance (should resume from previous state after a connection drop)
2. Login/Profile: A user must be able to log in and save previous games
3. Audit/History: Save previous played games and reload them for a review

### Scale of the Problem
1. ~100 simultaneous games
2. ~100k users
3. A few power users (playing ~1000 games per month) and many small-time users

