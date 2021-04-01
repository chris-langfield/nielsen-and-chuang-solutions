# nielsen-and-chuang-solutions
Detailed solutions to Nielsen and Chuang, "Quantum Computation and Quantum Information". This is my own work and I cannot guarantee the correctness of the solutions.

## Chapter 3 

### 3.2

*(Turing numbers) Show that single-tape Turing machines can each be given a number from the list 1,2,3,... in such a way that the number uniquely specifies the corresponding machine. We call this number the Turing number of the corresponding Turing machine. (Hint: Every positive integer has a unique prime factorization pa<sub>1</sub> pa<sub>2</sub> . . . pa<sub>k</sub> , where p<sub>i</sub> are distinct prime numbers and a<sub>1</sub>,...,a<sub>k</sub> are non-negative integers.)*

We define an arbitrary Turing machine with states Q<sub>1</sub> ... Q<sub>m</sub>, alphabet G = {X<sub>1</sub>, ... X<sub>n</sub>}, and transition functions (program lines) (Q<sub>i</sub>, X<sub>j</sub>) => (Q<sub>ij</sub>, X<sub>ij</sub>, S), where i = 1,2,...m, and j = 1,2,...n and S=0,1,2 (1 to go right, 2, to go left, 0 to not move). 

Each program line can be specified by 5 integers: i, j, u<sub>ij</sub>, v<sub>ij</sub>, and s<sub>ij</sub> where u<sub>ij</sub> is the number from 1 to m of the state specified in the program, and v<sub>ij</sub> is the character number from 1 to n of the character to be written by that line. s<sub>ij</sub> is the movement (0, 1, 2) specified given state i and character g<sub>j</sub> read. For instance if we had lines

(Q<sub>1</sub>, X<sub>1</sub>) => (Q<sub>2</sub>, X<sub>1</sub>, Right)

(Q<sub>3</sub>, X<sub>2</sub>) => (Q<sub>1</sub>, X<sub>1</sub>, Left)

We could represent these as lists of integers: [1,2,2,1,1] and [3,2,1,1,2] respectively. 

If we decide on the convention of iterating over the character numbers first and then the state numbers, we could read out the program lines as:

[1,1,u<sub>11</sub>,v<sub>11</sub>,s<sub>ij</sub>], [1,2,u<sub>12</sub>,v<sub>12</sub>,s<sub>12</sub>], ... [1,n,u<sub>1n</sub>,v<sub>1n</sub>,s<sub>1n</sub>]

. . .

[m, 1, u<sub>m1</sub>,v<sub>m1</sub>,s<sub>m1</sub>], [m,2,u<sub>m2</sub>,v<sub>m2</sub>,s<sub>m2</sub>], ... [m,n,u<sub>mn</sub>,v<sub>mn</sub>,s<sub>mn</sub>]


This gives us an ordered list of mn 5-tuples of integers, which we can convert to an ordered list of 5mn integers (by following our iterating convention above). We *could* just concatenate this large list of integers, which will be unique for each Turing Machine, and take the literal decimal value of all those numbers slapped together. But following the hint, we can also define the Turing Number as the integer having prime factorization:

2<sup>1</sup> 3<sup>1</sup> 5<sup>u<sub>11</sub></sup> 7<sup>v<sub>11</sub></sup> 9<sup>s<sub>11</sub></sup> 11<sup>1</sup> 13<sup>2</sup> 17<sup>u<sub>12</sub></sup> 19<sup>v<sub>12</sub></sup> 23<sup>s<sub>12</sub></sup> 29<sup>1</sup> 31<sup>3</sup> ...

Which would also uniquely define an integer associated with this Turing machine.


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

[2] (Q<sub>1</sub>, 0) => (Q<sub>1</sub>, 0, R)

[3] (Q<sub>1</sub>, 1) => (Q<sub>1</sub>, 1, R)

[4] (Q<sub>1</sub>, b) => (Q<sub>2</sub>, b, L)

Everytime we encounter a 0 or 1 in state Q<sub>1</sub>, we just leave it alone and continue on to the right. Once we reach a blank "b", however, we want to turn back around, and we change state so that we know we're going back to the left now (otherwise, if we stay in Q<sub>1</sub> we'll reach another 0 or 1 in the next step and turn right back around creating an infinite loop). 

Now we're in Q<sub>2</sub> and we want to find the last character of the input. But we don't know whether that will be a 1 or a 0. To keep track of this, we need to create two more states that let us know whether we found a 0 or a 1:

[5] (Q<sub>2</sub>, 0) => (Q<sub>3</sub>, 2, R)

[6] (Q<sub>2</sub>, 1) => (Q<sub>4</sub>, 2, R)

Here we will capture the value of the bit, and then overwrite it with our additional character, 2, so that we know we already dealt with this character in the input, and then turn back around.

So on our example string, after staying in Q<sub>1</sub> and reaching the end of the string, we reach the first blank, switch to Q<sub>2</sub>, move left, find a "0" there, and switched to Q<sub>3</sub>. Since we're in Q<sub>3</sub>, we know we have read in a "0", so we need to write a "0" next time we encounter a "b". But what state should we enter after dropping off our zero? While working this out, I thought I could just switch back to Q<sub>2</sub>, and head back left until I hit the next input character (This only works when writing the first input character, as we will see):

[7] (Q<sub>3</sub>, b) => (Q<sub>2</sub>, 0, L)

[8] (Q<sub>4</sub>, b) => (Q<sub>2</sub>, 1, L)

So now we've got:

$-1-1-1-0-0-1- **2** -0-b-b-b-b-b-b-b-b-b-b-b-b-b- ...

And we're in state Q<sub>2</sub>. We haven't defined a transition for Q<sub>2</sub> when it encounters a 2. We want to just ignore this value since we already copied over what was there:

[9] (Q<sub>2</sub>, 2) => (Q<sub>2</sub>, 2, L)

When we get to the penultimate input character, "1", we will overwrite it with a 2, change to state Q<sub4</sub>, and move to the right, using expression [6]. 

$-1-1-1-0-0- **2** -2-0-b-b-b-b-b-b-b-b-b-b-b-b-b- ...

But now we have to go back over the 2 and the 0 to get to our next "b". We want to ignore anything other than a "b" when we're in either Q<sub>3</sub> or Q<sub>4</sub>:

[9] (Q<sub>3</sub>, 0) => (Q<sub>3</sub>, 0, R)

[10] (Q<sub>3</sub>, 1) => (Q<sub>3</sub>, 1, R)

[11] (Q<sub>3</sub>, 2) => (Q<sub>3</sub>, 2, R)

[12] (Q<sub>4</sub>, 0) => (Q<sub>4</sub>, 0, R)

[13] (Q<sub>4</sub>, 1) => (Q<sub>4</sub>, 1, R)

[14] (Q<sub>4</sub>, 2) => (Q<sub>4</sub>, 2, R)

After switching to Q<sub>4</sub> and reaching the second blank, we're here:

$-1-1-1-0-0-2-2-0- **1** -b-b-b-b-b-b-b-b-b-b-b-b- ...

We're in state Q<sub>2</sub> but... according to eqn [5], this means that when we hit the 0 to our left, we'll switch to Q<sub>3</sub> and overwrite our first output character with a 2! This isn't right: we need to skip over already-written output, and the 2s, and reach our remaining input characters. So we need a new state that will allow us to go left over our output without modifying it. So if we are in state Q<sub>3</sub> or Q<sub>4</sub>, after replacing a blank with output, we need to go to a new state (**these replace equations [7] and [8]**)


[15] (Q<sub>3</sub>, b) => (Q<sub>5</sub>, 0, L)

[16] (Q<sub>4</sub>, b) => (Q<sub>5</sub>, 1, L)

When in Q<sub>5</sub>, we would like to ignore any "0" or "1", because we know this is our output:

[15] (Q<sub>5</sub>, 0) => (Q<sub>5</sub>, 0, L)

[16] (Q<sub>5</sub>, 1) => (Q<sub>5</sub>, 1, L)

But once we reach a 2, we know we've passed over our whole output string, and it is safe to switch back to Q<sub>2</sub> to start reading in input characters:

[17] (Q<sub>5</sub>, 2) => (Q<sub>2</sub>, 2, L)

And that's it! We can add a halt to our program by specifying:

[18] (Q<sub>2</sub>, $) => (Q<sub>h</sub>, $, _)

Where Q<sub>h</sub> indicates the halting state. The end result will be:

$-2-2-2-2-2-2-2-0-1-0-0-1-1-1-b-b-b-b-b-b-b- ...

We can clean this up by adding a new state that will help us cycle back and replace those "2"s with "b"s, but I will leave that as an exercise. 

**TL;DR**

Alphabet: {0, 1, 2}

Program: 

(Q<sub>s</sub>, $) => (Q<sub>1</sub>, $, R)

(Q<sub>1</sub>, 0) => (Q<sub>1</sub>, 0, R)

(Q<sub>1</sub>, 1) => (Q<sub>1</sub>, 1, R)

(Q<sub>1</sub>, b) => (Q<sub>2</sub>, b, L)

(Q<sub>2</sub>, 0) => (Q<sub>3</sub>, 2, R)

(Q<sub>2</sub>, 1) => (Q<sub>4</sub>, 2, R)

(Q<sub>2</sub>, 2) => (Q<sub>2</sub>, 2, L)

(Q<sub>3</sub>, 0) => (Q<sub>3</sub>, 0, R)

(Q<sub>3</sub>, 1) => (Q<sub>3</sub>, 1, R)

(Q<sub>3</sub>, 2) => (Q<sub>3</sub>, 2, R)

(Q<sub>4</sub>, 0) => (Q<sub>4</sub>, 0, R)

(Q<sub>4</sub>, 1) => (Q<sub>4</sub>, 1, R)

(Q<sub>4</sub>, 2) => (Q<sub>4</sub>, 2, R)

(Q<sub>3</sub>, b) => (Q<sub>5</sub>, 0, L)

(Q<sub>4</sub>, b) => (Q<sub>5</sub>, 1, L)

(Q<sub>5</sub>, 0) => (Q<sub>5</sub>, 0, L)

(Q<sub>5</sub>, 1) => (Q<sub>5</sub>, 1, L)

(Q<sub>5</sub>, 2) => (Q<sub>2</sub>, 2, L)

(Q<sub>2</sub>, $) => (Q<sub>h</sub>, $, _)

I worked this out on paper, and validated using: [this excellent TM simulator by Adrian Stoll](https://adrianstoll.com/turing-machine/turing-machine-simulator.html)













