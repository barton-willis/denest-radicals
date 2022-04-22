# denest-radicals
 Maxima CAS code for denesting radicals. 
 
 For a constant expression involving nested rational powers, this code attempts to find a unnested representation. Unlike Maxima's sqrtdenest function that handles nested square roots, this function attempts to denest more general expressions.

 When this code fails to denest a radical, it does not mean that the radical cannot be denested. Also, the sqrtdenest function handles some cases that this code cannot.
