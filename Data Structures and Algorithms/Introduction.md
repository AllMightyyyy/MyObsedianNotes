#### Objective : 
* explain importance of analysis of algorithms, their notations, algorithms, relationships, and solving as many problems as possible
* first understand, basic elements of algorithms, importance of algo analysis, then slowly move toward the other topics
* we should be able to find complexity of any given problem ( especial recursive functions )

1. **Variables**
	* hold data
2. **Data Types
	* set of data with predefined values ( Integer, Double, Char, String, etc. )
	* There are 2 types of data types :
		* System-defined ( also called "Primitive" data types )
		* User-defined data types
#### System defined data types ( Primitive data types )
* provided by Java are : int, float, char, double, bool, etc.
#### User defined data types
	example creating an object in java made up of multiple primitive data types

#### Data structures : 
* is a way to store and organize data in a computer so that it can be used efficiently 
* General data structures include "Arrays", "Files", "Linked Lists", "Stacks", "Queues", "Trees", "Graphs", and so on
* Classified into 2 types :
	* Linear data structures -> elements accessed in sequential order , but not compulsory to store all elements sequentially ( ex. Linked Lists, Stacks and Queues )
	* Non linear data structures -> elements of this data structure are stored/accessed in a non linear order. ( ex. Trees and graphs )

#### ADT
**ADT** ( Abstract data type ) -> Data Type + method implementations that can be used on these data types 
ADT -> consists of 2 parts :
1. Declaration of data
2. Declaration of operations
Common used ADTs -> Linked Lists, Stacks, Queues, Priority Queues, Binary Trees, Dictionaries, Disjoint Sets ( Union and Find ), Hash Tables, Graphs, and many others.

**Example :**
Stacks -> use LIFO -> Last-in-First-out mechanism while storing data in data structures
The last element inserted into the stack is the first element that gets deleted
Common operations are -> creating the stack, pushing element onto the task, popping an element from stack, finding current top of the stack, finding number of elements in the stack, etc

#### What is an Algorithm :
* An algorithm is the step-by-step unambiguous instructions to solve a given problem.
#### How to judge an algorithm :
* correctness -> does algo give solution to the problem in a finite number of steps
* efficiency -> how much resources ( memory & time ) does it take to execute

#### Why the analysis of algorithms :
* Algorithm analysis helps us to determine which algorithm is
most efficient in terms of time and space consumed.

#### Running time analysis : 
* the process of determining how processing time increases as the size of the problem (input size) increases.
	* input size is the number of elements in the input, and depending on the problem type, the input may be of different types. The following are the common types of inputs :
		* Size of an array
		* Polynomial degree
		* Number of elements in a matrix
		* Number of bits in the binary representation of the input
		* Vertices and edges in a graph.

#### How to compare algorithms :

**Execution times?** Not a good measure as execution times are specific to a particular computer.

**Number of statements executed?** Not a good measure, since the number of statements varies with the programming language as well as the style of the individual programmer.

*Ideal solution?* Let us assume that we express the running time of a given algorithm as a function of the input size n (i.e., f(n)) and compare these different functions corresponding to running times. This kind of comparison is independent of machine time, programming style, etc.

#### Rate of Growth :
* The rate at which the running time increases as a function of input
$$
n^4 + 2n^2 + 100n + 500 ≈ n^4
$$
			why is the equation above right ? because we can ignore the other orders since when the "n" gets bigger, n^4 dominates
![[Pasted image 20241027201655.png]]

#### Types of Analysis :
* Worst case -> Defines the input for which the algorithm takes a long time (slowest time to complete), Input is the one for which the algorithm runs the slowest.
* Best case -> Defines the input for which the algorithm takes the least time (fastest time to complete), Input is the one for which the algorithm runs the fastest.
* Average case -> Provides a prediction about the running time of the algorithm. Run the algorithm many times, using many different inputs that come from some distribution that generates these inputs, compute the total running time (by adding the individual times), and divide by the number of trials. Assumes that the input is random.

#### Big-O Notation [ Upper Bounding Function ] :

This notation gives the tight upper bound of the given function. Generally, it is represented as f(n)= O(g(n)). That means, at larger values of n, the upper bound of f(n) is g(n). For example, if f(n) = n4 + 100n2 + 10n + 50 is the given algorithm, then n4 is g(n). That means g(n) gives the maximum rate of growth for f(n) at larger values of n![[Pasted image 20241027202429.png]]
O–notation defined as O(g(n)) = {f(n): there exist positive constants c and n0 such that 0 ≤ f(n) ≤ cg(n) for all n > n0}. g(n) is an asymptotic tight upper bound for f(n). Our objective is to give the smallest rate of growth g(n) which is greater than or equal to the given algorithms’ rate of growth /(n).

#### Big-O Visualization :

O(g(n)) is the set of functions with smaller or the same order of growth as g(n). For example; O(n2) includes O(1), O(n), O(nlogn), etc.
**Note:** Analyze the algorithms at larger values of n only. What this means is, below n0 we do not care about the rate of growth.

#### Omega-Q Notation [Lower Bounding Function]

Similar to the O discussion, this notation gives the tighter lower bound of the given algorithm and we represent it as f(n) = Ω(g(n)). That means, at larger values of n, the tighter lower bound of f(n) is g(n). For example, if f(n) = 100n^2 + 10n + 50, g(n) is Ω(n^2).![[Pasted image 20241027205125.png]]
The Ω notation can be defined as Ω(g(n)) = {f(n): there exist positive constants c and n0 such that 0 ≤ cg(n) ≤ f(n) for all n ≥ n0}. g(n) is an asymptotic tight lower bound for f(n). Our objective is to give the largest rate of growth g(n) which is less than or equal to the given algorithm’s rate of growth f(n).

#### Theta-Θ Notation [Order Function]
![[Pasted image 20241027210234.png]]
This notation decides whether the upper and lower bounds of a given function (algorithm) are the same. The average running time of an algorithm is always between the lower bound and the upper bound. If the upper bound (O) and lower bound (Ω) give the same result, then the Θ notation will also have the same rate of growth.
As an example, let us assume that f(n) = 10n + n is the expression. Then, its tight upper bound g(n) is O(n). The rate of growth in the best case is g(n) = O(n).
In this case, the rates of growth in the best case and worst case are the same. As a result, the average case will also be the same. For a given function (algorithm), if the rates of growth (bounds) for O and Ω are not the same, then the rate of growth for the Θ case may not be the same. In this case, we need to consider all possible time complexities and take the average of those (for example, for a quick sort average case, refer to the Sorting chapter).

Now consider the definition of Θ notation. It is defined as Θ(g(n)) = {f(n): there exist positive constants c1,c2 and n0 such that 0 ≤ c1g(n) ≤ f(n) ≤ c2g(n) for all n ≥ n0}. g(n) is an asymptotic tight bound for f(n). Θ(g(n)) is the set of functions with the same order of growth as g(n)
#### Important Notes

For analysis (best case, worst case and average), we try to give the upper bound (O) and lower bound (Ω) and average running time (Θ). From the above examples, it should also be clear that, for a given function (algorithm), getting the upper bound (O) and lower bound (Ω) and average running time (Θ) may not always be possible. For example, if we are discussing the best case of an algorithm, we try to give the upper bound (O) and lower bound (Ω) and average running time (Θ).
In the remaining chapters, we generally focus on the upper bound (O) because knowing the lower bound (Ω) of an algorithm is of no practical importance, and we use the Θ notation if the upper bound (O) and lower bound (Ω) are the same.
#### Why is it called Asymptotic Analysis

From the discussion above (for all three notations: worst case, best case, and average case), we can easily understand that, in every case for a given function f(n) we are trying to find another function g(n) which approximates f(n) at higher values of n. That means g(n) is also a curve which approximates f(n) at higher values of n.
In mathematics we call such a curve an asymptotic curve. In other terms, g(n) is the asymptotic curve for f(n). For this reason, we call algorithm analysis asymptotic analysis

#### Guidelines for Asymptotic Analysis

There are some general rules to help us determine the running time of an algorithm.
1. Loops: The running time of a loop is, at most, the running time of the statements
inside the loop (including tests) multiplied by the number of iterations.
```java
for (i=1 ; i <=n ; i++ )
	m = m + 2; // constant time, c
```
$$
Total Time = c * n = c n = O(n)
$$
with "c" constant

2. Nest Loops : Analyze from the inside out. Total running time is the product of the
sizes of all the loops.
```java
// Outer loop execute n times
for ( i=1 ; i <= n ; i++) {
	// Inner loop executes n times
	for ( j=1 ; j <= n ; j++)
		k = k + 1; // constant time
}
```
$$
Total Time = c * n * n = c n^2 = O(n^2)
$$
3. Consecutive Statements : Add the time complexities of each statement

```java
x = x + 1; // constant time
// executes n times
for ( i=1 ; i <= n ; i++)
	m = m + 2; // constant time
// outer loop executes n times
for ( i=1 ; i <= n ; i++)
	// Inner loop executed n times
	for ( j=1 ; j <= n ; j++) 
		k = k + 1; // constant time
}
```
$$
Total Time = c0 + c1n + c2n^2 = O(n^2)
$$
4. If-Then-Else Statements : Worst-case running time: the test, plus either the 'then' part or the 'else' part ( whichever is larger )
```java
// test: constant
if (length() == 0) {
	return false; // then part: constant
}
else { // else part : (constant + constant) * n
	for (int n = 0; n < length(); n++) {
		//another if: constant + constant (no else part)
		if(!list[n].equals(otherList.list[n]))
			// constant
			return false;
	}
}
```
$$
Total Time = c0 + c1 + (c2 + c3) * n = O(n)
$$
5. Logarithmic Complexity : An algorithm is O(logn) if it takes a constant time to cut
the problem size by a fraction (usually by ½). As an example let us consider the
following program: 
```java
for (i = 1; i<=n;)
	i = i * 2;
```
If we observe carefully, the value of i is doubling every time. Initially i = 1, in next step i
= 2, and in subsequent steps i = 4,8 and so on. Let us assume that the loop is executing
some k times. At kth step 2k = n, and at (k + 1)th step we come out of the loop. Taking
logarithm on both sides, gives : 
$$
log(2^k) = log(n)
$$
$$
k * log(2) = log(n)
$$
$$
k = log(n)
$$
Total Time = O(log(n))

Note: Similarly, for the case below, the worst case rate of growth is O(logn). The same
discussion holds good for the decreasing sequence as well.
```java
for (i=n; i >= 1)
	i = i / 2;
```

**Another example**: *binary search* (finding a word in a dictionary of n pages)
	• Look at the center point in the dictionary
	• Is the word towards the left or right of center?
	• Repeat the process with the left or right part of the dictionary until the word is found.

#### Simplifying properties of asymptotic notations
	• Transitivity: f(n) = Θ(g(n)) and g(n) = Θ(h(n)) ⇒ f(n) = Θ(h(n)). Valid for O and Ω as well.
	• Reflexivity: f(n) = Θ(f(n)). Valid for O and Ω.
	• Symmetry: f(n) = Θ(g(n)) if and only if g(n) = Θ(f(n)).
	• Transpose symmetry: f(n) = O(g(n)) if and only if g(n) = Ω(f(n)).
	• If f(n) is in O(kg(n)) for any constant k > 0, then f(n) is in O(g(n)).
	• If f1(n) is in O(g1(n)) and f2(n) is in O(g2(n)), then (f1 + f2)(n) is in   O(max(g1(n)),(g1(n))).
	• If f1(n) is in O(g1(n)) and f2(n) is in O(g2(n)) then f1(n) f2(n) is in O(g1(n) g1(n)).

#### Commonly used Logarithms and Summations
![[Pasted image 20241027225724.png]]
#### Arithmetic series 
![[Pasted image 20241027225753.png]]

#### Geometric series
![[Pasted image 20241027225829.png]]

#### Harmonic Series
![[Pasted image 20241027225855.png]]

#### Other important formulas 
![[Pasted image 20241027225932.png]]


#### Master Theorem for Divide and Conquer Recurrences
Divide and conquer algorithms divide the problem into sub-problems, each of which is part of the original problem, and then perform some additional work to compute the final answer, and then perform some additional work to compute the final answer. 
Example -> a merge sort algorithm, operates 2 sub-problems, each of which is half the size of the original, and then performs O(n) additional work for merging. This gives the running time the equation : 
$$
T(n) = 2 * T(n / 2) + O(n)
$$
The following theorem can be used to determine the running time of divide and conquer algorithms. For a given program ( algorithm ), first we try to find the recurrence relation for the problem. 
If the recurrence is of the below form, then we can directly give the answer without fully solving it. If the recurrence is of the form 
$$
T(n) = a * T(n/b) + Teta(n^k * log^p(n))
$$
where : ` a >= 1, b > 1, k >= 0 and p is a real number, then :
![[Pasted image 20241027233202.png]]
