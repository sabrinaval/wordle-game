# wordle-game

## High-Level Approach

1. Protocol Implementation for Connection and JSON Messages
   - Establishes a TCP connection (port 27993) or a TLS-wrapped socket--port 27994 with flag `-s`.  
   - Sends and receives JSON messages line-by-line with socket connection:  
     1. Sends a `hello` with Northeastern username 
     2. Receives a `start` message with a game ID
     3. Loops and sends `guess` messages, receives `retry` with feedback history, or `bye` with the flag

2. Word List Reading
   - Reads `words.txt` as one five-letter word per line into memory
   - Uses this list for the entirety of the game, basis for searching for valid candidates

3. Automated Guessing
   - Picks a random word as the first guess
   - For every next guess required, filters the word list to narrow it down to only valid candidates that would reproduce the exact feedback marks for every previous guess, so they are either equivalent or better guesses
   - Randomly selects one of the remaining valid candidates from the curated list

## Challenges Faced
Some challenges faced consisted of getting the socket connection to work, and to correctly account for TLS use. I did this by wrapping the plain socket in an SSL context  



## Guessing Strategy




## Testing


