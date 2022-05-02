# Maxima CAS code for denesting radicals

 Simple and compact Maxima CAS code for denesting radicals (`denest`). 
 
 For a comprehensive Maxima package for denesting, see https://github.com/gschintgen/raddenest. 

 For a constant expression involving nested rational powers, the `denest` package attempts to find an un-nested representation of the algebraic number. Unlike Maxima's `sqrtdenest` function that handles nested square roots, this function attempts to denest more general expressions, including expressions involving cube roots; for example
 ~~~
(%i8) 2028*(2*3^(5/2)*%i+35)^(1/3)-(2*3^(5/2)*%i+35)^(2/3)*(8*3^(7/2)*%i-420)+10140$

(%i9) denest(%);
(%o9) 24336

(%i10)	denest(sqrt(5^(1/3)-4^(1/3)));
(%o10)	(-4^(1/3)*20^(2/3)+4*20^(1/3)+2*4^(2/3))/12
 ~~~
  
There is a great deal of literature with algorithms for denesting various classes of radicals, but the `denest` package uses a simple approach:

1. find a polynomial equation with integer coefficients with one solution being the algebraic number, 

2. solve the polynomial equation, 

3. remove the solutions that do not equal the algebraic number, 

4. when successful, return a denested algebraic number. 

When a polynomial has denested roots, Maxima sometimes expresses them as nested algebraic numbers. For such cases, this package fails to denest the number. Thus when this code fails to denest a radical, it does _not_ mean that the radical cannot be denested--it only means that the method isn't sufficiently general to handle the particular case.

For example, here is a polynomial that has roots that are surds, but Maxima's solve function finds nested radicals for the roots:
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
(%o5)	[x=-sqrt(3)-sqrt(2)+5,x=-sqrt(3)+sqrt(2)+5,x=sqrt(3)-sqrt(2)+5,x=sqrt(3)+sqrt(2)+5]
~~~

# Installation and usage

Copy the files `my_denest.mac` and `rtest_denest.mac` into a directory that is in the Maxima list `file_search_maxima.`  To load the package, enter `load(my_denest).` The only user level function is `denest.`  Example:
~~~
(%i13)	load(my_denest)$
(%i14)	denest((6*5^(2/3)+12*5^(1/3)+13)^(1/3));
(%o14)	5^(1/3)+2
~~~
For expressions that don't denest or don't contain radicals, `denest` returns the input unchanged; for example
~~~
(%i15)	denest(cos(x) + 1);
(%o15)	cos(x)+1
~~~

To run the test suite, at a Maxima prompt, enter `batch(rtest_denest,'test)`. These
tests include most of testsuite for the `raddenest` package. As of 2 May 2022, 35 out of 102 tests fail. 
