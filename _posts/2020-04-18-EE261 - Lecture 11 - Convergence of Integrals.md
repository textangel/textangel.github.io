**Fourier Analysis:** This is the field of convergence of Fourier Integrals and is a subfield of Functional Analysis. Some example issues include: 
1. Recall, $F(\pi(t)) = sinc(s)$, so $F^{-1}sinc(s) = \pi(t)$. But there is no elementary way to evaluate the integral of $F^{-1}sinc(s)$. Special ad-hoc techniques have been developed to evaluate this.
2. Consider the functions $f(t) = 1$, $f(t) = \sin(\pi t)$, $f(t) = \cos(\pi t)$. There is no way to make sense of the integral of the Fourier Transform $F(f(t)) = \int_{-\infty}^{\infty} e^{-2 \pi i s t} f(t) dt$ which all do not converge.

Need a more robust definition of the Fourier Transform to deal with common signals so that the integral converges and that Fourier Inversion also works. Previous techniques produced ad hoc special cases for for special functions for particular integrals. This was inpractical and Schwartz (1940) reworked the foundations of Fourier Analysis to defined Fourier Series in terms of generalized functions (distributions).

# The Schwarz Space
Identify a class of signals $S$ upon which to build a theory. 

Def: The **Schwarz Space** $S$is the class of rapidly decreasing functions $f$ such that 
1. $f(x)$ is smooth (infinitely differentiable) $\forall f \in S$.
2. $\forall m,n \ge 0, |x|^n \left| \frac{d^m}{dx^m} f(x) \right| \to 0$ as $x \to \pm \infty$, that is, as $x \to \infty$, any derivative $m$ of $f$ tends to 0 faster than any power $n$ of x. (m,n are independent here, so, e.g. the third derivative tends to 0 faster than $x^{100}$). In particular, this implies that as $x \to \pm \infty, f(x) \to 0$.

The Schwartz Space $S$  has the following properties:
1. If $f \in S$, then $Ff$ is defined and $Ff \in S$.
2. $f \in S \implies F^{-1}Ff =f$. Fourier Inversion of a function $Ff$ in $S$ is defined and is in $S$.
3. **Parseval's Identity**: $\int_{-\infty}^{\infty} |Ff(s)|^2 ds = \int_{-\infty}^{\infty} |f(t)|^2 dt \forall f \in S$  ("Total energy or total spectral power is the same in the frequency domain and in the time domain")

Examples of elements in S
 - Gaussians are in $S$. 
 - Let $C$ (**compact support** class) be the class of smooth (infinitely differentiable) functions which are identical to 0 outside a finite interval. $C \subset S$
 
Why this definition of $S$?
Recall **Derivative Theorem of Fourier Transforms:** The Fourier Transform of the n-th derivative is $(2 \pi i s)^n$ times the fourier transform. The Fourier Transform turns differentiation into multipluation. Intuition: The derivative formula relates differnetiability and the powers of F. Since all Fourier Transform must satisfy this formula, heuristically 


**Proof of Parseval's Identity**: $\int_{-\infty}^{\infty} |Ff(s)|^2 ds = \int_{-\infty}^{\infty} |f(t)|^2 dt$ ("Total energy or total spectral power is the same in the frequency domain and in the time domain")

If $f \in S$, then both integrals in Parseveal's identitiy exists.
We prove a more general version, using a change of order of integration. We can do this becuase Fourier integrals in $S$ are guarenteed to converge.

$$\int_{-\infty}^{\infty} Ff(s)\overline{Fg(s)} ds = \int_{-\infty}^{\infty} f(t)\overline{g(t)} dt$$

since $g \in S$, we can write $g(x) = \int_{-\infty}^{\infty}  e^{-2 \pi i s x} Fg(s) ds \Leftrightarrow \overline{g(x)} = \int_{-\infty}^{\infty}  e^{-2 \pi i s x} \overline{Fg(s)} ds $, so $\int_{-\infty}^{\infty}  f(x) \overline{g(x)} dx =  \int_{-\infty}^{\infty}  f(x) \left( \int_{-\infty}^{\infty}  e^{-2 \pi i s x} \overline{Fg(s)} ds \right)  dx  \\ = \int_{-\infty}^{\infty} \int_{-\infty}^{\infty} e^{-2 \pi i s x}  f(x)  \overline{Fg(s)} ds dx = \int_{-\infty}^{\infty}   \overline{Fg(s)} \left( \int_{-\infty}^{\infty}  e^{-2 \pi i s x} f(x) dx \right)  ds \\ =  \int_{-\infty}^{\infty}   \overline{Fg(s)} Ff(s)  ds $

We can change the order of integration becuase in $S$ everything converges.

## Generalized Functions (aka Distributions)
**Intro: Dirac Delta Function**
Generalized Functions are also known as **distributions** (not the same as probability distrbutions), as typified by the Dirac delta function $\delta$.

$\delta$ is supposed to represent a function that is concentrated at a point. The "heuristic" definitions of $\delta(x) = \begin{cases} 0 \; x \ne 0 \\ \infty \; x = 0 \end{cases}, \int_{-\infty}^\infty \delta(x) dx = 1, \int_{-\infty}^\infty f(x)\delta(x) dx = f(0)$ are heuristically helpful but can be considered abuse of notation. There are various other ways of defining this but it's always a limiting process. e.g. $\delta$ is the limit of a family of rectangle functions with area 1, and as the width $\epsilon \to 0$. The most natural way is to define $\delta$ as a generalized function.

**Definition of Generalized Functions (aka Distributions)**
Start with a class of **test functions** ($S$ for Fourier Transforms). This is the 'best functions" for the problem at hand.
1. Associated with the test functions is a class of **generalized functions** aka **distributions**. A distribution $T$ is a linear functional on the test functions. (A functional is an operator from a space to the space of real numbers. For a given test function $\phi$, $T(\phi)$ is a number and $T$ is linear.)
2. The distributions must satisfy continuity: If $\phi_n \to \phi$, then $T(\phi_n)$ converges to $T(\phi)$. [This is very complicated to make fully rigorous].

We often say that a distribution is **paired** with a test function, and write $<T,\phi> $ in place of $T(\phi)$. In functional analysis we say the space of distributions is the **dual space** of the set of test functions. Note that Generalized functions comprise the test functions as well as all associated functionals.

Intepretation: $\phi$ is a distribution of some physical property (temperature, voltage, etc) and $T$ is a way of measuring it.

Recall that the constant function, the rectangle function $\pi$, the triangle function $\Lambda$, and trig functions $\sin(x), cos(x)$ are all not in the Schwarz class $S$. However, we can define them as generalized functions (distributions) to make the integration converge. As generalized functions the above functions operate on test functions to produce real numbers, in this case, by integration.
Define:
  - $<1,\phi> = \int_{-\infty}^\infty 1 \cdot \phi(x) dx$
  - $<\pi,\phi> = \int_{-\infty}^\infty \pi(x) \cdot \phi(x) dx$ The rectangle function
  - $<\sin(2\pi x),\phi> = \int_{-\infty}^\infty \sin(2 \pi x) \cdot \phi(x) dx$   ; $\sin(2\pi x)$ does not converge, but if we multiply it by a $\phi(x)$ which is dying off, the integral does converge. 
  - $<e^{-2 \pi i a x},\phi> = \int_{-\infty}^\infty e^{-2 \pi i a x} \cdot \phi(x) dx$ is a distribution defined for almost all $\phi(x)$, provided that $\phi(x)$ increases sub-exponentially.

For any function $f(x)$, define $f(x)$ as a generalized function by defining:
- $<f(x),\phi> = \int_{-\infty}^\infty f(x) \cdot \phi(x) dx$. The RHS is known as the **parent** of $f(x)$.

Example: We can now interpret $\delta$ as a function whose purpose is to evaluate the function at the origin.
Instead of  $\int_{-\infty}^\infty f(x)\delta(x) dx = f(0)$, we can instead define $\delta$ this way: Def $\delta: <\delta, \phi> = \phi(0)$.
Instead of $\int_{-\infty}^\infty \delta(x-y)f(y) dy = f(x)$, we can instead define $\delta_a$ this way: Def $\delta_a: <\delta_a, \phi> = \phi(a)$ (This is the **shifted delta function**.)  

## The Generalized Fourier Transform (The Fourier Transform of Generalized Functions)
Recall that for Generalized Functions:
  - Test functions are generalized functions
  - Distributions $T$ indiced by test functions are generalized functions: if $f(x)$ is a funciton such that $\int_{-\infty}^\infty f(x)\phi(x) dx$ converges, we defined the pairing $<f, \phi> = \int_{-\infty}^\infty f(x)\phi(x) dx$ and $<f, \phi>$ is a generalized function.

Take the set of test functions to be the Schwarz class $S$ of rapidly decreasing functions. Recall $S$ has the properties: 1. $f \in S \implies Ff \in S$ and 2. $f \in S \implies F^{-1}Ff =f$. We form a corresponding class of distributions, called **tempered distribution**.

If $f \in S$, then we can change the order of integration in the below equation and the equaliy holds:
$$<FT, f> = \int_{-\infty}^\infty FT(x)f(x) dx = \\ 
\int_{-\infty}^\infty  \left( \int_{-\infty}^{\infty}  e^{-2 \pi i x y} T(y) dy \right) f(x) dx = \\
\int_{-\infty}^\infty  \int_{-\infty}^{\infty}  e^{-2 \pi i x y} T(y) f(x) dy dx = \\
\int_{-\infty}^\infty  \left( \int_{-\infty}^{\infty}  e^{-2 \pi i x y} f(x) dx \right) T(y) dy = \\
\int_{-\infty}^\infty  Ff(y) T(y) dy = <T, Ff>$$

Def: For a given tempered distribution $T$, define its **Fourier Transform** $FT$ as $<FT, f> = <T, Ff>$, where $f \in S$.
Def: For a given tempered distribution $T$, define its **Inverse Fourier Transform** $F^{-1}T$ as $<F^{-1}T, f> = <T, F^{-1}f>$, where $f \in S$.
Corollary: From the above, we have $<F^{-1}FT, f> = <FT, F^{-1}f> =  <T, FF^{-1}f> =  <T, f>$, so Fourier Inversion holds for generalized Fourier Transforms and $F^{-1}FT = T$.

**Fourier Transforms of Generalized Functions:**
  - $F\delta$: We can now define the Fourier Transform of the Dirac Delta $\delta$, $F\delta$: $<F\delta,f> = <\delta,Ff> = Ff(0) = \int_{-\infty}^\infty e^{-2 \pi i x 0} f(x) dx = \int_{-\infty}^\infty 1 f(x) dx = <1, f> $.
  Since $<F\delta,f> = <1, f>, \boxed{F\delta = 1}$.
  (This is another example of concentrated in time domain -> spread out in frequency domain. $\delta$ is infinitely concentrated, 1 is "uniformly spread out")
  - $F\delta_a$: We can now define the Fourier Transform of the Shifted Delta $\delta_a$, $F\delta_a$: $<F\delta_a,f> = <\delta_a,Ff> = Ff(a) = \int_{-\infty}^\infty e^{-2 \pi i x a} f(x) dx = <e^{-2 \pi i ax}, f> $.
  Since $<F\delta_a,f> = <e^{-2 \pi i ax}, f>, \boxed{F\delta_a = e^{-2 \pi i ax}}$.
  - $Fe^{2 \pi i ax}$: We can now define the Fourier Transform of the Complex Exponential $e^{2 \pi i ax}$, $Fe^{2 \pi i ax}$: $<Fe^{2 \pi i ax},f> = <e^{ 2 \pi i ax},Ff> = \int_{-\infty}^\infty e^{2 \pi i x a} Ff(x) dx = F^{-1}Ff(a) = f(a) = <\delta_a, f>$. Since $<Fe^{2 \pi i ax},f> = <\delta_a, f>$, $\boxed{Fe^{2 \pi i ax} = \delta_a}$.
  - $F1$: We plug in $a=0$ in the above case such that $e^{-2 \pi i 0x} = 1$, and obtain $\boxed{F1 = \delta}$.
  - $F\cos(2\pi a x)$: Recall that $\cos(2\pi a x) = \frac{1}{2} (e^{2 \pi i a x} + e^{-2 \pi i a x})$. Then using the above we obtain $\boxed{F\cos(2\pi a x) = \frac{1}{2} (\delta_a + \delta_{-a})}$ [Two spikes, one at $a$, one at $-a$.]
  - $F\sin(2\pi a x)$: Recall that $\sin(2\pi a x) = \frac{1}{2i} (e^{2 \pi i a x} - e^{-2 \pi i a x})$. Then using the above we obtain $\boxed{F\sin(2\pi a x) = \frac{1}{2i} (\delta_a - \delta_{-a})}$ [One positive spike at $a$, one negative spike at $-a$.]

**Derivatives of Distrbitions**
If T is a generalized function, how to define its derivative T'? Have to define the pairing $<T',f>$.
If T' were given by a function, $<T',f> = \int_{-\infty}^\infty T'(x) f(x) dx$. We can use integration by parts to reduce this to $<T',f> = \underbrace{\left[T(x)f(x) \right]_{-\infty}^\infty}_{=0} - \int_{-\infty}^\infty T(x) f'(x) dx =  - \int_{-\infty}^\infty T(x) f'(x) dx = -<T, f'>$. (The first term reduces to 0 because we assumed that $f(x) \in S$, meaning that it its value at $-\infty, \infty$ is 0.)