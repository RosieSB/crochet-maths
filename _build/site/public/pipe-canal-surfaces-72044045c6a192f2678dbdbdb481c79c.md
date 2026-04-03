# Canal surfaces and pipe surfaces

## Canal/Channel Surfaces
### Parametrisation

**References:** Mostly following [Hartman *Geometry and Algorithms for Computer Aided Design*](https://www2.mathematik.tu-darmstadt.de/~ehartmann/cdgen0104.pdf), pp. 117, although I do not trust the derivation therein. [Hilbert & Cohn-Vossen *Geometry and the Imagination* (1999)](https://en.wikipedia.org/wiki/Geometry_and_the_Imagination) also mentions them - see page 231.

Let $\gamma:[a,b]\to\mathbb{R}^3$ be a $C^2$ parametrised curve in $\mathbb{R}^3$, and let $r\in C^2([a,b])$. 

The *canal surface* of $\gamma$ with radius function $r$ is, by definition, the envelop of the family of spheres $\{S_t:t\in[a,b]\}$, where for each $t\in[a,b]$, $S_t$ denotes the sphere of radius $r(u)$ with centre $\gamma(u)$.

Consider $f\in C^2(\mathbb{R}^3\times [a,b])$ defined by 
$$
f(\mathbf{x},u)=\left(\mathbf{x}-\gamma(u)\right)^2-r(u)^2
$$
Then, 
$$
\forall u\in[a,b], \hspace{1em} S_u = \left\{\mathbf{x}\in\mathbb{R}^3:f(\mathbf{x},u)=0\right\}
$$

The "envelope condition" is simply the smoothness condition
$$
f_t(\mathbf{x},u)=0,
$$
or in other words,
$$
2(\mathbf{x}-\gamma)\dot\gamma - r\dot r = 0
$$

:::{note}
When $r$ is constant, the resulting envelope is called a *pipe surface*.
:::

In the more general case where $r$ may not be constant, the two defining equations for the canal surface are
$$
\left\{\begin{array}{rclr}
(\mathbf{x}-\gamma(u))^2 &=& r(u)^2 & (1) \\[.5em]
(\mathbf{x}-\gamma(u))\dot\gamma(u) &=& r(u)\dot r(u) & (2)
\end{array}\right.\hspace{.25em}, \hspace{2em} \forall t\in[a,b].
$$
Equation (2) describes a plane perpendicular to $\dot\gamma(u)$, which must intersect the sphere of equation (1) in a circle lying in a plane orthogonal to the curve $\gamma$ at time $t$. 

Let $\mathbf{c}(u)$, $\rho(u)$ denote respectively the centre and radius of this circle. Let $\alpha(u)$ denote the angle between $\mathbf{x}-\gamma(u)$ and $\dot\gamma(u)$. 

:::{figure} figs/canal-diagram.png
:width: 400

Diagram to assist canal surface parametrisation.
:::

$$
\cos(\alpha(u)) = \frac{\vert\dot r(u)\vert}{\Vert\dot\gamma(u)\Vert}=\frac{\sqrt{r(u)^2-\rho(u)^2}}{r(u)}
$$
and so
$$
\sqrt{r(u)^2-\rho(u)^2}=\frac{r(u)\dot r(u)}{\Vert\dot\gamma(u)\Vert}.
$$
In other words, the curves of constant $t$ for the canal surface are circles centred on 
$$
\mathbf{c}(u)=\gamma(u)-\frac{r(u)|\dot r(u)|}{\Vert\dot\gamma(u)\Vert^2}\dot\gamma(u)
$$
and with radius
$$
\rho(u) =  r(u)\left(1-\frac{\dot r(u)^2}{\Vert\dot\gamma(u)\Vert^2}\right)^{\frac{1}{2}} = r(u)\sin(\alpha(u)).
$$

Let $\mathbf{T}$, $\mathbf{N}$ and $\mathbf{B}$ be the moving orthonormal frame along $\gamma$ given by the Frenet-Serret formulas. Then
$$
\mathbf{T}=\frac{\dot\gamma(u)}{\Vert\dot\gamma(u)\Vert},
$$
and, if $s$ denotes arc length, Frenet-Serret says
$$
\left\{\begin{array}{rcl}
\displaystyle\frac{d\mathbf{T}}{ds}&=&\kappa\mathbf{N},\\[1em]
\displaystyle\frac{d\mathbf{N}}{ds}&=&-\kappa\mathbf{T}+\tau\mathbf{B},\\[1em]
\displaystyle\frac{d\mathbf{B}}{ds}&=&-\tau\mathbf{N}.
\end{array}\right.
$$
(In practice, for crochet, we may want an arc length parametrisation anyway, if arc length is proportional to row number, which it may be).

Then $\mathbf{N}$ and $\mathbf{B}$ are unit vectors, orthogonal to each other and to $\dot\gamma$. This allows us to right down a parametrisation for the canal surface, namely

$$
\mathbf{x}(u,v) = \mathbf{c}(u) + \rho(u)\left( \mathbf{N}\cos(v)+\mathbf{B}\sin(v) \right)
$$
or, more lengthily,
:::{math}
:enumerated: true
:label: eq:canal-param
\begin{aligned}
\mathbf{x}(u,v) &= \gamma(u)-\frac{r(u)|\dot r(u)|}{\Vert\dot\gamma(u)\Vert^2}\dot\gamma(u) \\
&\hspace{1.5em}+ r(u)\left(1-\frac{\dot r(u)^2}{\Vert\dot\gamma(u)\Vert^2}\right)^{\frac{1}{2}}\left( \mathbf{N}\cos(v)+\mathbf{B}\sin(v) \right)
\end{aligned}
:::


### Example: Helix
Arc length parametrisation [^1]:
$$
\gamma(u) = \left(\begin{array}{c} 
\cos\left(\frac{u}{\sqrt{2}}\right) \\
\sin\left(\frac{u}{\sqrt{2}}\right) \\
\frac{u}{\sqrt{2}}
\end{array}\right), \hspace{2em} u\in\mathbb{R}.
$$
Pipe surface, i.e. assume $r$ is constant. Then $\dot r=0$, and the surface parametrisation [](#eq:canal-param) simplifies somewhat:

:::{math}
:enumerated: true
:label: eq:pipe-param
\mathbf{x}(u,v) = \gamma(u) + r\left( \mathbf{N}\cos(v)+\mathbf{B}\sin(v) \right)
:::

Frenet-Serret apparatus: Recall arc length parametrisation means $\Vert\dot\gamma(u)\Vert=1$. So,
$$
\mathbf{T} = \dot\gamma(u) = 
\left(\begin{array}{c} -\frac{1}{\sqrt{2}}\sin\left(\frac{u}{\sqrt{2}}\right) \\
\frac{1}{\sqrt{2}}\cos\left(\frac{u}{\sqrt{2}}\right) \\
\frac{1}{\sqrt{2}}
\end{array}\right).
$$

$$
\frac{d\mathbf{T}}{ds} =  
\left(\begin{array}{c} -\frac{1}{2}\cos\left(\frac{u}{\sqrt{2}}\right) \\
-\frac{1}{2}\sin\left(\frac{u}{\sqrt{2}}\right) \\
0
\end{array}\right) = \kappa\mathbf{N}.
$$
So $\kappa = \left\Vert\frac{d\mathbf{T}}{ds}\right\Vert = \frac{1}{2}$ and the normal is
$$
\mathbf{N} = \left(\begin{array}{c} -\cos\left(\frac{u}{\sqrt{2}}\right) \\
-\sin\left(\frac{u}{\sqrt{2}}\right) \\
0
\end{array}\right).
$$
Finally, 
$$
\frac{d\mathbf{N}}{ds} = \left(\begin{array}{c} \frac{1}{\sqrt{2}}\sin\left(\frac{u}{\sqrt{2}}\right) \\
-\frac{1}{\sqrt{2}}\cos\left(\frac{u}{\sqrt{2}}\right) \\
0
\end{array}\right) = -\kappa\mathbf{T}+\tau\mathbf{B}
$$
so
$$
\tau\mathbf{B} = \left(\begin{array}{c} \frac{1}{\sqrt{2}}\sin\left(\frac{u}{\sqrt{2}}\right) \\
-\frac{1}{\sqrt{2}}\cos\left(\frac{u}{\sqrt{2}}\right) \\
0
\end{array}\right)+\frac{1}{2}\left(\begin{array}{c} -\frac{1}{\sqrt{2}}\sin\left(\frac{u}{\sqrt{2}}\right) \\
\frac{1}{\sqrt{2}}\cos\left(\frac{u}{\sqrt{2}}\right) \\
\frac{1}{\sqrt{2}}
\end{array}\right) 
= \left(\begin{array}{c} 
\frac{1}{2\sqrt{2}}\sin\left(\frac{u}{\sqrt{2}}\right) \\
-\frac{1}{2\sqrt{2}}\cos\left(\frac{u}{\sqrt{2}}\right) \\
\frac{1}{2\sqrt{2}}
\end{array}\right)
$$
Hence $\tau = \frac{1}{2}$ and we have binormal
$$
\mathbf{B} = \left(\begin{array}{c} 
\frac{1}{\sqrt{2}}\sin\left(\frac{u}{\sqrt{2}}\right) \\
-\frac{1}{\sqrt{2}}\cos\left(\frac{u}{\sqrt{2}}\right) \\
\frac{1}{\sqrt{2}}
\end{array}\right).
$$

So the helix pipe with radius $r$ has parametrisation
$$
\mathbf{x}(u,v) = \left(\begin{array}{c} 
\cos\left(\frac{u}{\sqrt{2}}\right) \\
\sin\left(\frac{u}{\sqrt{2}}\right) \\
\frac{u}{\sqrt{2}}
\end{array}\right) + r\left(\left(\begin{array}{c} -\cos\left(\frac{u}{\sqrt{2}}\right) \\
-\sin\left(\frac{u}{\sqrt{2}}\right) \\
0
\end{array}\right)\cos(v)+\left(\begin{array}{c} 
\frac{1}{\sqrt{2}}\sin\left(\frac{u}{\sqrt{2}}\right) \\
-\frac{1}{\sqrt{2}}\cos\left(\frac{u}{\sqrt{2}}\right) \\
\frac{1}{\sqrt{2}}
\end{array}\right)\sin(v)\right)
= \left(\begin{array}{c} 
(1-r\cos(v))\cos\left(\frac{u}{\sqrt{2}}\right) + \frac{r}{\sqrt2}\sin\left(\frac{u}{\sqrt2}\right)\sin(v) \\
(1-r\cos(v))\sin\left(\frac{u}{\sqrt{2}}\right)-\frac{r}{\sqrt2}\cos\left(\frac{u}{\sqrt2}\right)\sin(v) \\
\frac{u+r\sin(v)}{\sqrt{2}}
\end{array}\right).
$$


[^1]: Check:  $\displaystyle\int_0^u\Vert\dot\gamma(u)\Vert dt = \int_0^u\sqrt{\frac{1}{2}\sin^2\left(\frac{t}{\sqrt{2}}\right)+\frac{1}{2}\cos^2\left(\frac{t}{\sqrt{2}}\right)+\frac{1}{2}}dt =\int_0^u1dt = u$  ✓ . 

## Crocheting a pipe surface

This is a more complicated endeaver than that of a torus. One simplification to try first is to assume the directrix curve $\gamma$ is planar, so that torsion, $\tau$, is equal to $0$.

Locally, it seems plausable that a pipe surface with (constant) radius $r>0$, planar directrix $\gamma$ and curvature $\kappa$ would coincide with a torus with major radius $\frac{1}{\kappa}$ and minor radius $r$.

How would one prove this? Well, a torus is itself a pipe surface of a circle with radius $R$. Indeed, using [](#eq:pipe-param) with $\gamma(u) = (R\cos(u),R\sin(u),0)$, we have $\mathbf{N}=(-\cos(u),-\sin(u),0)$, $\mathbf{B}=(0,0,1)$ and

\begin{align*}
\mathbf{x}(u,v) &= (R\cos(u),R\sin(u),0) + r\Big( (-\cos(u),-\sin(u),0)\cdot\cos(v) \\ 
&\hspace{18em} +(0,0,1)\cdot\sin(v) \Big) \\
&= \big((R-r\cos(v))\cos(u),(R-r\cos(v))\sin(u),r\sin(v)\big), 
\end{align*}

which is precisely the parametrisation of {numref}`Torus`. 

On the other hand, if $\gamma$ is a general curve lying in the $xy$ plane, parametrised by arc length, with curvature $\kappa$ (and torsion $\tau=0$), then.
$$
\mathbf{T} = \dot\gamma, \; \mathbf{N} = \frac{1}{\kappa}\ddot\gamma, \text{ and } \mathbf{B}=(0,0,1)
$$