# nielsen-and-chuang-solutions
Solutions to Nielsen and Chuang, "Quantum Computation and Quantum Information"

## Chapter 3 

### 3.2



### 3.3

*(Turing machine to reverse a bit string) Describe a Turing machine which takes a binary number x as input, and outputs the bits of x in reverse order. (Hint: In this exercise and the next it may help to use a multi-tape Turing machine and/or symbols other than ◃, 0, 1 and the blank.)*

I feel like having two tapes will either make this too trivial or make me want to pull my hair out. I didn't find out because I used the second part of the hint and decided to use a single tape, but include an extra character in my alphabet: 2.  In this solution I used the assumption of a tape that is infinite in one direction starting with a leftmost "starting" square, that I denote with $. So the challenge is to come up with a program (instruction set) that will read in an arbitrary binary string ("0,1,0,1,1,1,1,0,1,1,0...") and print out its reverse (e.g. "0,1,1,0,1,1,1,1,0,1,0") onto the tape, and then halt.

We can start by coming up with any old string, say "1110010" to try stuff out on. So with an alphabet G = {0, 1, 2}, the read/write pointer beginning on the starting square, $, our tape will have the starting square, followed by our binary string, followed by blanks to infinity:

Machine State: Q_s

Program: ???

**V**

$-1-1-1-0-0-1-0-b-b-b-b-b-b-b-b-b-b-b-b-b- ...

We need to read in that first "1", and somehow stick it at the end of the string, then read in the second "1", and put it right before the last character, and so on. But the thing is, if we overwrite the last character, the penultimate character, and so on, then we'll have erased information we need. Those two characters, of course, have to be the first and second characters of our output.

So, wanting to keep the input string intact, we'll take advantage of the infinite space to the right of the input, and just start writing the output into the series of blanks. 

We face another problem now. If we start with the first character of input, we'll somehow need to transport that information to the right, across our entire output, out into the field of blanks, to position 14 (since our string has length 7) on the tape. We don't have any counter variables we can assign, though, we can only use transition functions (the program lines). 

We can do less traveling up and down the tape by starting with the last character of our input, and just writing it out to the first blank space we encounter.

So first we define a 
