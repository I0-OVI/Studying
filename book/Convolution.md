# Convolution 
> From wikipedia: In mathematics, convolution is a mathematical operation on two functions $f$ and $g$ that produces a third function $f*g$, as the integral of the product of the two functions after one is reflected about the y-axis and shifted.

### Definition
The convolution of $f$ and $g$ is written $f*g$, denoting the operator with the symbol $*$. It is defined as the integral of the product of two functions after one is reflected about the y-axis and shifted.This is a particular kind of integral transform:
$$(f*g)(t) = \displaystyle \int _{-\infty} ^ \infty  f(\tau) g(t-\tau)\; d\tau$$
If functions $f$ and $g$ only support on the $[0,\infty)$ (zero for the negative argument), the upper limit could be truncated, resulting in:
$$f(g*t)(t)= \displaystyle \int _{-\infty} ^{t} f(\tau)g(t-\tau) \; d\tau \;\;\text{for} \;f,g :[0,\infty)\to \R$$ 

Take an example to understand this formula:
Imaging a train is moving through a tunnel and it starts to whistle. We define the strength of the whistle by $f(\tau)$ where $\tau$ is the time the whistle is sounded. Generally, we call them inputs. $g$ function describes how the previous input is heard after some seconds passed.  $t-\tau$ is the elapsed time since whistle. If this value is less than zero, it means the whistle is not sounded now. Therefore, $g$ should return zero if the input is negative. This means the $f$ and $g$ are casual. The upper limit of the integral should be t instead of infinity. The contribution of whistle at time $\tau$ is defined by the product of the input and the residue of previous input since this method could easily evaluate the actual amount of the whistle's contribution from $\tau$ remaining until $t$. 

*t and $\tau$ are easy to make us confuse due to their meaning.*
*$\tau$ is the input time.*
*$t$ is the time when we observe the output of the system.*

Now, if we replace the continuously moving train by several small cars, the convolution will become to discrete one. 
$$\text{causal one}\;\;\;\;\;\;\;\;\;[f*g](n) = \displaystyle \sum _{m=0} ^{n} f[m] g[n-m] $$
$$\text{non-causal one}\;\;\;\;\;\;\;\;\;[f*g](n) = \displaystyle \sum _{m=-\infty} ^{\infty} f[m] g[n-m] $$
The discrete one could become to continuous one in following steps:
>*First, we need to know how the discrete signals related to continuous signals.*
$f[n]\to x(nT) \;\;\;g[n]\to g(nT)$
*The indexes of the $f$ and $g$ in discrete function are like the indexes of array. It shows which value we are looking at. But the indexes of the continuous one are labeled by the time when samples were taken.*
*So, we could convert the discrete one to continuous one by matching the index: $t=nT$*
*Hence we can transform the equation:*
$$[f*g](n)=\displaystyle \sum _{k=-\infty} ^{\infty} f(kT)g((n-k)T)$$
$$\displaystyle \lim _{T \to 0} \sum _{k=-\infty} ^{\infty} f(kT)g((n-k)T)= \int _{-\infty} ^ \infty  f(\tau) g(t-\tau)\; d\tau$$
*where we define $kT=\tau$.*

### Fourier Transformation
Fourier transformation is a method to represent a signal in the frequency domain instead of time.
For a continuous-time signal $x(t)$:
$$X(f)=\displaystyle \int ^{\infty} _{-\infty} x(t)e^{-j2\pi ft} \;\; dt \tag {*}$$ 
where $X(f)$ is the frequency domain representation of $x(t)$.
How could we transform the equation above to get $X(f)=F(f)*G(f)$.
>*First, we substitute*
$$x(t)= (f*g)(t) = \displaystyle \int _{-\infty} ^ \infty  f(\tau) g(t-\tau)\; d\tau$$*into the frequency domain representation.*
$$X(f)=\displaystyle \int ^{\infty} _{-\infty} \left( \int _{-\infty} ^ \infty  f(\tau) g(t-\tau)\; d\tau \right) e^{-j2\pi ft} \;\; dt$$
Based on the **Fubini’s Theorem**, we could swap the order of integral:
$$X(f)=\displaystyle \int ^{\infty} _{-\infty} f(\tau) \left( \int _{-\infty} ^ \infty    g(t-\tau)\;e^{-j2\pi ft}\; dt \right)  \;\; d\tau$$
*We could first solve this integral:$\displaystyle\int _{-\infty} ^ \infty  g(t-\tau)\;e^{-j2\pi ft} dt$*
*Substitute $u=t-r$,we could get:*
$$\displaystyle \int _{-\infty} ^ \infty  g(u)\;e^{-j2\pi f(u+r)} du$$
$$\displaystyle e^{-j2\pi fr} \int _{-\infty} ^ \infty  g(u)\;e^{-j2\pi fu} du$$
*Let's look at the definition of the Fourier transformation and we can notice that this part $\displaystyle\int _{-\infty} ^ \infty  g(u)\;e^{-j2\pi fu} du$ can be expressed as $G(f)$.
We could plug it back to the original equation:*
$$X(f)=\displaystyle \int ^{\infty} _{-\infty} f(\tau) e^{-j2\pi fr} G(f)  \;\; d\tau$$
$$X(f)=\displaystyle G(f)\int ^{\infty} _{-\infty} f(\tau) e^{-j2\pi fr}   \;\; d\tau$$
$$X(f)=\displaystyle G(f)\cdot F(f)$$

### Convolution Neural Network
In this part we just talk about the convolution in CNN. I have not learnt neural network and I will update it in the future. 
First, we need to know an important term: kernel. It is a small matrix of weights which slides over the input matrix to get the output matrix.
Before having an example, the output matrix is calculated as follow:
$$O(i,j)=\displaystyle \sum _{m} \sum _{n} I(i+n,j+m) \cdot K(n,m)$$
where $n$ and $m$ means the coordinates in the kernel matrix, $i$ and $j$ represent the coordinates in output matrix.
*It is easy to notice this equation is like the form of 1D convolution:$[f*g](n) = \displaystyle \sum _{m=0} ^{n} f[m] g[n-m]$. However, in the 2D convolution, since there are no flipping operation, addition is applied in the equation. So, we just say convolution in CNN but this operation is actually dot products.*
>*Now we have the input matrix:
$$ 
    I= \begin{bmatrix} 
        1&2&3&0&1 \\ 
        4&5&6&1&2 \\ 
        7&8&9&0&3 \\
        1&2&1&3&1
        \end{bmatrix}
    $$
and the kernel matrix:
$$
    K= \begin{bmatrix}
        1&0&-1 \\
        1&0&-1 \\
        1&0&-1 
    \end{bmatrix}
$$
We slide the kernel to this part of input matrix:$\begin{bmatrix}
        1&2&3 \\
        4&5&6 \\
        7&8&9 
    \end{bmatrix}$
Then we have the dot product with kernel matrix:$\begin{bmatrix}
        1 \cdot 1 & 2 \cdot 0 & 3 \cdot -1 \\
        4 \cdot 1 & 5 \cdot 0 & 6 \cdot -1 \\
        7 \cdot 1 & 8 \cdot 0 & 9 \cdot -1
    \end{bmatrix}$
Finally, we sum these values together to get the first element in the output matrix:$\begin{bmatrix}
        -6&?&? \\
        ?&?&? \\
        ?&?&? 
    \end{bmatrix}$
In the next step, we will move the kernel to this part:$\begin{bmatrix}
        2&3&0 \\
        5&6&1 \\
        8&9&0 
    \end{bmatrix}$*

The output matrix shows the feature of input matrix which depends on the weights storing in the kernel. For example, the kernel above is a vertical edge detector which shows the brighter edge. We also have horizontal edge detector: $\begin{bmatrix}
        -1&-1&-1 \\
        0&0&0 \\
        1&1&1 
    \end{bmatrix}$. 
These output matrixes calculated from different kernels are all input in the early layers of the neural network.