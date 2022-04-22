# Denest radicals
 Maxima CAS code for denesting radicals. For another Maxima package for denesting, see https://github.com/gschintgen/raddenest.
 
 For a constant expression involving nested rational powers, this code attempts to find a un-nested representation. Unlike Maxima's `sqrtdenest` function that handles nested square roots, this function attempts to denest more general expressions, including expressions involving cube roots; for example
 ~~~
(%i8) 2028*(2*3^(5/2)*%i+35)^(1/3)-(2*3^(5/2)*%i+35)^(2/3)*(8*3^(7/2)*%i-420)+10140$

(%i9) denest(%);
(%o9) 24336
 ~~~
 
There is a great deal of literature with algorithms for denesting various classes of radicals, but this package use a simple approach.
It finds a polynomial equation for the radical, solves the polynomial equation, and filters the spurious solutions, and returns a denested radical when successful. The method fails when Maxima's polynomial solver returns a nested radical when there is a non nested radical solution. Thus when this code fails to denest a radical, it does not mean that the radical cannot be denested--it only means that the method isn't sufficiently general to handle the particular case.

For the square root case, this code calls `sqrtdenest.`

 
