**0\. The Fourier Transforms and its Applications - ****Intro**

The Fourier Series - the studies of periodic phenomenon. 

The Fourier Transform - a limiting case of Fourier series, as applied to non-periodic phenomenon.

Analysis - break up a signal into simpler constituent parts

Synthesis - reassemble the signal from the constituent parts

Both are accomplished by linear operations

Often we classify periodic phenomena as periodicity in time or periodicity in space (physical quantity distributed over a region with symmetry). Some are both (ie wave motion). Based in mathematical symmetry in group theory.

We use as basic building blocks:
$\lambda \nu = v$, that is,  frequency * wavelength = velocity
Model signals - $sin(2\pi t)$, $cos(2\pi t)$ [period 1]
Use 1 period, many frequencies [Use sin(2\pi t), sin(4 \pi t) etc] 

## 1. Fourier Series
\begin{align}
& \sum_{k=1}^N A_k \sin(2 \pi k t + \phi_k) \tag{1.1} \\
& \sum_{k=1}^n A_k \cos(2 \pi kt) + B_k \sin(2 \pi kt) + \frac{a_0}{2} \tag{1.2} \\
& \sum_{k=-n}^n c_k e^{2\pi i k t} \tag{1.3}
\end{align}
(Where (1.2) is obtained by applying the trigonometric identity (A.1) to (1.1), and adding an optional constant and (1.3) is from converting (2) to complex exponentila form. We often use form (1.3).

The signals we use satisfy the **symmetry property** $c_{-k} = \bar{c_k}$, where the latter is the complex conjugate. If the coefficients satisfy the symmetry property, then the imaginary inputs cancel and the ourput is real.

The Fundamental Question: $f(t)$ is a periodic function of period 1. Can we write it as 
$$f(t) = \sum_{k=-n}^n c_k e^{2\pi i k t}$$
Suppose we can. Then, 
\begin{align}
f(t)  & = \sum_{k=-n}^n c_k e^{2\pi i k t} \\ 
c_m e^{2\pi i m t} & = f(t) - \sum_{k \ne m} c_k e^{2\pi i k t} \\ 
c_m  & = f(t)e^{-2\pi i m t} - \sum_{k \ne m} c_k e^{2\pi i (k-m) t} \\
\int_0^1 c_m dt  & = \int_0^1 f(t)e^{-2\pi i m t} dt - \int_0^1 \sum_{k \ne m} c_k e^{2\pi i (k-m) t} dt\\
c_m  & = \int_0^1 f(t)e^{-2\pi i m t} dt - \sum_{k \ne m} c_k \left[ \frac{1}{2 \pi i (k-m)} e^{2\pi i (k-m) t} \right]_0^1\\
c_m  & = \int_0^1 f(t)e^{-2\pi i m t} dt - \sum_{k \ne m} c_k \frac{1}{2 \pi i (k-m)} \underbrace{\left[e^{2\pi i (k-m)} - e^{0} \right]}_{=0, b/c n \ne m}\\
c_m  & = \int_0^1 f(t)e^{-2\pi i m t} dt 
\end{align}

Define $\hat f(k) = c_m   = \int_0^1 f(t)e^{-2\pi i m t} dt $. 
Then the **Fourier Series** for $f(t)$ is $\sum_{k=-n}^n \hat f(k) e^{2 \pi i k t}$

**Note on Convergence:** Any discontinuity in any derivative it impossible to exactly represent as a finite sum of $\sin$ and $\cos$. Any non-smooth signal will generate an infinte number of Fourier coefficients. You need a lot of cancellations for the series to converge.

Fourier series converge uniformly when function is infinitely smooth. The smoother the function, the faster the Fourier series converges.
The Fourier series converges when there are jump discontinuities to the average point at the jump.
The Fourier series converges when the function is continuous. 

**General Case:**
Suppose $f(t)$ is periodic with period 1 and $\int_0^1 |f(t)^2 dt < \infty$ (2). 
Then $\hat f(k) = \int_0^1 e^{-2\pi i k t} f(t) dt $ is well-defined and as $n \rightarrow \infty$,
$$\int_0^1 \left| \sum_{k=-n}^n \hat f(k)e^{-2\pi i k t} - f(t)\right|^2 dt \rightarrow 0$$
This is known as **mean-squared convergence**. Note that the Fourier Series does not converge pointwise in the general case.

## 2. Complex Function Spaces
The condition in (2) is known as **square integrability** and is written $f \in L^2([0,1])$. This is the definition of energy in physics so often this is called "finite energy". The integration is Lebesque integral.

Def: If $f,g \in L^2([0,1])$, define the **function inner product** as
$$(f,g) = \int_0^1f(t)\overline{g(t)}dt$$
(This is the continuous generalization of the dot product of two vectors)

Def: $f \perp g$: $f,g$ are **orthogonal** if $(f,g)=0$.
Def: $(f,f):$ The **norm** of f is $\|f\|^2 = \int_0^1 f(t)^2 dt$
Def: The **Pythagoream Theorem** of function spaces states that $\|f + g \|^2 = \|f \|^2 + \|g \|^2$ iff $f \perp g$.
Def: The **projection** of $u$ on $v$ is $(v,u)u$.

**The Fourier Transform as a Projection in Function Space:** Now, $(e^{-2\pi i m t},e^{-2\pi i n t}) = \begin{cases} 0 \; m \ne n \\ 1 \; m = n\end{cases}$ (the complex exponential has an orthogonality relationship and can form an orthonormal basis). The Fourier Transform is the projection of the orignial function onto the space of complex exponentials, since $(f,e^{-2\pi i n t}) = \int_0^1 f(t) \overline{e^{2\pi i m t}} dt = \int_0^1 f(t)e^{-2\pi i m t} dt = \hat f(n)$. The complex exponentials form an orthonomal basis for the space of square-normed periodic functions $L^2([0,1])$.

Def: **Rayleigh's Identity** states that $\int_0^1 f(t)^2 dt = \sum_{-\infty}^{\infty}|\hat f(k)|^2$. ("The length of a vector is the sum of squares of its components"). Often the LHS "the length of the function" is interpreted as "the energy of the function", and Rayleigh's Identity says "the energy of a function is the sum of the energy of its components".

## 3. Application of Fourier Series to Heat Flow
Given an inital distribution of temperature $f(x)$ along a circle (a heated 2D ring), where $x$ is revolutions around the circle, what is the distribution $u(x,t)$ of the temperature at time $t$? 
The temperature is periodic of period 1 in space, such that and $u(x+1,t) = u(x,t)$. 

Then $u(x,t) = \sum_{k=-\infty}^\infty c_k(t) e^{2\pi i k x}$, where the time dependence is in the $c_k$. What are the $c_k$.

We can use the **heat equation/ diffusion equation**: The time derivative is a constant times the second spacial derivative $u_t = a u_{xx}$

We choose $a = 0.5$. Plug $u(x,t) = \sum_{k=-\infty}^\infty c_k(t) e^{2\pi i k x}$ into $u_t = 0.5 u_{xx}$.

Then, taking the derivative, 

\begin{align}
u_t & = \sum_k c_k'(t) e^{2\pi i k x} \\
u_{xx} & = \sum_k c_k(t) (2\pi i k)^2 e^{2\pi i k x} = \sum_k -c_k(t) 4 \pi^2 k^2 e^{2\pi i k x} \\
\sum_k c_k'(t) e^{2\pi i k x} & = 0.5 * \sum_k -c_k(t) 4 \pi^2 k^2 e^{2\pi i k x} \\
\sum_k c_k'(t) e^{2\pi i k x} & = \sum_k -c_k(t) 2 \pi^2 k^2 e^{2\pi i k x} \\
c_k'(t) & = -2 \pi^2 k^2 c_k(t) \;\;\text{by equating the terms} 
\end{align}
This is a simple differential equation which is solved as $c_k(t) = c_k(0)e^{-2\pi^2 k^2 t}$.

Now, as our initial temperature distribution was $f(x)$, 
$f(x)  = u(x,0) = \sum_{k= - \infty}^{\infty} c_k(0) e^{2\pi i k x}$, so $c_k(0)$ is the k-th coefficient of f, $\hat f(k)$.

The final solution to the ring heat equation is 
$$u(x,t) = \sum_{k=-\infty}^\infty \hat f (k) e^{-2\pi^2 k^2 t} e^{2\pi i k x} \tag{3.1}$$
Using $\hat f(k) = \int_0^1 e^{-2\pi i k y} f(y) dy$, we have 
\begin{align}
u(x,t) & = \sum_{k=-\infty}^\infty \hat f (k) e^{-2\pi^2 k^2 t} e^{2\pi i k x} \\
& = \sum_{k=-\infty}^\infty \left(\int_0^1 e^{-2\pi i k y} f(y) dy \right) e^{-2\pi^2 k^2 t} e^{2\pi i k x} \\
& = \int_0^1  \left( \sum_{k=-\infty}^\infty e^{-2\pi i k y} e^{-2\pi^2 k^2 t} e^{2\pi i k x} \right) f(y) dy\\
& = \int_0^1  \left( \sum_{k=-\infty}^\infty e^{2\pi i k (x-y)} e^{-2\pi^2 k^2 t} \right) f(y) dy\\
\end{align}

Define the **heat kernel** $g(x,t) = \sum_{k=-\infty}^\infty e^{2\pi i k x} e^{-2\pi^2 k^2 t} $. g(x,t) is also called **the fundamental solution to the heat equation** as well as **Green's function**.
Then we can write $u(x,t) = \int_0^1 g(x-y,t) f(y) dy$, that is, the general solution at any time $t$ is a **convolution** of the initial condition $f(y)$ at time 0 with the kernel $g(x-y,t)$.



##A.1: Appendix:
$$\sin(2 \pi kt + \phi_k) = \sin(2\pi k t)(\cos \phi_k) + \cos(2\pi k t)\sin(\phi_k) \tag{A.1}$$
$$\cos(2 \pi kt) = \frac{e^{2\pi i k t} + e^{-2 \pi i k t}}{2}, \sin(2 \pi kt) = \frac{e^{2\pi i k t} - e^{-2 \pi i k t}}{2i}$$