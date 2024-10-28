#### Definition : 
* Equation that uses recursion to relate terms in a sequence or elements in an array
* a way to define a sequence or array in terms of itself
#### Applications :
* number theory -> Fibonacci sequence
* combinatorics -> distribution of objects into bins
* calculus -> Euler's method
* etc.
#### Why use recurrence ? 
used when an exhaustive approach to problem solving is too arduous to be practical
not ideal, but can be much more efficient than alternative of exhaustive casework

#### Setting up a recurrence relation
Complicated Problem + recursion = iterative process based on simpler versions of the problem

Example of application -> Tower of Hanoi Puzzle
![[Pasted image 20241028090738.png]]
The rules of the puzzle are as follows:
- The puzzle begins with all disks placed on one of the pegs. They are placed in order of largest to smallest, bottom to top.
- The goal of the puzzle is to move all of the disks onto another peg.
- Only one disk may be moved at a time, and disks are always placed onto pegs.
- Disks may only be moved onto an empty peg or onto a larger disk.

Let Tn​ be defined as the minimum number of moves needed to solve a puzzle with n disks.

##### Identifying a candidate problem to solve with a recurrence relation :
- The problem can be reduced to simpler cases. The Tower of Hanoi puzzle can be simplified by removing some of the disks.
- There is a numerical value, nn, to identify each case. For the Tower of Hanoi puzzle, this numerical value is the number of disks.
- The problem increases in complexity as the numerical identifier increases. As the number of disks in the Tower of Hanoi problem increases, it becomes more difficult to find a solution.

#### STEPS TO SOLVE THE PUZZLE :
1. Define a base case -> Simple version = 1 disk ![[Pasted image 20241028091056.png]]
one disk ->n = 1 -> T1 = 1 ( it will take only 1 move to move one disk into any other peg )

2. Solution when n = 2 :
![[iqxa5kox.bmp]]
In this case T2 = 3 

3. n = 3 : ![[dsd64604.bmp]]

It can be seen from above that T3=7.

You can continue developing more complicated cases as needed. The goal of this process is to understand how the problem works, and to begin to think about how to set up the recurrence relation.

4. Write the Recurrence relation : 
	Think about how the cases are related to each other, an equation that shows T(i) in relation to T(i-1) T(i - 2) etc etc ... while K=(i - ) >= 0
with n = 1, simple, move disk once and you are done
with n = 2, smaller disk had to be moved before larger disk can be moved, then, smaller disk placed on larger disk to complete puzzle
with n = 3, smaller disks had to be moved before largest disk, smaller disks were placed on larger disk to complete puzzle

Pattern Recognition -> have to move smaller disks, then move largest disk, then move smaller disks back on to the large disk to complete puzzle

In Terms of 'n' : 
T(n-1) moves to move smaller disks away from large disk
1 move to move large disk onto new peg
T(n-1) moves to move smaller disk back on top of the large disk

$$
T(n) = 2 * T( n - 1 ) + 1
$$
This is an example of **Linear Recurrence relation**, can be put into a closed form, using techniques 