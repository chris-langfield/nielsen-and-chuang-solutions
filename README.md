# nielsen-and-chuang-solutions
Solutions to Nielsen and Chuang, "Quantum Computation and Quantum Information"

## Chapter 3 

### 3.2



### 3.3

*(Turing machine to reverse a bit string) Describe a Turing machine which takes a binary number x as input, and outputs the bits of x in reverse order. (Hint: In this exercise and the next it may help to use a multi-tape Turing machine and/or symbols other than ◃, 0, 1 and the blank.)*

I feel like having two tapes will either make this too trivial or make me want to pull my hair out. I didn't find out because I used the second part of the hint and decided to use a single tape, but include an extra character in my alphabet: 2.  In this solution I used the assumption of a tape that is infinite in one direction starting with a leftmost "starting" square, that I denote with $. So the challenge is to come up with a program (instruction set) that will read in an arbitrary binary string ("0,1,0,1,1,1,1,0,1,1,0...") and print out its reverse (e.g. "0,1,1,0,1,1,1,1,0,1,0") onto the tape, and then halt.

I begin by coming up with any old string, say "1110010" to try stuff out on. So with an alphabet G = {0, 1, 2}, the read/write pointer beginning on the starting square, $, our tape will have the starting square, followed by our binary string, followed by blanks to infinity:

**V**

$-1-1-1-0-0-1-0-b-b-b-b-b-b-b-b-b-b-b-b-b- ...
