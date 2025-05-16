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
Some challenges faced consisted of getting the socket connection to work, and to correctly account for TLS use. I did this by wrapping the plain socket in an SSL context, without certification verification, but this allowed for the channel to still be encrypted.

Another challenge was with deciding how to filter automated guesses, and ensure consequent guesses were actually improvements, or at the very least as good as the last guess. This was especially more challenging to think about for the marks that were in the word, but not in the right position, as words that were still valid would require the letter, but not need it in the same position or same amount in the word. I worked on solving this by generating a history of past guesses, to ensure those weren't guess again, and by comparing the most recent guess to the rest of the list of words in words.txt.

A final big challenge I faced was figuring out how to get a visual of how my code was actually working, to which I temporarily added debugging print statements (no longer there as I didn't want to leave excess comments) which helped me get a clear understanding of what was going on and overcoming the challenge of knowing where I was actually at in the code.

## Guessing Strategy
My guessing strategy starts with making a random first guess from the entire list, instead of having a specific starter word every time. Meaning it could have the small chance of being the exact word in one go, or have no matches at all.
The history of previous guesses is maintained in order to keep track of what words from the list have been guessed, to both minimize guessing time and improve accuracy. Otherwise, if the previous guesses were still being used the quality of each next guess would never improve.
In the cases of subsequent guesses, the available list of words (not in the history of guesses) are compared to the most recent last guess, and only words from the dictionary that match the same marks that guess had will then be used. This ensures that every next guess will be more accurate than the last, or at the very least just as accurate as the last guess, narrowing down the list of candidates until the correct guess is made.

## Testing
The way I tested was by simply running both versions, the default and TLS, in command line and ensuring at a basic level the flags were at least printed in the command line. 
Beyond that, I also tested by adding debug print statements to ensure the correct steps were being made by my code, and also to get an idea of how many guesses my guessing strategy would take on average. In addition to this, I conducted my own local unit testing to get a real example of what the code was executing, and I did this by using manually created histories of guesses that were on a much smaller level to test the filtering logic.

