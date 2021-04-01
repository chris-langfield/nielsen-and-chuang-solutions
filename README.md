# nielsen-and-chuang-solutions
Detailed solutions to Nielsen and Chuang, "Quantum Computation and Quantum Information". This is my own work and I cannot guarantee the correctness of the solutions.

## Chapter 3 

### 3.2



### 3.3

*(Turing machine to reverse a bit string) Describe a Turing machine which takes a binary number x as input, and outputs the bits of x in reverse order. (Hint: In this exercise and the next it may help to use a multi-tape Turing machine and/or symbols other than ◃, 0, 1 and the blank.)*

I feel like having two tapes will either make this too trivial or make me want to pull my hair out. I didn't find out because I used the second part of the hint and decided to use a single tape, but include an extra character in my alphabet: 2.  In this solution I used the assumption of a tape that is infinite in one direction starting with a leftmost "starting" square, that I denote with $. So the challenge is to come up with a program (instruction set) that will read in an arbitrary binary string ("0,1,0,1,1,1,1,0,1,1,0...") and print out its reverse (e.g. "0,1,1,0,1,1,1,1,0,1,0") onto the tape, and then halt.

We can start by coming up with any old string, say "1110010" to try stuff out on. So with an alphabet G = {0, 1, 2}, the read/write pointer beginning on the starting square, $, and an initial state we can call Q<sub>s</sub>, our tape will have the starting square, followed by our binary string, followed by blanks to infinity: (bold indicates position of the read/write pointer)

Machine State: Q<sub>s</sub>

Program: ???

**$** -1-1-1-0-0-1-0-b-b-b-b-b-b-b-b-b-b-b-b-b- ...

We need to read in that first "1", and somehow stick it at the end of the string, then read in the second "1", and put it right before the last character, and so on. But the thing is, if we overwrite the last character, the penultimate character, and so on, then we'll have erased information we need. Those two characters, of course, have to be the first and second characters of our output.

So, wanting to keep the input string intact, we'll take advantage of the infinite space to the right of the input, and just start writing the output into the series of blanks. 

We face another problem now. If we start with the first character of input, we'll somehow need to transport that information to the right, across our entire output, out into the field of blanks, to position 14 (since our string has length 7) on the tape. We don't have any counter variables we can assign, though, we can only use transition functions (the program lines). 

We can do less traveling up and down the tape by starting with the last character of our input, and just writing it out to the first blank space we encounter. The plan is to take the last input character, write it to the first blank character, take the second-to-last input character, write it to the second blank, and so on:

$-1-1-1-0-0-1-0-X-X-X-X-X-X-X-b-b-b-b-b-b- ...

So first we define a program line to get us going:

[1] (Q<sub>s</sub>, $) => (Q<sub>1</sub>, $, R)

This moves us into a new Q<sub>1</sub> state, leaves the starting square as we found it, and moves to the right (R).

Now, we want to skip over the rest of our input until we reach the first blank square, so we'll define the following transitions:

(Q<sub>1</sub>, 0) => (Q<sub>1</sub>, 0, R);
(Q<sub>1</sub>, 1) => (Q<sub>1</sub>, 1, R);
(Q<sub>1</sub>, b) => (Q<sub>2</sub>, b, L);

Everytime we encounter a 0 or 1 in state Q<sub>1</sub>, we just leave it alone and continue on to the right. Once we reach a blank "b", however, we want to turn back around, and we change state so that we know we're going back to the left now (otherwise, if we stay in Q<sub>1</sub> we'll reach another 0 or 1 in the next step and turn right back around creating an infinite loop). 

Now we're in Q<sub>2</sub> and we want to find the last character of the input. But we don't know whether that will be a 1 or a 0. To keep track of this, we need to create two more states that let us know whether we found a 0 or a 1:

(Q<sub>2</sub>, 0) => (Q<sub>3</sub>, 2, R);
(Q<sub>2</sub>, 1) => (Q<sub>4</sub>, 2, R);

Here we will capture the value of the bit, and then overwrite it with our additional character, 2, so that we know we already dealt with this character in the input, and then turn back around.

So on our example string, after staying in Q<sub>1</sub> and reaching the end of the string, we reach the first blank, switch to Q<sub>2</sub>, move left, find a "0" there, and switched to Q<sub>3</sub>. Since we're in Q<sub>3</sub>, we know we have read in a "0", so we need to write a "0" next time we encounter a "b". But what state should we enter after dropping off our zero? While working this out, I thought I could just switch back to Q<sub>2</sub>, and head back left until I hit the next input character (This only works when writing the first input character, as we will see):

(Q<sub>3</sub>, b) => (Q<sub>2</sub>, 0, L)
(Q<sub>4</sub>, b) => (Q<sub>2</sub>, 1, L)

So now we've got:

$-1-1-1-0-0-1- **2** -0-X-X-X-X-X-X-b-b-b-b-b-b- ...

And we're in state Q<sub>2</sub>. We haven't defined a transition for Q<sub>2</sub> when it encounters a 2. We want to just ignore this value since we already copied over what was there:

(Q<sub>2</sub>, 2) => (Q<sub>2</sub>, 2, L)

When we get to the penultimate input character, we will overwrite it with a 2, change to state Q<sub4</sub>, and move to the right, using expression 




