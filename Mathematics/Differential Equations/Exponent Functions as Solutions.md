In differential equations, exponential functions arise naturally due to their nature of self-replicating under differentiation, which means their rate of change is proportional to the function itself

Let's consider a simple linear differential equation with constant coefficients :
$$
	(dy/dt) = ky
$$
where y(t) is the function of time, and k is a constant. Solution to this equation is : 
$$
 y(t) = Ce^{kt}
$$
where C is a constant determined by initial conditions. 
solution can be verified like this :
$$
dy(t) / dt = d(Ce^{kt})/dt = ky(t)
$$
#### Difference Equations: Analogous Exponential Behavior
Difference equations, like differencial equations, often involve linear relationships with constant coefficients, but in a discrete rather than continuous setting. Consider a simple linear homogeneous recurrence relation :
$$
x_{n} = a x_{n-1}
$$
to see why 
$$
x_n = r^n 
$$
might be a solution, substitute it into the recurrence relation :
$$
r^n = a r^{n-1}
$$
Dividing both sides by r^(n-1) assuming r is not null
gives us : r = a
This shows that 
$$
x_n = r^n 
$$
is indeed a solution when r = a. More generally, for a second order linear homogeneous recurrence relation like : 
$$
x_n = bx_{n-1} + cx_{n-2}
$$
assume x_n = r^n and substitute :
$$
r^n = b r^{n-1} + cr^{n-2}
$$
dividing both sides by r_{n-2} assuming r is not null gives us : 
$$
r^2 = br + c
$$
thus by finding r solution of this equation we would also be solving the recurrence. 
