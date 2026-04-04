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

Substituting into [](#canal-param) gives the following surface.

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

## Example: Circular directrix

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

:::{code-cell}
:label: circle-canal-parameters
:tag: remove-input
R = 10
a = 2
b = .8
c = 6
:::

Parameters $R$, $a$, $b$ and $c$ are chosen so as to produce a smooth and not too bulbus result. In this case, $R=${eval}`R`, $a=${eval}`a`, $b=${eval}`b`, $R=${eval}`c`.

::::{figure}
:::{code-cell}
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

