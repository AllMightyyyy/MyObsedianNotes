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
