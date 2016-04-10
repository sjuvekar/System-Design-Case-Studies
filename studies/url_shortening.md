## Supported operations
1. Shortening: Given a long URL, return a short URL
2. Redirection: Given a short URL, redirect to the original  URL if exists
3. Custom URL: Allow the users to specify custom short URL and associate with long URL
4. Analytics: Count the number of hits/redirections to shortened URL
5. Expiry/Deletion: Allow deletion of the URL
6. High Avaialability

## Scale of the problem
1. ~200 reads per second
2. ~20 writes per second
3. ~10TB of storage over five years

## Abstract Architecture
Abstract Architecture has three layers
1. Application Layer:
  * Shortening Service: Get the URL request from the user and shorten
  * Redirection Service: Get a shortened URL and redirect it to the original URL
2. Data Storage Layer:
  * Stores two directional maps: (URL => Shortened URL) and (Shortened URL => URL)
  * md5 hash along with some simple operations (like adding secret salt and taking base 62 digits) is the most suitable candidate.

## Operations
1. Application layer receives shortening request
  * Check if shortened URL already exists in data layer, and return it.
  * Else create a shortened URL, check if it already exists. In case of collision, keep generating new.
  * Save the new URL in Data Storage and return
2. Application layer receives redirection request
  * Checks if original URL exists in inverse database and return. 
  * Else error
3. Auto expiry: Every day at fixed time in Data layer?
