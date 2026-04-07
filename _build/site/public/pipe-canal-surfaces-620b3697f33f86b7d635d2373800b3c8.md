---
kernelspec:
  name: python3
  display_name: 'Python 3'
---

# Canal surfaces and pipe surfaces

## Canal/Channel Surfaces
### Parametrisation

**References:** 
- [Hartman *Geometry and Algorithms for Computer Aided Design*](https://www2.mathematik.tu-darmstadt.de/~ehartmann/cdgen0104.pdf) pp. 117, although there are problems with their mathematical derivations. 
- [Hilbert & Cohn-Vossen *Geometry and the Imagination* (1999)](https://en.wikipedia.org/wiki/Geometry_and_the_Imagination) also mentions them - see page 231.

Let $\gamma:[a,b]\to\mathbb{R}^3$ be a $C^2$ parametrised curve in $\mathbb{R}^3$, and let $r\in C^2([a,b])$. 

The *canal surface* of $\gamma$ with radius function $r$ is, by definition, the envelope of the family of spheres $\{S_u:u\in[a,b]\}$, where for each $u\in[a,b]$, $S_u$ is the sphere of radius $r(u)$ with centre $\gamma(u)$.

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
\end{array}\right.\hspace{.25em}, \hspace{2em} \forall u\in[a,b].
$$
Equation (2) describes a plane perpendicular to $\dot\gamma(u)$, which must intersect the sphere of equation (1) in a circle lying in a plane orthogonal to the curve $\gamma$ at time $u$. 

Let $\mathbf{c}(u)$, $\rho(u)$ denote respectively the centre and radius of this circle. Let $\alpha(u)$ denote the angle between $\mathbf{x}-\gamma(u)$ and $\dot\gamma(u)$. 

:::{figure} figs/canal-diagram.png
:width: 600

Diagram to assist canal surface parametrisation.
:::

$$
\cos(\alpha(u)) = \frac{\vert\dot r(u)\vert}{\Vert\dot\gamma(u)\Vert}=\frac{\sqrt{r(u)^2-\rho(u)^2}}{r(u)}
$$
and so
$$
\sqrt{r(u)^2-\rho(u)^2}=\frac{r(u)\dot r(u)}{\Vert\dot\gamma(u)\Vert}.
$$
In other words, the curves of constant $u$ for the canal surface are circles centred on 
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
\mathbf{x}(u,v) &= \gamma(u)-\frac{r(u)\dot r(u)}{\Vert\dot\gamma(u)\Vert^2}\dot\gamma(u) \\
&\hspace{1.5em}+ r(u)\left(1-\frac{\dot r(u)^2}{\Vert\dot\gamma(u)\Vert^2}\right)^{\frac{1}{2}}\left( \mathbf{N}\cos(v)+\mathbf{B}\sin(v) \right)
\end{aligned}
:::


### Example: Helix
Arc length parametrisation:

$$
\gamma(u) = \left(\begin{array}{c} 
\cos\left(\frac{u}{\sqrt{2}}\right) \\
\sin\left(\frac{u}{\sqrt{2}}\right) \\
\frac{u}{\sqrt{2}}
\end{array}\right), \hspace{2em} u\in\mathbb{R}.
$$

::::{figure}
:align: center
:::{code-cell} python
:label: helix
:tags: remove-input
import numpy as np
import matplotlib.pyplot as plt

fig = plt.figure(figsize = (6,6), label = ' ')
ax = plt.axes(projection='3d')

u = np.linspace(0, 7, 500)
sqrt2 = np.sqrt(2)
x = np.cos(u/sqrt2)
y = np.sin(u/sqrt2)
z = u/sqrt2

ax.plot(x, y, z)

# Axis labels
ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_zlabel('z')

ax.set_aspect('equal')

plt.show()
:::

Parametrised curve $\gamma$, a helix, produced with matplotlib.
::::

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
\begin{align*}
\mathbf{x}(u,v) %&= \left(\begin{array}{c} 
%\cos\left(\frac{u}{\sqrt{2}}\right) \\
%\sin\left(\frac{u}{\sqrt{2}}\right) \\
%\frac{u}{\sqrt{2}}
%\end{array}\right) + r\left(\left(\begin{array}{c} -\cos\left(\frac{u}{\sqrt{2}}\right) \\
%-\sin\left(\frac{u}{\sqrt{2}}\right) \\
%0
%\end{array}\right)\cos(v) \right. \\
%&\hspace{12em} \left.+ \left(\begin{array}{c} 
%\frac{1}{\sqrt{2}}\sin\left(\frac{u}{\sqrt{2}}\right) \\
%-\frac{1}{\sqrt{2}}\cos\left(\frac{u}{\sqrt{2}}\right) \\
%\frac{1}{\sqrt{2}} 
%\end{array}\right)\sin(v)\right) \\
&= \left(\begin{array}{c} 
(1-r\cos(v))\cos\left(\frac{u}{\sqrt{2}}\right) + \frac{r}{\sqrt2}\sin\left(\frac{u}{\sqrt2}\right)\sin(v) \\[.5em]
(1-r\cos(v))\sin\left(\frac{u}{\sqrt{2}}\right)-\frac{r}{\sqrt2}\cos\left(\frac{u}{\sqrt2}\right)\sin(v) \\[.5em]
\frac{u+r\sin(v)}{\sqrt{2}}
\end{array}\right).
\end{align*}

::::{figure}
:::{code-cell} python
:label: helix-pipe
:tags: remove-input

import numpy as np
import matplotlib.pyplot as plt
%matplotlib ipympl

fig = plt.figure(figsize = (10,8), label = ' ')
ax = plt.axes(projection='3d')

u = np.linspace(-7, 7, 100)
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)

sqrt2 = np.sqrt(2)

#Diretrix
gamma = (np.cos(u/sqrt2), np.sin(u/sqrt2), u/sqrt2)

#ON Frame
T = ((-1/sqrt2)*np.sin(u/sqrt2),1/sqrt2*np.cos(u/sqrt2),1/sqrt2)
N = (-np.cos(u/sqrt2),-np.sin(u/sqrt2),0)
B = ((1/sqrt2)*np.sin(u/sqrt2),-(1/sqrt2)*np.cos(u/sqrt2),1/sqrt2)

#Radius function
r = .6

x = gamma[0] + r*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y = gamma[1] + r*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z = gamma[2] + r*(N[2]*np.cos(v)+B[2]*np.sin(v)) 

ax.plot_surface(x, y, z, color = 'blue', edgecolor = 'black', linewidth = .1, alpha = .5)

#Axis labels
ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_zlabel('z')

ax.set_aspect('equal')

plt.show()
:::

Pipe surface on a helix.
::::

### Example: Non-constant radius function
For a non-constant radius example, use the same helix $\gamma$, but this time with 
$$
r = 1-\frac{u}{7}, \;\; -7\leq u\leq 7.
$$ 
Then $\dot r = -\frac{1}{7}. Note we have chosen $r$ so that

- $r\geq 0$ 

- $|\dot r|\leq\frac{1}{4} < 1 =|\dot\gamma|$.
for our range of parameter $-7\leq u\leq 7$.

Substituting into [](#eq:canal-param) gives the following surface.

::::{figure}
:::{code-cell} python
:label: helix-canal
:tags: hide-input
import numpy as np
import matplotlib.pyplot as plt
%matplotlib ipympl

fig = plt.figure(figsize = (10,8), label = ' ')
ax = plt.axes(projection='3d')

u = np.linspace(-7, 7, 100)
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)

sqrt2 = np.sqrt(2)

#Diretrix
gamma = (np.cos(u/sqrt2), np.sin(u/sqrt2), u/sqrt2)

#ON Frame
T = ((-1/sqrt2)*np.sin(u/sqrt2),1/sqrt2*np.cos(u/sqrt2),1/sqrt2)
N = (-np.cos(u/sqrt2),-np.sin(u/sqrt2),0)
B = ((1/sqrt2)*np.sin(u/sqrt2),-(1/sqrt2)*np.cos(u/sqrt2),1/sqrt2)

#Radius function
r = 1-u/7
rdot = -1/7

x = gamma[0] - r*rdot*T[0] + r*np.sqrt(1-(rdot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y = gamma[1] - r*rdot*T[1] + r*np.sqrt(1-(rdot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z = gamma[2] - r*rdot*T[2] + r*np.sqrt(1-(rdot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v)) 

ax.plot_surface(x, y, z, color = 'blue', edgecolor = 'black', linewidth = .1, alpha = .5)

#Axis labels
ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_zlabel('z')

ax.set_aspect('equal')

plt.show()
:::

Helical canal surface with non-constant radius function.
::::

### Example: Circular directrix

This time let us choose a circle lying in the $x$-$y$ plane for the directrix $\gamma$ and another sinusoidal radius function. We continue using arc length parametrisation for $\gamma$ as it simplifies computation of ON frame significantly.

\begin{gather*}
\gamma = \left(R\cos\left(\frac{u}{R}\right), R\sin\left(\frac{u}{R}\right), 0\right), \\
\mathbf{T} = \dot\gamma = \left(-\sin\left(\frac{u}{R}\right), \cos\left(\frac{u}{R}\right), 0\right)
\mathbf{N} = \ddot\gamma = \left(-\cos\left(\frac{u}{R}\right), -\sin\left(\frac{u}{R}\right), 0\right)
\mathbf{B} = (0,0,1)
\end{gather*}

For the radius function, take
\begin{gather*}
r = a+b\sin\left(\frac{cu}{R}\right) \\
rdot = \frac{bc}{R}\cos\left(\frac{cu}{R}\right)
\end{gather*}

:::{code-cell} python
:label: circle-canal-parameters
:tag: remove-input
R = 10
a = 2
b = .8
c = 6
:::

Parameters $R$, $a$, $b$ and $c$ are chosen so as to produce a smooth and not too bulbus result. In this case, $R=${eval}`R`, $a=${eval}`a`, $b=${eval}`b`, $R=${eval}`c`.

::::{figure}
:::{code-cell} python
:label: circle-canal
:tags: hide-input

import numpy as np
import matplotlib.pyplot as plt
%matplotlib ipympl

fig = plt.figure(figsize = (10,8), label = ' ')
ax = plt.axes(projection='3d')

u = np.linspace(0, 2*np.pi*R, 100)
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
  
sqrt2 = np.sqrt(2)

gamma = (R*np.cos(u/R), R*np.sin(u/R), 0)
T = (-np.sin(u/R),np.cos(u/R),0)
N = (-np.cos(u/R),-np.sin(u/R),0)
B = (0,0,1)
r = a+b*np.sin(c*u/R)
rdot = (b*c/R)*np.cos(c*u/R)

x = gamma[0] - r*rdot*T[0] + r*np.sqrt(1-(rdot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y = gamma[1] - r*rdot*T[1] + r*np.sqrt(1-(rdot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z = gamma[2] - r*rdot*T[2] + r*np.sqrt(1-(rdot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v)) 

ax.plot_surface(x, y, z, color = 'blue', edgecolor = 'black', linewidth = .1, alpha = .5)

# Axis labels
ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_zlabel('z')

ax.set_aspect('equal')

plt.show()
:::

Canal surface with sinusoidal radius function and circular directrix.
::::

## Crocheting a pipe surface

This is a more complicated endeaver than that of a torus. One simplification to try first is to assume the directrix curve $\gamma$ is planar, so that torsion, $\tau$, is equal to $0$.

Locally, a pipe surface with (constant) radius $r>0$, planar directrix $\gamma$ and curvature $\kappa$ would coincide with a torus with major radius $\frac{1}{\kappa}$ and minor radius $r$.

:::{dropdown} Click for check
First observe that a torus is itself a pipe surface of a circle with radius $R$. Indeed, using  $\gamma(u) = (R\cos\left(\frac{u}{R}\right),R\sin\left(\frac{u}{R}\right),0)$, $\mathbf{N}=(-\cos\left(\frac{u}{R}\right),-\sin\left(\frac{u}{R}\right),0)$, $\mathbf{B}=(0,0,1)$, and so the parametrisation [](#eq:pipe-param) becomes

\begin{align*}
\mathbf{x}(u,v) &= \left(R\cos\left(\frac{u}{R}\right),R\sin\left(\frac{u}{R}\right),0\right) + r\Big( \left(-\cos\left(\frac{u}{R}\right),-\sin\left(\frac{u}{R}\right),0\right)\cdot\cos(v) \\ 
&\hspace{18em} +(0,0,1)\cdot\sin(v) \Big) \\
&= \left(\left(R-r\cos(v)\right)\cos\left(\frac{u}{R}\right),\left(R-r\cos(v)\right)\sin\left(\frac{u}{R}\right),r\sin(v)\right), 
\end{align*}

which is equivalent to the parametrisation of [](#CT). 

On the other hand, if $\gamma$ is a general planar curve parametrised by arc length, with curvature $\kappa$ (and torsion $\tau=0$). Fix some instant $u\in[0,\infty)$ and rotate/translate coordinate axes so that $\gamma(u)=(R,0,0)$ and $\dot\gamma(u)=(0,1,0)$, where $R=\frac{1}{\kappa}$. We also have orthonormal frame
$$
\mathbf{T} = \dot\gamma, \;\; \mathbf{N} = R\ddot\gamma,\; \text{ and } \;\mathbf{B}=(0,0,1).
$$ 

We don't know what $\ddot\gamma(u)$ is, but since we know that $\mathbf{B}=\mathbf{T}\times\mathbf{N}$, it is necessarily the case that $\mathbf{N} = (-1,0,0)$ at instant $u$. Moreover, since we are using arc length parametrisations and $\tau=0$,
$$
\frac{d\mathbf{T}}{du} = \kappa\mathbf{N}, \; \frac{d^2\mathbf{T}}{du^2}=\kappa\frac{d\mathbf{N}}{du}=-\kappa^2\mathbf{T}, ...
$$
and
$$
\frac{d\mathbf{N}}{du} = -\kappa\mathbf{T}, \; \frac{d^2\mathbf{N}}{du^2}=-\kappa\frac{d\mathbf{T}}{du}=-\kappa^2\mathbf{N}, ...
$$
More generally, for $m\in\mathbb{N}\cup\{0\}$,
\begin{gather*}
\mathbf{T}^{(2m)}(u) = (-1)^m\kappa^{2m}\mathbf{T}(u) = (-1)^m\kappa^{2m}(0,1,0),\\
 \mathbf{T}^{(2m+1)}(u) = (-1)^m\kappa^{2m+1}\mathbf{N}(u) = (-1)^m\kappa^{2m+1}(-1,0,0), \\
\mathbf{N}^{(2m)}(u) = (-1)^m\kappa^{2m}\mathbf{N}(u) = (-1)^m\kappa^{2m}(-1,0,0), \\
\mathbf{N}^{(2m+1)}(u) = (-1)^{m+1}\kappa^{2m+1}\mathbf{T}(u) = (-1)^{m+1}\kappa^{2m+1}(0,1,0).
\end{gather*}

Hence for small changes in $u$,
\begin{align*}
\mathbf{N}(u+\delta u) &= \sum_{j=0}^\infty \frac{(\delta u)^j}{j!}\mathbf{N}^{(j)}(u)\\
&= \mathbf{N}(u)\left(1-(\kappa\cdot\delta u)^2+\ldots\right) + \mathbf{T}(u)\left(-\kappa\cdot\delta u+\frac{1}{3!}(\kappa\cdot\delta u)^3-\ldots\right) \\
&= \cos(\kappa\cdot\delta u)(-1,0,0) - \sin(\kappa\cdot\delta u)(0,1,0) \\
&= \left(-\cos\left(\frac{\delta u}{R}\right),-\sin\left(\frac{\delta u}{R}\right),0\right)
\end{align*}
and
\begin{align*}
\gamma(u+\delta u) &=  \gamma(u) + \delta u\dot\gamma(u) + \frac{(\delta u)^2}{2!}\ddot\gamma(u)+ \ldots\\
&= (R,0,0) + \sum_{j=1}^\infty\frac{(\delta u)^j}{j!}\mathbf{T}^{(j-1)}(u) \\
&= (R,0,0) + \sum_{m=0}^\infty\frac{(\delta u)^{(2m+1)}}{(2m+1)!}\mathbf{T}^{(2m)}(u) + \sum_{m=0}^\infty\frac{(\delta u)^{(2m+2)}}{(2m+2)!}\mathbf{T}^{(2m+1)}(u) \\
%&= (R,0,0) + \sum_{m=0}^\infty\frac{(\delta u)^{(2m+1)}}{(2m+1)!}(-1)^m\kappa^{2m}\mathbf{T}(u) + \sum_{m=0}^\infty\frac{(\delta u)^{(2m+2)}}{(2m+2)!}(-1)^m\kappa^{2m+1}\mathbf{N}(u) \\--->
&= R\sin(\kappa\cdot\delta u)(0,1,0) - \cos(\kappa\cdot\delta u)R(-1,0,0) \\
&= \left(R\cos\left(\frac{\delta u}{R}\right),R\sin\left(\frac{\delta u}{R}\right),0\right).
\end{align*}
So the parametrisation of the pipe surface near $u$ will be

\begin{align*}
\mathbf{x}(u+\delta u,v) &= \gamma(u+\delta u) + r\left(\mathbf{N}(u+\delta u)\cos(v)+(\mathbf{B}(u+\delta u))\sin(v) \right) \\
&= \left(\big(R-r\cos(v)\big)\cos\left(\frac{\delta u}{R}\right),\big(R-r\cos(v)\big)\sin\left(\frac{\delta u}{R}\right),r\sin(v)\right)
\end{align*}

This confirms our suspicion: small deviations in the parameter $u$ give surface sections that coincide precisely with the torus (with radii $R=\frac{1}{\kappa}$ and $r$).
:::

This suggests a method for crocheting pipe (and eventually, canal surfaces): work out how to crochet small sections of pipe of a specified radius and curvature, and then stack them, changing the curvature and radius as needed. For now, we continue to restrict to the torsion-free (i.e. planar directrix) case.

### Curved pipe pattern examples
The following patterns were created by taking small sections of torus, modified from [](#torus-crochet-eg) and similar. 

The sections were chosen so as to line up with a single stitch on the inside of the bend, as this is the smallest possible section of torus that can be crocheted. The turning angle $\theta$ in each case should therefore satisfy 
:::::{grid} 2
::::{grid-item}
:::{image} figs/Pipe-turning-angle.png
:width: 800
:align: center
:::
::::
::::{grid-item}
$$
\theta=2\arcsin\left(\frac{w}{2(R-r)}\right) \approx \frac{w}{R-r}.%w=2(R-r)\sin\left(\frac{\theta}{2}\right) 
$$
::::
:::::
(Here, $w$ denotes stitch width, and $R$, $r$ is the major and minor radii of the torus. )

We can also easily calculate curvature using work from [](#torus-curvature). We had principal curvatures
$$
\kappa_\theta=-\frac{1}{r}, \hspace{1em} \kappa_\phi=\frac{\cos\theta}{R-r\cos\theta},
$$
and Gauss curvature
$$
K = \kappa_\theta\kappa_\phi =  -\frac{\cos\theta}{r(R-r\cos\theta)}.
$$

In more practical terms, $K$ takes all values in the interval
$$
-\frac{1}{r(R-r)}\leq K\leq \frac{1}{r(R+r)},
$$
attaining its minimum (negative) value at the inner most point of the turn ($\theta=0$), and its maximum (positive) at the outer most point of the turn ($\theta=\pi$). See below for concrete evaluation of these formulae in specific cases. 

:::::{tab-set}
::::{tab-item} Example 1
:::{code-cell} python 
:label: curved-pipe-parameters-1
:tags: remove-input
from tabulate import tabulate
import numpy as np
from mpl_toolkits import mplot3d
import matplotlib.pyplot as plt
import matplotlib.colors as mcolors
from matplotlib.colors import TABLEAU_COLORS, same_color
prop_cycle = plt.rcParams['axes.prop_cycle']
colors = list(mcolors.TABLEAU_COLORS)

%matplotlib ipympl



#Major & minor radii
R = 3
r = 2
#Stitch dimensions
w = 1
h = 1

#Curvatures
dp = 2
kappa = float(np.round(1/R,dp))
K_inner = float(np.round(-1/(r*(R-r)),dp))
K_outer = float(np.round(1/(r*(R+r)),dp))


#Turning angle
#np.set_printoptions(legacy='1.25')
theta = float(np.round(2*np.arcsin(w/(2*(R-r))),dp))
:::

Parameter values: $R=${eval}`R`, $r=${eval}`r`, $w=${eval}`w` and $h=${eval}`h`.

Curvature: 
- Of the directrix circle: $\kappa=\frac{1}{R}=${eval}`kappa`. 

- Of pipe: {eval}`K_inner`$\leq K \leq ${eval}`K_outer`.

Turning angle: $\theta = 2\arcsin\left(\frac{w}{2(R-r)}\right)=${eval}`theta`.

:::{code-cell} python
:label: curved-pipe-code-1
:tags: remove-input
# Row count
N = round(r*np.pi/h)

# Stitch count
st_count = [0]*(N+1)
reduced_st_count = [0]*(N+1)
row = [0]*(N+1)
for n in range(N+1):
    st_count[n]=round(2*np.pi*(R-r*np.cos(n*np.pi/int(N)))/w)
    row[n] = n #"Row "+str(n)+": "+str(st_count[n])+" st."


# Pattern model
fig = plt.figure(figsize = (8,8), label = ' ')
ax = plt.axes(projection='3d')
ax.grid()
for n in range(N+1):
    reduced_st_count[n] = round(st_count[n]/st_count[0])
    for k in range(reduced_st_count[n]):
        theta = n*np.pi/N
        phi = k*2*np.pi/st_count[n] # < phi1
        u = np.linspace((n-.5)*np.pi/N, (n+.5)*np.pi/N, 2)
        v = np.linspace((k-.5)*2*np.pi/st_count[n], (k+.5)*2*np.pi/st_count[n], 2)
        u, v = np.meshgrid(u, v)
        x = (R-r*np.cos(u))*np.cos(v)
        y = (R-r*np.cos(u))*np.sin(v)
        z = r*np.sin(u)
        ax.plot_surface(x, y, z, color=colors[k%10], edgecolor='black', linewidth=.1)
        if n>0 and n<N:
            ax.plot_surface(x, y, -z, color=colors[k%10], edgecolor='black', linewidth=.1)


#Dual stitch count table
dual_st_count = [2*N]
r = 0
rnd = [0]
for n in range(1,N+1):
    if reduced_st_count[n]>reduced_st_count[n-1]:
        dual_st_count.append(2*N-(2*n-1))
        r = r + 1
        rnd.append(r)

colour = [0]*(r+1)
for k in range(r+1):
    colour[k] = colors[k%10][4:]
    colour[k] = colour[k].capitalize()

table = np.transpose([rnd, dual_st_count, colour])
pattern = tabulate(table,headers=["Round","Stitch count", "Colour"],tablefmt="html")
display(pattern)

#Axis labels
ax.set_xlabel('x', labelpad=20)
ax.set_ylabel('y', labelpad=20)
ax.set_zlabel('z', labelpad=20)

ax.set_aspect('equal')

ax.view_init(elev=50, azim=-155, roll=0)

plt.show()
:::
::::

::::{tab-item} Example 2
:::{code-cell} python 
:label: curved-pipe-parameters-2
:tags: remove-input
from tabulate import tabulate
import numpy as np
from mpl_toolkits import mplot3d
import matplotlib.pyplot as plt
import matplotlib.colors as mcolors
from matplotlib.colors import TABLEAU_COLORS, same_color
prop_cycle = plt.rcParams['axes.prop_cycle']
colors = list(mcolors.TABLEAU_COLORS)

%matplotlib ipympl



#Major & minor radii
R = 5
r = 2
#Stitch dimensions
w = 1
h = 1

#Curvatures
kappa = float(np.round(1/R,dp))
K_inner = float(np.round(-1/(r*(R-r)),dp))
K_outer = float(np.round(1/(r*(R+r)),dp))

#Turning angle
#np.set_printoptions(legacy='1.25')
theta = float(np.round(2*np.arcsin(w/(2*(R-r))),dp))
:::

Parameter values: $R=${eval}`R`, $r=${eval}`r`, $w=${eval}`w` and $h=${eval}`h`.

Curvature: 
- Of the directrix circle: $\kappa=\frac{1}{R}=${eval}`kappa`. 

- Of pipe: {eval}`K_inner`$\leq K \leq ${eval}`K_outer`.

Turning angle: $\theta = 2\arcsin\left(\frac{w}{2(R-r)}\right)=${eval}`theta`.

:::{code-cell} python
:label: curved-pipe-code-2
:tags: remove-input
# Row count
N = round(r*np.pi/h)

# Stitch count
st_count = [0]*(N+1)
reduced_st_count = [0]*(N+1)
row = [0]*(N+1)
for n in range(N+1):
    st_count[n]=round(2*np.pi*(R-r*np.cos(n*np.pi/int(N)))/w)
    row[n] = n #"Row "+str(n)+": "+str(st_count[n])+" st."


# Pattern model
fig = plt.figure(figsize = (8,8), label = ' ')
ax = plt.axes(projection='3d')
ax.grid()
for n in range(N+1):
    reduced_st_count[n] = round(st_count[n]/st_count[0])
    for k in range(reduced_st_count[n]):
        theta = n*np.pi/N
        phi = k*2*np.pi/st_count[n] # < phi1
        u = np.linspace((n-.5)*np.pi/N, (n+.5)*np.pi/N, 2)
        v = np.linspace((k-.5)*2*np.pi/st_count[n], (k+.5)*2*np.pi/st_count[n], 2)
        u, v = np.meshgrid(u, v)
        x = (R-r*np.cos(u))*np.cos(v)
        y = (R-r*np.cos(u))*np.sin(v)
        z = r*np.sin(u)
        ax.plot_surface(x, y, z, color=colors[k%10], edgecolor='black', linewidth=.1)
        if n>0 and n<N:
            ax.plot_surface(x, y, -z, color=colors[k%10], edgecolor='black', linewidth=.1)


#Dual stitch count table
dual_st_count = [2*N]
r = 0
rnd = [0]
for n in range(1,N+1):
    if reduced_st_count[n]>reduced_st_count[n-1]:
        dual_st_count.append(2*N-(2*n-1))
        r = r + 1
        rnd.append(r)

colour = [0]*(r+1)
for k in range(r+1):
    colour[k] = colors[k%10][4:]
    colour[k] = colour[k].capitalize()

table = np.transpose([rnd, dual_st_count, colour])
pattern = tabulate(table,headers=["Round","Stitch count", "Colour"],tablefmt="html")
display(pattern)

#Axis labels
ax.set_xlabel('x', labelpad=20)
ax.set_ylabel('y', labelpad=20)
ax.set_zlabel('z', labelpad=20)

ax.set_aspect('equal')

ax.view_init(elev=50, azim=-155, roll=0)

plt.show()
:::
::::

::::{tab-item} Example 3
:::{code-cell} python 
:label: curved-pipe-parameters-3
:tags: remove-input
from tabulate import tabulate
import numpy as np
from mpl_toolkits import mplot3d
import matplotlib.pyplot as plt
import matplotlib.colors as mcolors
from matplotlib.colors import TABLEAU_COLORS, same_color
prop_cycle = plt.rcParams['axes.prop_cycle']
colors = list(mcolors.TABLEAU_COLORS)

%matplotlib ipympl



#Major & minor radii
R = 9
r = 4
#Stitch dimensions
w = 1
h = 1

#Curvatures
kappa = float(np.round(1/R,dp))
K_inner = float(np.round(-1/(r*(R-r)),dp))
K_outer = float(np.round(1/(r*(R+r)),dp))

#Turning angle
#np.set_printoptions(legacy='1.25')
theta = float(np.round(2*np.arcsin(w/(2*(R-r))),dp))
:::

Parameter values: $R=${eval}`R`, $r=${eval}`r`, $w=${eval}`w` and $h=${eval}`h`.

Curvature: 
- Of the directrix circle: $\kappa=\frac{1}{R}=${eval}`kappa`. 

- Of pipe: {eval}`K_inner`$\leq K \leq ${eval}`K_outer`.

Turning angle: $\theta = 2\arcsin\left(\frac{w}{2(R-r)}\right)=${eval}`theta`.

:::{code-cell} python
:label: curved-pipe-code-3
:tags: remove-input
# Row count
N = round(r*np.pi/h)

# Stitch count
st_count = [0]*(N+1)
reduced_st_count = [0]*(N+1)
row = [0]*(N+1)
for n in range(N+1):
    st_count[n]=round(2*np.pi*(R-r*np.cos(n*np.pi/int(N)))/w)
    row[n] = n #"Row "+str(n)+": "+str(st_count[n])+" st."


# Pattern model
fig = plt.figure(figsize = (8,8), label = ' ')
ax = plt.axes(projection='3d')
ax.grid()
for n in range(N+1):
    reduced_st_count[n] = round(st_count[n]/st_count[0])
    for k in range(reduced_st_count[n]):
        theta = n*np.pi/N
        phi = k*2*np.pi/st_count[n] # < phi1
        u = np.linspace((n-.5)*np.pi/N, (n+.5)*np.pi/N, 2)
        v = np.linspace((k-.5)*2*np.pi/st_count[n], (k+.5)*2*np.pi/st_count[n], 2)
        u, v = np.meshgrid(u, v)
        x = (R-r*np.cos(u))*np.cos(v)
        y = (R-r*np.cos(u))*np.sin(v)
        z = r*np.sin(u)
        ax.plot_surface(x, y, z, color=colors[k%10], edgecolor='black', linewidth=.1)
        if n>0 and n<N:
            ax.plot_surface(x, y, -z, color=colors[k%10], edgecolor='black', linewidth=.1)


#Dual stitch count table
dual_st_count = [2*N]
r = 0
rnd = [0]
for n in range(1,N+1):
    if reduced_st_count[n]>reduced_st_count[n-1]:
        dual_st_count.append(2*N-(2*n-1))
        r = r + 1
        rnd.append(r)

colour = [0]*(r+1)
for k in range(r+1):
    colour[k] = colors[k%10][4:]
    colour[k] = colour[k].capitalize()

table = np.transpose([rnd, dual_st_count, colour])
pattern = tabulate(table,headers=["Round","Stitch count", "Colour"],tablefmt="html")
display(pattern)

#Axis labels
ax.set_xlabel('x', labelpad=20)
ax.set_ylabel('y', labelpad=20)
ax.set_zlabel('z', labelpad=20)

ax.set_aspect('equal')

ax.view_init(elev=50, azim=-155, roll=0)

plt.show()
:::
::::
:::::

(klein)=
## Klein bottle as a channel surface??

The trick will be choosing the right directrix and radius function, for optimum crochetability. 


### Directrix shape

It seems simplest to build the directrix out of straight and circular arc segments as follows:

- Straight segment 1: For main bottle section, from base to beginning of handle.

- Circlular arc 1: Out of top of main bottle section round to form top of handle

- Straight segment 2: For middle of handle (includes self intersection point)

- Circular arc 2: to smooth out join of handle to main bottle directrix

- Straight segment 3: Down to base of bottle. 

The main equations to find are for straight segment 2 and the two circular arcs. 

For simplicity, assume Straight segment 1 lies on the $z$ axis, and fix the origin as the join of Circular arc 2 with Straight segment 3. The base point is at height $d<0$ on the negative $z$-axis. 

:::{image} figs/Klein-directrix-calc.png
:width: 600
:::

Using the notation in the diagram, 

$$
P=(a+a\cos\theta,h-a\sin\theta), \;\;Q=(b-b\cos\theta,b\sin\theta)
$$

Write $z=my+c$ for the equation of $L$. Then 
$$
m = \cot\theta = \frac{b\sin\theta - h+a\sin\theta}{b-b\cos\theta - a-a\cos\theta}
$$

so
$$
\cos\theta(b-b\cos\theta - a - a\cos\theta) = \sin\theta(b\sin\theta - h + a\sin\theta)
$$
$$
b\cos\theta-b\cos^2\theta - a\cos\theta-a\cos^2\theta = b\sin^2\theta - h\sin\theta +a\sin^2\theta
$$
$$
h\sin\theta = r+s+(r-s)\cos\theta
$$
and hence
$$
h=(a+b)\csc\theta+(a-b)\cot\theta.\tag{1}
$$
For the $z$-intercept $c$, using coordinates of $Q$,
\begin{align*}
c &= b\sin\theta - \cot\theta(b-b\cos\theta) \\
&= b\left(\sin\theta-\frac{\cos\theta}{\sin\theta}+\frac{\cos^2\theta}{\sin\theta}\right) \\
&=b(\csc\theta-\cot\theta).
\end{align*}

On the other hand, using coordinates of $P$,
\begin{align*}
c&=h-a\sin\theta-\cot\theta(a+a\cos\theta)\\
&=h-a\left(\sin\theta+\frac{\cos\theta}{\sin\theta}+\frac{\cos^2\theta}{\sin\theta}\right) \\
&= h-a(\csc\theta+\cot\theta).
\end{align*}

So, 
$$
h-a(\csc\theta+\cot\theta)=b(\csc\theta-\cot\theta),
$$
which rearranges to give $(1)$.

So $L$ has Cartesian equation
$$
L: \; z=(\cot\theta)y+b(\csc\theta-\cot\theta).
$$

(directrix-param)=
#### Arc length parametrisation
We take the concatenation of the following parametric curves.

- Straight segment 1: 
$$
\gamma_1(t)=(0,0,d+t),\hspace{1em} t\in[0,h-d]
$$

- Circlular arc 1: 
$$
\gamma_2(t)=\left(0,a+a\cos\left(\pi-\frac{t}{a}\right), h+a\sin\left(\pi-\frac{t}{a}\right)\right),\hspace{1em} t\in[0,a(\pi+\theta)]
$$

- Straight segment 2: 
$$
\gamma_3(t)=\frac{t}{\Vert Q-P\Vert}Q + \left(1-\frac{t}{\Vert Q-P\Vert}\right)P,\hspace{1em} t\in[0,{\Vert Q-P\Vert}]
$$

- Circular arc 2: 
$$\gamma_4(t)=\left(0, b+b\cos\left(\pi-\theta+\frac{t}{b}\right), b\sin\left(\pi-\theta+\frac{t}{b}\right)\right),\hspace{1em} t\in[0,b\theta]
$$

- Straight segment 3: 
$$
\gamma_5=\left(0, 0, -t\right) ,\hspace{1em} t\in[0,-d]
$$


#### Example
:::{code-cell} python
:label: klein-directrix-eg-params
:tags: remove-input
import numpy as np

d = -.5
a = 1 
b = 2
theta = .7

h = (a+b+(a-b)*np.cos(theta))/np.sin(theta)

P = np.array([0,a*(1+np.cos(theta)),h-a*np.sin(theta)])
Q = np.array([0,b*(1-np.cos(theta)),b*np.sin(theta)])
PQ = Q-P
magPQ = np.sqrt(PQ[1]**2+PQ[2]**2)
:::

Below is an example, made with parameter values d={eval}`d`, a={eval}`a` and b={eval}`b`. From these we get P=(0,{eval}`float(np.round(P[1],dp))`,{eval}`float(np.round(P[2],dp))`), Q=(0,{eval}`float(np.round(Q[1],dp))`,{eval}`float(np.round(Q[2],dp))`) and h={eval}`float(np.round(h,dp))`.

::::{figure}
:label: fig:klein-directrix-eg
:::{code-cell} python
:label: klein-directrix-calc
:tags: hide-input
import numpy as np
import matplotlib.pyplot as plt
import ipywidgets as widgets
from ipywidgets import Layout

import numpy as np
import matplotlib.pyplot as plt

%matplotlib ipympl

plt.rcParams['text.usetex'] = True


fig = plt.figure(figsize = (7,8), label = ' ')
fig.tight_layout()
ax = plt.axes(projection='3d')

#Straight segment 1
u = np.linspace(0, np.pi, 2)

t = np.linspace(0, h-d, 2)

x = 0
y = 0
z = d+t

ax.plot(x, y, z, color='tab:blue')
ax.text(0, -.35*a ,h/2 ,r'$\gamma_1$', color='tab:blue', fontsize=16)

#Circular segment 1

t = np.linspace(0, a*(np.pi+theta), 100)

x = 0
y = a*(1+np.cos(np.pi-t/a))
z = h+a*np.sin(np.pi-t/a)

ax.plot(x, y, z, color='tab:orange')
ax.text(0, 2.05*a ,1.1*h ,r'$\gamma_2$', color='tab:orange', fontsize=16)

#Straight segment 2

t = np.linspace(0, magPQ, 2)

x = (t/magPQ)*Q[0] + (1-t/magPQ)*P[0]
y = (t/magPQ)*Q[1] + (1-t/magPQ)*P[1]
z = (t/magPQ)*Q[2] + (1-t/magPQ)*P[2]

ax.plot(x, y, z, color='tab:green')
ax.text(0, (P[1]+Q[1])/2-.1*a ,(P[2]+Q[2])/2-.3*a ,r'$\gamma_3$', color='tab:green', fontsize=16)

#Circular segment 2

t = np.linspace(0, b*theta, 100)

x = 0
y = b+b*np.cos(np.pi-theta+t/b)
z = b*np.sin(np.pi-theta+t/b)

ax.plot(x, y, z, color='tab:red')
ax.text(0, b+b*np.cos(np.pi-theta/2)+.1*a, b*np.sin(np.pi-theta/2)-.1*a, r'$\gamma_4$', color='tab:red', fontsize=16)

#Straight segment 3

t = np.linspace(0, -d, 2)

x = 0
y = 0
z = -t

ax.plot(x, y, z, color='tab:purple')
ax.text(0, -.35*a, d/2, r'$\gamma_5$', color='tab:purple', fontsize=16)

#Annotations
#ax.scatter(P[0],P[1],P[2],color='k')
#ax.text(P[0],P[1],P[2]-.5*a, "P")

#ax.scatter(Q[0],Q[1],Q[2],color='k')
#ax.text(Q[0],Q[1],Q[2]-.5*a, "Q")

# Axis labels
ax.set_xlabel(' ')
ax.set_ylabel(' ')
ax.set_zlabel('z')

ax.set(xlim=(-2,2), ylim=(-1.5*a,2.5*a), zlim=(1.2*d,1.2*(h+a)))

#Ticks
ax.set_xticks([])


if b<=a:
    ax.set_yticks([0,b,a], labels=["0", str(b), str(a)])
else:
    ax.set_yticks([0,a,b], labels=["0", str(a), str(b)])

ax.set_zticks([d, 0,h,h+a], labels=[str(d), "0", str(float(np.round(h,dp))), str(float(np.round(h+a,dp)))])

ax.set_aspect('equal')

ax.view_init(elev=0, azim=0, roll=0)
:::
::::

### Klein bottle

::::{figure}
:::{code-cell} python
:label: klein-canal-1
:tags: hide-input
import numpy as np

from scipy import special

import matplotlib.pyplot as plt
%matplotlib ipympl

fig = plt.figure(figsize = (7,8), label = ' ')
fig.tight_layout()
ax = plt.axes(projection='3d')

#Straight segment 1

u = np.linspace(0, h-d, 100)
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)

#Diretrix
gamma = (0,0,d+u)

#ON Frame
T = (0,0,1)
N = (1,0,0)
B = (0,1,0)

#Radius function
r = .5+ ( 1+special.erf(8*u/(h-d)-4) )/4
rdot = (4/(h-d))*special.erf(8*u/(h-d)-4)/(np.sqrt(np.pi)*(h-d))

x = gamma[0] - r*rdot*T[0] + r*np.sqrt(1-(rdot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y = gamma[1] - r*rdot*T[1] + r*np.sqrt(1-(rdot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z = gamma[2] - r*rdot*T[2] + r*np.sqrt(1-(rdot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v)) 

ax.plot_surface(x, y, z, color = 'blue', edgecolor = 'black', linewidth = .1, alpha = .5)

#Axis labels
ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_zlabel('z')

ax.set(xlim=(-2,2), ylim=(-1.5*a,2.5*a), zlim=(1.2*d,1.2*(h+a)))

#Ticks
ax.set_xticks([])


if b<=a:
    ax.set_yticks([0,b,a], labels=["0", str(b), str(a)])
else:
    ax.set_yticks([0,a,b], labels=["0", str(a), str(b)])

ax.set_zticks([d, 0,h,h+a], labels=[str(d), "0", str(float(np.round(h,dp))), str(float(np.round(h+a,dp)))])

ax.set_aspect('equal')

ax.view_init(elev=20, azim=0, roll=0)

plt.show()
:::