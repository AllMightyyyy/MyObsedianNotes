Equation that relates a term in a sequence or a multidimensional array to previous terms using recursion. 

**Linear** refers to the fact that previous terms are arranged as 1st degree polynomial in the recurrence relation
#### Definition :
A **linear recurrence relation** is an equation that defines the n'th term in a sequence in terms of the k previous terms in the sequence. The recurrence relation is in the form:
$$
x_{\text{n}}​=c_{\text{1}}​x_{\text{n - 1}}​+c_{\text{2}}​x_{\text{n - 2}}​+⋯+c_{\text{k}}x_{\text{n - k}}​
$$
where c(i) is a constant coefficient

##### Exercise example :
Solve the recurrence relation : 
$$
x_{\text{1}} = 3, x_{\text{n}} = 3 x_{\text{n -1}}
$$
find the formula that solves this problem for us :
![[Pasted image 20241028093746.png]]
Thus, the solution for all positive integers n is :
$$
x_{\text{n}} = 3^n
$$

Geometrical characteristic equation :
let's us say we have a recurrence :
$$
x_n = c_1 x_{n-1} + c_2 x_{n-2} + ... + c_k x_{n-k}
$$
and a solution : 
$$
x_n = a r^n
$$
plugging it in, we get :
$$
ar^n = c_1 a r^{n-1} + c_2 a r^{n-2} + ... + c_k a r^{n-k}
$$
Since a, r non null, we can divide by 
$$
ar^{n-k}
$$
and we get :
$$
r^k = c_1 r^{k-1} + c_2 r^{k-2} + ... + c_k
$$
Moving the terms over, we get :
$$
r_k - c_1 r^{k-1} - c_2 r^{k-2} - ... - c_k = 0
$$
which is a polynomial in r, so the solution satisfies the recurrence only if r is a root of this polynomial. This polynomial is called the **Characteristic polynomial** of the recurrence
Also, note that if two geometric series satisfy a recurrence, the sum of them also satisfies the recurrence. Then, we can find the following method for solving recurrences
- Find the characteristic polynomial.
- Find the roots 
$$
r_1 , r_2, ... , r_k
$$
of the characteristic polynomial.
- Assuming no multiple roots, the closed-form expression will look like :
$$
x_n = a_1 (r_1)^n + a_2 (r_2)^n + ... + a_k (r_k)^n
$$
for some constant a's
- Use the initial values to find the values of the a's

##### Repeated Roots
A linear recurrence with repeated roots is linear recurrence of the form
$$
x_n = c_1 x_{n-1} + c_2 x_{n-2} + ... + c_k x_{n-k}
$$
where all the c's are constants, and whose characteristic polynomial, 
$$
r^k - c_1 r^{k-1} - c_2 r^{k-2} - ... - c_k
$$
may have repeated roots, that is, roots with multiplicity higher than 1
![[Pasted image 20241028220723.png]]
#### General Method of Solving Linear Recurrences with Repeated Roots
- Find the characteristic polynomial (degree k).
- Find all roots of the characteristic polynomial 
$$
r_1, r_2, ..., r_m
$$
where m is less than or equal to k . with multiplicities 
$$
l_1, l_2, ... , l_m 
$$
respectively
* The closed-form expression of the solution can be expressed as a linear combination of all the sequences of the form
$$
n^j r_i ^n
$$
where 0≤j≤li​−1 and 1≤i≤m.
* Use the initial values of to find the constants used as coefficients of the linear combination.

The following is an **example** of solving a recurrence with a 3rd3rd degree characteristic polynomial and one repeated root:
![[Pasted image 20241028223545.png]]
![[Pasted image 20241028224527.png]]


   