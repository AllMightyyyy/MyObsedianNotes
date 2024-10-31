---
tags: [Mathematics, RecurrenceRelations, Applications, TowerOfHanoi, LinearRecurrence, Definitions]
---

# Recurrence Relations

#### **Definition:**
* An equation that uses recursion to relate terms in a sequence or elements in an array.
* A way to define a sequence or array in terms of itself.

#### **Applications:**
* **Number Theory:** Fibonacci sequence.
* **Combinatorics:** Distribution of objects into bins.
* **Calculus:** Euler's method.
* **Etc.**

#### **Why Use Recurrence?**
* Used when an exhaustive approach to problem-solving is too arduous to be practical.
* Not ideal, but can be much more efficient than the alternative of exhaustive casework.

#### **Setting Up a Recurrence Relation**
* **Complicated Problem + Recursion =** Iterative process based on simpler versions of the problem.

**Example of Application:** *Tower of Hanoi Puzzle*
![[Pasted image 20241028090738.png]]

The rules of the puzzle are as follows:
- The puzzle begins with all disks placed on one of the pegs. They are placed in order of largest to smallest, bottom to top.
- The goal of the puzzle is to move all of the disks onto another peg.
- Only one disk may be moved at a time, and disks are always placed onto pegs.
- Disks may only be moved onto an empty peg or onto a larger disk.

Let \( T_n \) be defined as the minimum number of moves needed to solve a puzzle with \( n \) disks.

##### **Identifying a Candidate Problem to Solve with a Recurrence Relation:**
- The problem can be reduced to simpler cases. The Tower of Hanoi puzzle can be simplified by removing some of the disks.
- There is a numerical value, \( n \), to identify each case. For the Tower of Hanoi puzzle, this numerical value is the number of disks.
- The problem increases in complexity as the numerical identifier increases. As the number of disks in the Tower of Hanoi problem increases, it becomes more difficult to find a solution.

#### **Steps to Solve the Puzzle:**
1. **Define a Base Case:** Simple version = 1 disk
   ![[Pasted image 20241028091056.png]]
   One disk: \( n = 1 \) -> \( T_1 = 1 \) (it will take only 1 move to move one disk to any other peg).

2. **Solution When \( n = 2 \):**
   ![[iqxa5kox.bmp]]
   In this case, \( T_2 = 3 \).

3. **When \( n = 3 \):**
   ![[dsd64604.bmp]]
   
   It can be seen from above that \( T_3 = 7 \).

   You can continue developing more complicated cases as needed. The goal of this process is to understand how the problem works and to begin to think about how to set up the recurrence relation.

4. **Write the Recurrence Relation:**
   Think about how the cases are related to each other, an equation that shows \( T(i) \) in relation to \( T(i-1) \), \( T(i-2) \), etc., while \( K = (i - ) \geq 0 \).

   - With \( n = 1 \), simple: move disk once and you are done.
   - With \( n = 2 \), smaller disk had to be moved before the larger disk can be moved, then the smaller disk placed on the larger disk to complete the puzzle.
   - With \( n = 3 \), smaller disks had to be moved before the largest disk; smaller disks were placed on the larger disk to complete the puzzle.

   **Pattern Recognition:** Have to move smaller disks, then move the largest disk, then move smaller disks back onto the large disk to complete the puzzle.

   **In Terms of 'n':**
   - \( T(n-1) \) moves to move smaller disks away from the large disk.
   - 1 move to move the large disk onto the new peg.
   - \( T(n-1) \) moves to move the smaller disks back on top of the large disk.

   $$
   T(n) = 2 \cdot T(n - 1) + 1
   $$
   This is an example of a **linear recurrence relation**, which can be put into a closed form using various techniques.
