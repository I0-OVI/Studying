# Lambda Calculus

>From wikipedia: Lambda calculus is a formal system for expressing computation based on function abstraction and application using variable binding and substitution. 
It was introduced by Church and his doctoral student was Alan Turing.

### Lambda term
1. **Variable**: lower case letters, like x,y,a,b...
2. **Lambda abstraction**: $\lambda x. \; M$ is the function definition where x is the input of the function and M is the body of function. 
3. **Application**: $(M \,N)$  where both of them are lambda function and this means: we apply function M taking N as the input.
### Reduction operation
1. **alpha conversion**: renaming the bound variable in the expression,eg: $\lambda x. x \equiv \lambda y.y$
2. **beta reduction**: $\lambda x.\; M\, N$ replacing the bound variable with lambda term $N$ \
For example: $\lambda x. ( \lambda y.\; x+y)\: 3\:5$\
First, we find the x in the lambda term and substitute it by value of three. Then we get $\lambda y.\; 3+y \: 5$ and do the same operation to this expression. And finally we get 8 as the result.
*It is like we have function f(y)=y+3 and when y=5, we could get f(5)=5+3=8.*
### Beta normal form
Definition : a lambda term is in beta normal form if it contains no beta-reducible sub-terms.

For example: $\lambda x. ( \lambda y.\; x+y)\: 3$\
This expression still has a input needed to deal with. After we substitute x=3, we could get the beta normal form :$\lambda y.\; 3+y$

However, there are some expressions which don't have beta normal form:$ \lambda x.\:x\:x\;\lambda x.\:x \:x $. \
If we want to reduce this, we can first substitute the x= $\lambda x.\:x \:x$.
Then, the expression becomes to $\lambda x.\:x \:x \; \lambda x.\:x \:x$ which is a infinite loop.
### Church's numeral
We have projection function $proj^i _{j} $: Given tuple of i values, it returns the j-th value from the tuple.  For example, $proj^2 _{2}$ means to select the second value from the tuple of 2 values.
It is equivalent to the lambda term $\lambda xy. \; y$

Here is the church's numeral. \
$\text{Number } n \equiv \lambda f.\, \lambda x.\, \underbrace{f(f(\cdots f(x) \cdots))}_{n\ \text{times}}$ \
More detailed:\
1 is defined as $\lambda fx.\; f\:x$ \
2 is defined as $\lambda fx.\; f(f\:x)$
3 is defined as $\lambda fx.\; f(f(f\:x))$
4 ...
We could notice that projection function has some relationship between the church's numeral: the Church 0 is identical to $proj^2 _{2}$.\
*However, this is the special case. Church's numeral and projection are different system.*
### Successor
Now, we only have functions but there are no operation between functions. We define $SUCC \equiv \lambda x \: f\: y. \;f(x\: f \: y)$ \
And we take 1 as the input to see what happen to the function: \
*In the previous paragraph, we define 1 as $\lambda ab. \; a(b)$.*
Here is the whole process:
>$SUCC \equiv \lambda x \: f\: y. \;f(x\: f \: y)\;\; \lambda ab. \: a(b)$.
$SUCC \equiv \lambda y\: f.\;f(\lambda a b.\;a(b)\: f\: y)$
$SUCC \equiv \lambda y\:f.\; f(\lambda b.\;f(b)\: y)$
$SUCC \equiv \lambda y\:f.\;f(f(y))$ 
From the previous paragraph, we could find that the result is equivalent to 2.

Therefore, we proof that SUCC 1 equals to 2 and we could also proof that SUCC N equals to N+1 ,which means we find something abstract that matches the Peano axioms.

### Addition
After defining a successor, we could make an operation $ADD$. How could we find a lambda term so that $ADD\; A\: B \equiv A+B$.
An idea comes out: ADD is basically A times **SUCC** added to B times **SUCC**. So we could have a function so that $A+B \equiv\underbrace {SUCC(SUCC(\cdots SUCC(A)\cdots))}_{B \;times}$.\
With great attention, this is much like the **Church's numeral**: A integer number in Lambda Calculus take SUCC function as one of the inputs
> B SUCC  
*Suppose B is equal to 3*
*We have 3 SUCC*
$\lambda fx.\; f(f(f\:x))\; SUCC$
*Then:*
$\lambda x.SUCC(SUCC(SUCC\: x))$

Last step is to take A as the input of variable x. And we get the addition: $ADD \;A\: B \equiv \lambda AB. (B\; SUCC \; A)$
### Multiple
Now, it is easy for us to build it. Thinking this way: addition is to repeat **SUCC** function for B times based on A and multiple is to repeat **ADD** function for B times to A.
However, multiple is defined as: 
$MULT \equiv \lambda m n f x.\; m \: (n\: f)\: x$
*which is much different from the addition.*
Let's see how it work with the input 2 and 3:
>$MULT \; 2\: 3 \equiv \lambda f x.\; 2 \: (3 \: f) \: x$
$MULT \; 2 \: 3 \equiv \lambda f x . \; \lambda a b. a(a(b)) \; (3\: f)\; x$
$MULT \; 2 \: 3 \equiv \lambda f x . \;  (3\;f)(3\: f \: x)$
$MULT \; 2 \: 3 \equiv \lambda f x . \;  (3\;f)(f(f(f \:x)))$
*We denote $f(f(f \:x))$ as A*
$MULT \; 2 \: 3 \equiv \lambda f x . \;  3\;f \;A$
$MULT \; 2 \: 3 \equiv \lambda f x . \;  f(f(f(A)))$
$MULT \; 2 \: 3 \equiv \lambda f x . \;  f(f(f(f(f(f \:x)))))$
*We get 6 as the final output.*

### Booleans     
In lambda calculus, true, false and if are defined as functions.
>$True \equiv \lambda ab.\;a$
$False \equiv \lambda ab. \; b$ 
$IF \equiv \lambda fxy.\; f\:x\: y$

Also, we can define:
>$EQ_0 \equiv \lambda x. \; x\:(\lambda y.\; False)\; \; True$
This function determines whether x is equal to zero.
*We will use it later*

### Fixed point
First, we need to review the fixed point in the general mathematics: fixed point of a function $f$ is a value $x$ such that $f(x)=x$.
So, can we find a function in lambda calculus such that: $F\; X = X$ ?

Yes, we can, bro, yes. \
The **Y-combinator** $!$
It is a function which could find a fixed point of any function $f$.
$Y \equiv \lambda f. \; (\lambda x.\;f(x\:x))\;(\lambda x.\;f(x\:x))$
> *Let's have a look if we take M as the input of Y combinator.*
$Y\; M \equiv \lambda f. \; (\lambda x.\;f(x\:x))\;(\lambda x.\;f(x\:x))\;M$
$Y\; M \equiv \underbrace{\lambda x.\;M(x\:x)}_{M_1} \; \underbrace{(\lambda x.\;M(x\:x))}_{M_2}\;$
$Y \; M \equiv M(M_2 \; M_2)$
*From the previous step, $M_1$ is equivalent to $M_2$*
*Therefore:*
$Y \; M \equiv M(M_1 \; M_2)$
$Y \; M \equiv M(Y \; M)$
*$Y \; M$ is the fixed point* 

This means we could define recursive function in lambda calculus.
Now, we could define **MULT** function in a new way: add the value of variable $x$ for $y$ times.
In C++, we could write this code:
```C++
int MULT(int y) 
{
    if(y==0)
        return 0;
    return x+MULT(y-1);
}
```
Therefore, we could define **MLT** based on this code:
$MLT \equiv \lambda fxy.\; IF \; (EQ_0\; y)\; \underbrace{\lambda ab.b}_{\text{this is zero} }\;{ADD \; x \; (f \; x \:\underbrace{(PRED \;y)}_{\text{equivalent to y-1}})}$
*We need to find the fixed point $(Y\; F )$ of MLT function. Then this function is proofed to be a recursive function, which means we could replace $f$ by MLT*.

### lambda-definable
**Composition**
This means we build a function by plugging the outputs of some functions into another function.
For example, we have $f(x_1,x_2,\dots x_k)$ and functions $g_1(x_1,x_2,\dots x_k) \; g_2(x_1,x_2,\dots x_k) \dots g_k(x_1,x_2,\dots x_k)$.
Hence, the composition of $f$ function is $f(g_1(x_1,x_2,\dots x_k) \; g_2(x_1,x_2,\dots x_k) \dots g_k(x_1,x_2,\dots x_k))$.
We could define this function in lambda calculus:
$f \equiv \lambda f \vec x g_1 g_2 \dots g_k. \; f \: (g_1 \: \vec x) \; (g_2 \: \vec x)\dots (g_k \: \vec x)$
**Primitive Recursion**
>$h=\rho ^n(f,g)$

This means: 
$f$: $\mathbb{N}^n \to \mathbb{N}$ is the base case.
$g$: $\mathbb{N}^{n+2} \to \mathbb{N}$ is the recursive step.
The recursive function is defined by:
>$h(x_1,x_2\dots x_n,0)=f(x_1,x_2 \dots x_n)$
$h(x_1,x_2\dots x_n,k)=g(x_1,x_2 \dots x_n,k-1,h(x_1,x_2\dots x_n,k-1))$

This $h$ could be expressed by using Y combinator.
$h \equiv Y\{  \lambda z\vec x x.\; IF \; [EQ_0 \: x] \; [F \: \vec x]  [G \: \vec x \: (PRED \: x) \: (z \: \vec x \; (PRED \: x))]\} $

**Minimization**
It handles the $\mu$-recursion:
$\mu ^nf$ represents the least x such that $f(\vec x, x)=0$.
*It is actually to find the position of zero in tuple of n numbers.* 
This function could be define in lambda calculus in this way:
$\lambda \vec x . Y\{ \lambda z \vec x x. \; IF \; [EQ_0 \: (F\: \vec x \: x)]\; [x] \; [z \: \vec x \: (SUCC \: x)] \}\; \vec x \: 0$
The inputs mean we have an array with n number of values and we iterate it from x=0.

So far, we proof that 
1. **Initial function** like projection, successor
2. **Composition**
3. **Primitive recursion**
4. **Minimization**


could be expressed in the lambda calculus.

Therefore, all functions built from these which is computable function could be expressed in lambda calculus. Hence, we have computable $\Rightarrow$ lambda-definable.

