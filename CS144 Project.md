#CS144 
#Cipherlock

# Vanilla Game
## Guard
- has 'true-passcode' -> the answer
	- 4 digit code from 1-6
- encrypted using 'true-cipher'
	- 1-1 mapping of the digits 1-6
	- essentially just gives each digit from 1-6 a specific digit that it changes to
- 'true-lockcode' is the passcode after using the cipher
## System
- saves the true-cipher and lock-code
- only requiring the true-passcode to gain entry
## Hacker
- hacker gets 10 guesses
### Guess-lockcode
#### Crack
- full dot (●) -> digit is in right position (GREEN Wordle)
- half-dot (○) -> right digit wrong position (YELLOW)
- no-dot (-) -> wrong digit (GRAY)
- symbols are unordered
	- full dots, half dots, no dots in that order
### Guess-cipher
#### Reveal
- indicates all mappings in the guess cipher that match those in the true cipher
- guess = 413651 and true = 213461
	- reveal = -13--1
