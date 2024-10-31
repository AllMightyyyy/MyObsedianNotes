---
tags: [DataStructures, Algorithms, Analysis, BigO, AsymptoticAnalysis, Recursion, MasterTheorem]
---

#### Objective : 
* Explain the importance of analysis of algorithms, their notations, algorithms, relationships, and solving as many problems as possible.
* First understand the basic elements of algorithms, importance of algorithm analysis, then slowly move toward the other topics.
* We should be able to find the complexity of any given problem (especially recursive functions).

1. **Variables**
    * Hold data
    * **Tags:** [#Variables](#Variables)

2. **Data Types**
    * Set of data with predefined values (Integer, Double, Char, String, etc.)
    * There are 2 types of data types:
        * System-defined (also called "Primitive" data types)
        * User-defined data types
    * **Tags:** [#DataTypes](#DataTypes)

#### System defined data types (Primitive data types)
* Provided by Java are: int, float, char, double, bool, etc.
* **Tags:** [#PrimitiveDataTypes](#PrimitiveDataTypes)

#### User defined data types
* Example: Creating an object in Java made up of multiple primitive data types.
* **Tags:** [#UserDefinedDataTypes](#UserDefinedDataTypes)

#### Data structures : 
* A way to store and organize data in a computer so that it can be used efficiently.
* General data structures include "Arrays", "Files", "Linked Lists", "Stacks", "Queues", "Trees", "Graphs", and so on.
* Classified into 2 types:
    * Linear data structures -> Elements accessed in sequential order, but not compulsory to store all elements sequentially (e.g., Linked Lists, Stacks, and Queues).
    * Non-linear data structures -> Elements of this data structure are stored/accessed in a non-linear order (e.g., Trees and Graphs).
* **Tags:** [#DataStructures](#DataStructures), [#LinearDataStructures](#LinearDataStructures), [#NonLinearDataStructures](#NonLinearDataStructures)

#### ADT
**ADT** (Abstract Data Type) -> Data Type + method implementations that can be used on these data types.  
ADT consists of 2 parts:
1. Declaration of data
2. Declaration of operations  
Commonly used ADTs -> Linked Lists, Stacks, Queues, Priority Queues, Binary Trees, Dictionaries, Disjoint Sets (Union and Find), Hash Tables, Graphs, and many others.
* **Tags:** [#ADT](#ADT)

**Example :**
Stacks -> use LIFO (Last-in-First-out) mechanism while storing data in data structures.  
The last element inserted into the stack is the first element that gets deleted.  
Common operations are -> creating the stack, pushing element onto the stack, popping an element from the stack, finding the current top of the stack, finding the number of elements in the stack, etc.
* **Tags:** [#Stacks](#Stacks), [#LIFO](#LIFO), [#StackOperations](#StackOperations)

#### What is an Algorithm :
* An algorithm is the step-by-step unambiguous instructions to solve a given problem.
* **Tags:** [#Algorithm](#Algorithm)

#### How to judge an algorithm :
* Correctness -> Does the algorithm give a solution to the problem in a finite number of steps.
* Efficiency -> How much resources (memory & time) does it take to execute.
* **Tags:** [#AlgorithmJudgment](#AlgorithmJudgment), [#Correctness](#Correctness), [#Efficiency](#Efficiency)

#### Why the analysis of algorithms :
* Algorithm analysis helps us to determine which algorithm is most efficient in terms of time and space consumed.
* **Tags:** [#AlgorithmAnalysis](#AlgorithmAnalysis), [#Efficiency](#Efficiency)

#### Running time analysis : 
* The process of determining how processing time increases as the size of the problem (input size) increases.
    * Input size is the number of elements in the input, and depending on the problem type, the input may be of different types. The following are the common types of inputs:
        * Size of an array
        * Polynomial degree
        * Number of elements in a matrix
        * Number of bits in the binary representation of the input
        * Vertices and edges in a graph.
* **Tags:** [#RunningTimeAnalysis](#RunningTimeAnalysis), [#InputSize](#InputSize)

#### How to compare algorithms :
    
**Execution times?** Not a good measure as execution times are specific to a particular computer.

**Number of statements executed?** Not a good measure, since the number of statements varies with the programming language as well as the style of the individual programmer.

*Ideal solution?* Let us assume that we express the running time of a given algorithm as a function of the input size n (i.e., f(n)) and compare these different functions corresponding to running times. This kind of comparison is independent of machine time, programming style, etc.
* **Tags:** [#ComparingAlgorithms](#ComparingAlgorithms), [#ExecutionTime](#ExecutionTime), [#StatementCount](#StatementCount), [#FunctionOfInputSize](#FunctionOfInputSize)

#### Rate of Growth :
* The rate at which the running time increases as a function of input.
$$
n^4 + 2n^2 + 100n + 500 ≈ n^4
$$
Why is the equation above right? Because we can ignore the other orders since when the "n" gets bigger, n^4 dominates.
![[Pasted image 20241027201655.png]]
* **Tags:** [#RateOfGrowth](#RateOfGrowth), [#DominantTerm](#DominantTerm)

#### Types of Analysis :
* Worst case -> Defines the input for which the algorithm takes a long time (slowest time to complete), input is the one for which the algorithm runs the slowest.
* Best case -> Defines the input for which the algorithm takes the least time (fastest time to complete), input is the one for which the algorithm runs the fastest.
* Average case -> Provides a prediction about the running time of the algorithm. Run the algorithm many times, using many different inputs that come from some distribution that generates these inputs, compute the total running time (by adding the individual times), and divide by the number of trials. Assumes that the input is random.
* **Tags:** [#TypesOfAnalysis](#TypesOfAnalysis), [#WorstCase](#WorstCase), [#BestCase](#BestCase), [#AverageCase](#AverageCase)

#### Big-O Notation [ Upper Bounding Function ] :
    
This notation gives the tight upper bound of the given function. Generally, it is represented as f(n) = O(g(n)). That means, at larger values of n, the upper bound of f(n) is g(n). For example, if f(n) = n⁴ + 100n² + 10n + 50 is the given algorithm, then n⁴ is g(n). That means g(n) gives the maximum rate of growth for f(n) at larger values of n.  
![[Pasted image 20241027202429.png]]  
O–notation defined as O(g(n)) = {f(n): there exist positive constants c and n₀ such that 0 ≤ f(n) ≤ cg(n) for all n > n₀}. g(n) is an asymptotic tight upper bound for f(n). Our objective is to give the smallest rate of growth g(n) which is greater than or equal to the given algorithms’ rate of growth f(n).
* **Tags:** [#BigONotation](#BigONotation), [#UpperBound](#UpperBound), [#AsymptoticAnalysis](#AsymptoticAnalysis)

#### Big-O Visualization :
    
O(g(n)) is the set of functions with smaller or the same order of growth as g(n). For example, O(n²) includes O(1), O(n), O(n log n), etc.  
**Note:** Analyze the algorithms at larger values of n only. What this means is, below n₀ we do not care about the rate of growth.
* **Tags:** [#BigOViz](#BigOViz), [#OrderOfGrowth](#OrderOfGrowth)

#### Omega-Q Notation [Lower Bounding Function]
    
Similar to the O discussion, this notation gives the tighter lower bound of the given algorithm and we represent it as f(n) = Ω(g(n)). That means, at larger values of n, the tighter lower bound of f(n) is g(n). For example, if f(n) = 100n² + 10n + 50, g(n) is Ω(n²).  
![[Pasted image 20241027205125.png]]  
The Ω notation can be defined as Ω(g(n)) = {f(n): there exist positive constants c and n₀ such that 0 ≤ cg(n) ≤ f(n) for all n ≥ n₀}. g(n) is an asymptotic tight lower bound for f(n). Our objective is to give the largest rate of growth g(n) which is less than or equal to the given algorithm’s rate of growth f(n).
* **Tags:** [#OmegaNotation](#OmegaNotation), [#LowerBound](#LowerBound)

#### Theta-Θ Notation [Order Function]
![[Pasted image 20241027210234.png]]  
This notation decides whether the upper and lower bounds of a given function (algorithm) are the same. The average running time of an algorithm is always between the lower bound and the upper bound. If the upper bound (O) and lower bound (Ω) give the same result, then the Θ notation will also have the same rate of growth.  
As an example, let us assume that f(n) = 10n + n is the expression. Then, its tight upper bound g(n) is O(n). The rate of growth in the best case is g(n) = O(n).  
In this case, the rates of growth in the best case and worst case are the same. As a result, the average case will also be the same. For a given function (algorithm), if the rates of growth (bounds) for O and Ω are not the same, then the rate of growth for the Θ case may not be the same. In this case, we need to consider all possible time complexities and take the average of those (for example, for a quick sort average case, refer to the Sorting chapter).
    
Now consider the definition of Θ notation. It is defined as Θ(g(n)) = {f(n): there exist positive constants c₁, c₂, and n₀ such that 0 ≤ c₁g(n) ≤ f(n) ≤ c₂g(n) for all n ≥ n₀}. g(n) is an asymptotic tight bound for f(n). Θ(g(n)) is the set of functions with the same order of growth as g(n).
* **Tags:** [#ThetaNotation](#ThetaNotation), [#OrderFunction](#OrderFunction), [#AsymptoticAnalysis](#AsymptoticAnalysis)

#### Important Notes
    
For analysis (best case, worst case, and average), we try to give the upper bound (O) and lower bound (Ω) and average running time (Θ). From the above examples, it should also be clear that, for a given function (algorithm), getting the upper bound (O) and lower bound (Ω) and average running time (Θ) may not always be possible. For example, if we are discussing the best case of an algorithm, we try to give the upper bound (O) and lower bound (Ω) and average running time (Θ).  
In the remaining chapters, we generally focus on the upper bound (O) because knowing the lower bound (Ω) of an algorithm is of no practical importance, and we use the Θ notation if the upper bound (O) and lower bound (Ω) are the same.
* **Tags:** [#ImportantNotes](#ImportantNotes), [#BigONotation](#BigONotation), [#OmegaNotation](#OmegaNotation), [#ThetaNotation](#ThetaNotation)

#### Why is it called Asymptotic Analysis
    
From the discussion above (for all three notations: worst case, best case, and average case), we can easily understand that, in every case for a given function f(n), we are trying to find another function g(n) which approximates f(n) at higher values of n. That means g(n) is also a curve which approximates f(n) at higher values of n.  
In mathematics, we call such a curve an asymptotic curve. In other terms, g(n) is the asymptotic curve for f(n). For this reason, we call algorithm analysis asymptotic analysis.
* **Tags:** [#AsymptoticAnalysis](#AsymptoticAnalysis), [#GrowthFunctions](#GrowthFunctions)

#### Guidelines for Asymptotic Analysis
    
There are some general rules to help us determine the running time of an algorithm.
1. **Loops:** The running time of a loop is, at most, the running time of the statements inside the loop (including tests) multiplied by the number of iterations.
    ```java
    for (i=1 ; i <=n ; i++ )
        m = m + 2; // constant time, c
    ```
    $$
    Total Time = c * n = c n = O(n)
    $$
    with "c" constant
    
2. **Nest Loops:** Analyze from the inside out. Total running time is the product of the sizes of all the loops.
    ```java
    // Outer loop execute n times
    for (i=1 ; i <= n ; i++) {
        // Inner loop executes n times
        for (j=1 ; j <= n ; j++)
            k = k + 1; // constant time
    }
    ```
    $$
    Total Time = c * n * n = c n^2 = O(n^2)
    $$
    
3. **Consecutive Statements:** Add the time complexities of each statement.
    ```java
    x = x + 1; // constant time
    // executes n times
    for (i=1 ; i <= n ; i++)
        m = m + 2; // constant time
    // outer loop executes n times
    for (i=1 ; i <= n ; i++)
        // Inner loop executed n times
        for (j=1 ; j <= n ; j++) 
            k = k + 1; // constant time
    }
    ```
    $$
    Total Time = c0 + c1n + c2n^2 = O(n^2)
    $$
    
4. **If-Then-Else Statements:** Worst-case running time: the test, plus either the 'then' part or the 'else' part (whichever is larger).
    ```java
    // test: constant
    if (length() == 0) {
        return false; // then part: constant
    }
    else { // else part : (constant + constant) * n
        for (int n = 0; n < length(); n++) {
            // another if: constant + constant (no else part)
            if(!list[n].equals(otherList.list[n]))
                // constant
                return false;
        }
    }
    ```
    $$
    Total Time = c0 + c1 + (c2 + c3) * n = O(n)
    $$
    
5. **Logarithmic Complexity:** An algorithm is O(log n) if it takes a constant time to cut the problem size by a fraction (usually by ½).  
    *Example:*
    ```java
    for (i = 1; i <= n;)
        i = i * 2;
    ```
    If we observe carefully, the value of i is doubling every time. Initially i = 1, in the next step i = 2, and in subsequent steps i = 4, 8, and so on. Let us assume that the loop is executing some k times. At the kth step, 2^k = n, and at the (k + 1)th step, we come out of the loop. Taking logarithm on both sides gives:
    $$
    \log(2^k) = \log(n)
    $$
    $$
    k \cdot \log(2) = \log(n)
    $$
    $$
    k = \log(n)
    $$
    Total Time = O(log(n))
    ```
    
    Note: Similarly, for the case below, the worst-case rate of growth is O(log n). The same discussion holds good for the decreasing sequence as well.
    ```java
    for (i = n; i >= 1; )
        i = i / 2;
    ```
    
    **Another example**: *binary search* (finding a word in a dictionary of n pages)
    • Look at the center point in the dictionary  
    • Is the word towards the left or right of the center?  
    • Repeat the process with the left or right part of the dictionary until the word is found.
    * **Tags:** [#LogarithmicComplexity](#LogarithmicComplexity), [#BinarySearch](#BinarySearch)

#### Simplifying properties of asymptotic notations
* Transitivity: f(n) = Θ(g(n)) and g(n) = Θ(h(n)) ⇒ f(n) = Θ(h(n)). Valid for O and Ω as well.
* Reflexivity: f(n) = Θ(f(n)). Valid for O and Ω.
* Symmetry: f(n) = Θ(g(n)) if and only if g(n) = Θ(f(n)).
* Transpose symmetry: f(n) = O(g(n)) if and only if g(n) = Ω(f(n)).
* If f(n) is in O(kg(n)) for any constant k > 0, then f(n) is in O(g(n)).
* If f₁(n) is in O(g₁(n)) and f₂(n) is in O(g₂(n)), then (f₁ + f₂)(n) is in O(max(g₁(n), g₂(n))).
* If f₁(n) is in O(g₁(n)) and f₂(n) is in O(g₂(n)), then f₁(n) * f₂(n) is in O(g₁(n) * g₂(n))).
* **Tags:** [#SimplifyingProperties](#SimplifyingProperties), [#Transitivity](#Transitivity), [#Reflexivity](#Reflexivity), [#Symmetry](#Symmetry), [#TransposeSymmetry](#TransposeSymmetry), [#BigONotation](#BigONotation)

#### Commonly used Logarithms and Summations
![[Pasted image 20241027225724.png]]
* **Tags:** [#Logarithms](#Logarithms), [#Summations](#Summations)

#### Arithmetic series 
![[Pasted image 20241027225753.png]]
* **Tags:** [#ArithmeticSeries](#ArithmeticSeries)

#### Geometric series
![[Pasted image 20241027225829.png]]
* **Tags:** [#GeometricSeries](#GeometricSeries)

#### Harmonic Series
![[Pasted image 20241027225855.png]]
* **Tags:** [#HarmonicSeries](#HarmonicSeries)

#### Other important formulas 
![[Pasted image 20241027225932.png]]
* **Tags:** [#ImportantFormulas](#ImportantFormulas)

#### Master Theorem for Divide and Conquer Recurrences
Divide and conquer algorithms divide the problem into sub-problems, each of which is part of the original problem, and then perform some additional work to compute the final answer.  
Example -> A merge sort algorithm operates on 2 sub-problems, each of which is half the size of the original, and then performs O(n) additional work for merging. This gives the running time the equation: 
$$
T(n) = 2 \cdot T(n / 2) + O(n)
$$
The following theorem can be used to determine the running time of divide and conquer algorithms. For a given program (algorithm), first we try to find the recurrence relation for the problem.  
If the recurrence is of the below form, then we can directly give the answer without fully solving it. If the recurrence is of the form 
$$
T(n) = a \cdot T(n/b) + \Theta(n^k \cdot \log^p(n))
$$
where: `a >= 1, b > 1, k >= 0, and p is a real number`, then:
![[Pasted image 20241027233202.png]]
* **Tags:** [#MasterTheorem](#MasterTheorem), [#DivideAndConquer](#DivideAndConquer), [#RecurrenceRelation](#RecurrenceRelation)

