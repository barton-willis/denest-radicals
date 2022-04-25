# Denest radicals
 Maxima CAS code for denesting radicals. For another Maxima package for denesting, see https://github.com/gschintgen/raddenest.
 
 For a constant expression involving nested rational powers, this code attempts to find an un-nested representation. Unlike Maxima's `sqrtdenest` function that handles nested square roots, this function attempts to denest more general expressions, including expressions involving cube roots; for example
 ~~~
(%i8) 2028*(2*3^(5/2)*%i+35)^(1/3)-(2*3^(5/2)*%i+35)^(2/3)*(8*3^(7/2)*%i-420)+10140$

(%i9) denest(%);
(%o9) 24336
 ~~~
 For the square root case, this code calls `sqrtdenest.`
 
There is a great deal of literature with algorithms for denesting various classes of radicals, but this package uses a simple approach:

1. find a polynomial equation with integer coefficients with one solution being the radical.  

2. solve the polynomial equation, 

3. filter the spurious solutions, 

4. return a denested radical when successful. 

When there are denested solutions, Maxima's polynomial solver doesn't always find them. For such cases, this denesting method fails. When this code fails to denest a radical, it does not mean that the radical cannot be denested--it only means that the method isn't sufficiently general to handle the particular case.

Here is a polynomial that has solutions that are surds, but Maxima's solve function
finds nested radicals for the solutions:

~~~
(%i1)	p : x^4-20*x^3+140*x^2-400*x+376;
(p)	x^4-20*x^3+140*x^2-400*x+376

(%i2)	solve(p,x);
(%o2)	[x=5-sqrt(20-8*sqrt(6))/2,x=sqrt(20-8*sqrt(6))/2+5,x=5-sqrt(8*sqrt(6)+20)/2,x=sqrt(8*sqrt(6)+20)/2+5]
~~~

To find the solutions explicitly as surds, here is one approach:
~~~

(%i3)	factor(p,g^2-3);
(%o3)	(x^2-2*g*x-10*x+10*g+26)*(x^2+2*g*x-10*x-10*g+26)

(%i4)	subst(g=sqrt(3),%);
(%o4)	(x^2-2*sqrt(3)*x-10*x+10*sqrt(3)+26)*(x^2+2*sqrt(3)*x-10*x-10*sqrt(3)+26)

(%i5)	solve(%,x);
(%o5)	[x=-sqrt(3)\-sqrt(2)+5,x=-sqrt(3)+sqrt(2)+5,x=sqrt(3)-sqrt(2)+5,x=sqrt(3)+sqrt(2)+5]
~~~



 
