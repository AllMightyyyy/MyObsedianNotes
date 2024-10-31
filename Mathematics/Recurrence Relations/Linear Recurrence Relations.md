---
tags: [Mathematics, RecurrenceRelations, LinearRecurrence, Definitions, Examples]
---

# Recurrence Relations

**Definition:**  
An equation that relates a term in a sequence or a multidimensional array to previous terms using recursion.

**Linear** refers to the fact that previous terms are arranged as 1st-degree polynomials in the recurrence relation.

#### **Definition:**
A **linear recurrence relation** is an equation that defines the \( n^{th} \) term in a sequence in terms of the \( k \) previous terms in the sequence. The recurrence relation is in the form:
$$
x_{n} = c_{1}x_{n-1} + c_{2}x_{n-2} + \dots + c_{k}x_{n-k}
$$
where \( c_i \) is a constant coefficient.

##### **Exercise Example:**
Solve the recurrence relation:
$$
x_{1} = 3, \quad x_{n} = 3 x_{n-1}
$$
Find the formula that solves this problem for us:
![[Pasted image 20241028093746.png]]

Thus, the solution for all positive integers \( n \) is:
$$
x_{n} = 3^n
$$

#### **Geometrical Characteristic Equation:**
Let's say we have a recurrence:
$$
x_n = c_1 x_{n-1} + c_2 x_{n-2} + \dots + c_k x_{n-k}
$$
and a solution:
$$
x_n = a r^n
$$
Plugging it in, we get:
$$
a r^n = c_1 a r^{n-1} + c_2 a r^{n-2} + \dots + c_k a r^{n-k}
$$
Since \( a \) and \( r \) are non-null, we can divide by \( a r^{n-k} \) and obtain:
$$
r^k = c_1 r^{k-1} + c_2 r^{k-2} + \dots + c_k
$$
Moving the terms over, we get:
$$
r^k - c_1 r^{k-1} - c_2 r^{k-2} - \dots - c_k = 0
$$
which is a polynomial in \( r \), so the solution satisfies the recurrence only if \( r \) is a root of this polynomial. This polynomial is called the **Characteristic Polynomial** of the recurrence.

Also, note that if two geometric series satisfy a recurrence, the sum of them also satisfies the recurrence. Then, we can find the following method for solving recurrences:
- Find the characteristic polynomial.
- Find the roots \( r_1, r_2, \dots, r_k \) of the characteristic polynomial.
- Assuming no multiple roots, the closed-form expression will look like:
$$
x_n = a_1 (r_1)^n + a_2 (r_2)^n + \dots + a_k (r_k)^n
$$
for some constants \( a_i \).
- Use the initial values to find the values of the \( a_i \)'s.
