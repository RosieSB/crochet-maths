---
kernelspec:
  name: python3
  display_name: 'Python 3'
---

# Interactive torus pattern
Consider a torus embedded in $\mathbb{R}^3$, centred on the origin and with major and minor radii $R>r>0$.

:::{figure} figs/torus-diagram.png
:width: 400
:label: fig:torus-diag

Diagram of a torus with radii $R>r>0$.
:::

A corresponding parametrisation is 

\begin{gather*}
\mathbf{r}:[-\pi,\pi]\times[0,2\pi]\to\mathbb{R}^3; \hspace{1em} \mathbf{r}(\theta,\phi)=(x,y,z) \\
x=(R-r\cos\theta)\cos\phi, \hspace{1em} y=(R-r\cos\theta)\sin\phi, \hspace{1em} z=r\sin\theta
\end{gather*}

We measure crochet rows in units of $\theta$, and begin with the upper half ($z\geq 0$, $0\leq\theta\leq\pi$). The lower half ($z<0$) is identical and can be added on after. 

By design, the first row corresponds to $\theta = 0$ and has circumference $2\pi(R-r)$. Assume a constant gauge, with each stitch measuring $w$ wide and $h$ tall. Then, the starting round should have approximately $2\pi(R-r)/w$ stitches.

Let $N$ denote the total number of rows for the upper half, **chosen so as to satisfy $N\approx\frac{r\pi}{h}$**.  

For each $n$, write $C(n)$ for the circumference of row $n$ and write $s(n)$ for the stitch count of row $n$. Then, for $n=0,1,\ldots, N$,
$$
s(n) = \frac{C(n)}{w},
$$
and
$$
C(n)=2\pi\left(R-r\cos\left(\frac{n\pi}{N}\right)\right).
$$

## Pattern 

**Set up:** Chain $\left\lfloor\frac{2\pi(R-r)}{w}\right\rfloor$ stitches and join into a round.

**Row n:** Crochet $\left\lfloor \frac{2\pi}{w}\left(R-r\cos\left(\frac{n\pi}{N}\right)\right)\right\rfloor$ stitches around. Taking care to spread increases as evenly as possible.

### Example

:::{code-cell} python
:label: enter-R
:tags: [remove-input]
from ipywidgets import interact, interactive, fixed, interact_manual
import ipywidgets as widgets
import numpy as np

# Major & minor radii
R = 4

def F(R: float):
    return R

interact(F, R=4)
