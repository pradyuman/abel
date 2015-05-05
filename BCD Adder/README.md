This ABEL program uses a binary-coded-decimal (BCD) adder to implement a guessing game.

The target CPLD is: ispMACH LC4256ZE 144-pin TQFP (LC4256ZE-5TN144C)

This program creates a guessing game, where players have 9 rounds to guess what a pseudo-randomly generated radix number is (the number is in between radix -8 and +7).
A player is awarded 0 points if their guess is higher than the actual value, 4 points if the guess is less than the actual value, and 9 points if the guess is exactly the actual value.
A player wins if their score is at least 50 by the end of the 9 rounds.

The point total for the player is displayed using two BCD digits on DIS2 and DIS1. A 4-bit linear feedback shift register will be used to generate the pseudo-random radix number.
The player enters their guess using DIP3..DIP0. The guesses are clocked in using S1.

The jumbo red LED will be illuminated if the guess was greater than the actual value.
The jumbo yellow LED will be illuminated if the guess was less than the actual value.
The jumbo green LED will be illuminated if the guess was equal to the actual value.

Once the 9th round is complete, the device will display the player's score as well as the string "yEAH" if the player won or the string "LoSEr" if the player lost.
This string will scroll on the displays until S2 is pressed to asynchronously reset the game.

If DIP7 is asserted, the player can play in cheat mode, where the radix value is displayed on the top row of LEDs.
