---
kernelspec:
  name: python3
  display_name: 'Python 3'
---
(klein)=
# Klein bottle crochet

The goal of this section is to design an easily adaptable and structurally precise crochet pattern for a Klein bottle of specified dimensions, by first expressing it as a parametrised canal surface, (c.f. [](#canal)).

## Klein bottle as a canal surface

### Directrix shape

It seems simplest to build the directrix out of straight and circular arc segments as follows:

- Straight segment 1: For main bottle section, from base to beginning of handle.

- Circlular arc 1: Out of top of main bottle section round to form top of handle

- Straight segment 2: For middle of handle (includes self intersection point)

- Circular arc 2: to smooth out join of handle to main bottle directrix

- Straight segment 3: Down to base of bottle. 

The main equations to find are for straight segment 2 and the two circular arcs. 

For simplicity, assume Straight segment 1 lies on the $z$ axis, and fix the origin as the join of Circular arc 2 with Straight segment 3. The base point is at height $d<0$ on the negative $z$-axis. 

:::{image} figs/Klein-directrix.png
:width: 600
:::

Using the notation in the diagram, 

$$
P=(a+a\cos\theta,H-a\sin\theta), \;\;Q=(b-b\cos\theta,d+b\sin\theta)
$$

Write $z=my+c$ for the equation of $L$. Then 
$$
m = \cot\theta = \frac{d+b\sin\theta - H + a\sin\theta}{b-b\cos\theta - a-a\cos\theta}
$$

which rearranges to give the following contraint on our choice of $h$, $a$ and $b$:
:::{math}
:enumerated: true
:label: hab-constraint
(H-d)\sin\theta=a+b+(a-b)\cos\theta.
:::
For the $z$-intercept $c$, using coordinates of $Q$,
$$
c = d+b\sin\theta - \cot\theta(b-b\cos\theta) =d+b(\csc\theta-\cot\theta).
$$  

On the other hand, using coordinates of $P$,
$$
c=H-a\sin\theta-\cot\theta(a+a\cos\theta) = H-a(\csc\theta+\cot\theta).
$$

So $H-a(\csc\theta+\cot\theta)=d+b(\csc\theta-\cot\theta)$, which rearranges to give [](#hab-constraint).

Hence $L$ has Cartesian equation $z=(\cot\theta)y+d+b(\csc\theta-\cot\theta)$.

Finally, note that
$$
\begin{aligned}
\left\Vert \overrightarrow{PQ}\right\Vert &= \sqrt{\left(b-a-(a+b)\cos\theta\right)^2+\left((a+b)\sin\theta+d-H\right)^2} \\
&= \sqrt{ (b-a)^2+2(a-b)(a+b)\cos\theta +(a+b)^2-2(a+b)h\sin\theta+(H-d)^2} \\
&= \sqrt{ (b-a)^2 -(a+b)^2+(H-d)^2} \\
&= \sqrt{(H-d)^2-4ab}
\end{aligned}
$$
where we have applied [](#hab-constraint) to the second line. 

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
\gamma_3(t)=\frac{t}{\left\Vert\overrightarrow{PQ}\right\Vert}Q + \left(1-\frac{t}{\left\Vert\overrightarrow{PQ}\right\Vert}\right)P,\hspace{1em} t\in\left[0,\left\Vert\overrightarrow{PQ}\right\Vert\right]
$$

- Circular arc 2: 
$$\gamma_4(t)=\left(0, b+b\cos\left(\pi-\theta+\frac{t}{b}\right), b\sin\left(\pi-\theta+\frac{t}{b}\right)\right),\hspace{1em} t\in[0,b\theta]
$$

- Straight segment 3: 
$$
\gamma_5(t)=\left(0, 0, -t\right) ,\hspace{1em} t\in[0,-d]
$$

### Radius function
:::{figure} figs/klein-radius-fn.png
:width: 500

Radius function sketch
:::

Base: Half ellipse with centre $r=\frac{e+p}{2}$. Note $\dot r$ has a singularity at $z=d$. This means we can't use the canal surface parametrisation; however we can easily model it as the lower half of a (stretched) torus:
$$
\left\{
    \begin{array}{rcl}
    x &=& \left(\frac{e+p}{2}-\frac{e-p}{2}\cos(u)\right)\cos(v) \\
    y &=& \left(\frac{e+p}{2}-\frac{e-p}{2}\cos(u)\right)\sin(v) \\
    z &=& d\sin(u)
    \end{array}
    \right.
$$
In the $(r,z)$ plane, the base has equation  
$$
\left(\frac{r-\frac{1}{2}(e+p)}{\frac{1}{2}(e-p)}\right)^2+\frac{z}{d^2}=1, \hspace{1em} d\leq z\leq 0,
$$
and at $(e,0)$ meets the circular segment
$$
r^2+(z-d)^2=e^2, \hspace{1em} d\leq z\leq d+e\sin\phi.
$$
We would like to choose $d$ so that the curvature of these two sections of curve agree at their meeting point $(e,0)$. The circular section has constant curvature equal to $\frac{1}{e}$. 

The curvature of an ellipse with equation $x=A\cos(t)$, $y=B\sin(t)$ is given by the formula 
$$
\kappa=\frac{AB}{\left(A^2\sin^2(t)+B^2\cos^2(t)\right)^\frac{3}{2}}.
$$Applying this with $A=\frac{1}{2}(e-p)$, $B=d$ and $t=0$ tells us that the half-ellipse has curvature $\kappa=\frac{A}{B^2} = \frac{e-p}{2d^2}$ at $(e,0$). In other words, we should choose $d$ so that $
\frac{e-p}{2d^2}=\frac{1}{e}$. So, we take 
:::{math}
:enumerated: true
:label:de-constraint
d := \sqrt{\frac{e(e-p)}{2}}.
:::

Middle circular section: 
$$
r=\sqrt{e^2-(z-d)^2} \hspace{1em} d\leq z\leq d+e\sin\phi
$$

Linear section: we have end points $X=(e\cos\phi,d+e\sin\phi)$, $Y=(p+c-c\cos\phi,H-c\sin\phi)$, so line through $X$ and $Y$ has equation $z=(-\cot\phi)r +k$, where
$$
k=d+e\sin\phi+(\cot\phi)e\cos\phi=d+e\csc\phi.
$$
On the other hand
$$
k=H-c\sin\phi+(\cot\phi)(p+c-c\cos\phi) = H+(c+p)\cot\phi-c\csc\phi
$$
So we have the following constraint on our choice of new parameters $p$, $e$ and $\phi$:
$$
e = (H-d)\sin\phi+(c+p)\cos\phi-c
$$
or in other words,
:::{math}
:enumerated: true
:label:pephi-constraint
c = \frac{(H-d)\sin\phi+p\cos\phi-e}{1-\cos\phi}
:::

Linear section equation:
$$
r=(d+e\csc\phi-z)\tan\phi=d\tan\phi+e\sec\phi-z\tan\phi,
$$
for $d+e\sin\phi\leq z\leq H-c\sin\phi$.

Final circular section: subject to [](#pephi-constraint), 
$$
r=p+c-\sqrt{c^2-(z-H)^2}, \hspace{1em}z\in[H-c\sin\phi,H]
$$

Parametrisation:
- $r_1(u)=\sqrt{e^2-(u-d)^2}$, $\dot r_1(u)=-\frac{u-d}{\sqrt{e^2-(u-d)^2}}$, for $u\in[d,d+e\sin\phi]$.
- $r_2(u)=d\tan\phi+e\sec\phi-u\tan\phi$, $\dot r_2(u)=-\tan\phi$, for $u\in[d+e\sin\phi,H-c\sin\phi]$.
- $r_3(u)=p+c-\sqrt{c^2-(u-d-h)^2}$, $\dot r_3(u)=\frac{u-d-h}{\sqrt{c^2-(u-d-h)^2}}$ for $u\in[H-c\sin\phi,H]$.

For canal surface, it is required that $|\dot r|<\Vert\dot\gamma\Vert=1$, The smaller $|\dot r|$, the smoother (less lumpy) the resulting surface.

### Implementation with matplotlib

::::{figure}
:::{code-cell} python
:label: klein-canal
:tags: remove-input

import numpy as np
import matplotlib.pyplot as plt

from matplotlib import cm
#%matplotlib ipympl


#Rounding
dp = 2

#Bottle parameters
p = 1
a = 3.4
b = 4
theta = .7
phi = .45
w = 8.6
e = w/2
d = np.sqrt((w/2)*((w/2)-p)/2)
H = (a+b+(a-b)*np.cos(theta))/np.sin(theta)+d
c = ((H-d)*np.sin(phi)+p*np.cos(phi)-(w/2))/(1-np.cos(phi))

P = np.array([0,a*(1+np.cos(theta)),H-a*np.sin(theta)])
Q = np.array([0,b*(1-np.cos(theta)),d+b*np.sin(theta)])
PQ = Q-P
magPQ = np.sqrt(PQ[1]**2+PQ[2]**2)
PQhat = PQ/magPQ

X = np.array([0,(w/2)*np.cos(phi),d+(w/2)*np.sin(phi)])
Y = np.array([0,p+c-c*np.cos(phi),H-d-c*np.sin(phi)])


t = np.array([
    0,
    d, 
    (w/2)*np.sin(phi)+d, 
    H-c*np.sin(phi), 
    H,
    H+a*(np.pi+theta), 
    H+a*(np.pi+theta)+magPQ, 
    H+a*(np.pi+theta)+magPQ+b*theta,
    H+d+a*(np.pi+theta)+magPQ+b*theta
])

u = np.linspace(0, np.max(t), 100)

ut = [0]*(len(t)-1)

for j in range(len(t)-1):
    ut[j] = int(np.max(np.argwhere(u<t[j+1])))


usplit = [u[:ut[0]],
          u[ut[0]:ut[1]+1],
          u[ut[1]:ut[2]+1],
          u[ut[2]:ut[3]+1],
          u[ut[3]:ut[4]+1],
          u[ut[4]:ut[5]+1],
          u[ut[5]:ut[6]+1],
          u[ut[6]:ut[7]+1]]

for j in range(len(ut)): #to remove annoying gaps
    l=len(usplit[j])
    usplit[j][l-1] = t[j+1]


# Set up a figure and axes
fig = plt.figure(figsize=plt.figaspect(.5))
fig.suptitle('Klein bottle as canal surface')

ax2 = fig.add_subplot(1, 2, 1)
ax1 = fig.add_subplot(1, 2, 2, projection='3d')

ax2.set_title('Directrix')
ax1.set_title('Surface')

#Base 1
#Not canal
u = usplit[0]
r = (p+e)/2+((e-p)/2)*np.sqrt(1-((u-d)/d)**2)
gamma = (0,0,u)
T = (0,0,1)
N = (0,1,0)
B = (1,0,0)
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x = gamma[0] + r*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y = gamma[1] + r*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z = gamma[2] + r*(N[2]*np.cos(v)+B[2]*np.sin(v))

ax1.plot_surface(x, y, z, edgecolor = 'black', linewidth = .1, alpha = .25)
ax2.plot(0*gamma[2],gamma[2])

#Bottle canal 1
u = usplit[1]
r = np.sqrt((w/2)**2-(u-d)**2)
rdot = -(u-d)/np.sqrt((w/2)**2-(u-d)**2)
gamma = (0,0,u)
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x = gamma[0] - r*rdot*T[0] + r*np.sqrt(1-(rdot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y = gamma[1] - r*rdot*T[1] + r*np.sqrt(1-(rdot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z = gamma[2] - r*rdot*T[2] + r*np.sqrt(1-(rdot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v))

ax1.plot_surface(x, y, z, edgecolor = 'black', linewidth = .1, alpha = .25)
ax2.plot(0*gamma[2],gamma[2])

#Bottle canal 2
u = usplit[2]
r = (w/2)/np.cos(phi)-(u-d)*np.tan(phi)
rdot = -np.tan(phi)
gamma = (0,0,u)
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x = gamma[0] - r*rdot*T[0] + r*np.sqrt(1-(rdot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y = gamma[1] - r*rdot*T[1] + r*np.sqrt(1-(rdot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z = gamma[2] - r*rdot*T[2] + r*np.sqrt(1-(rdot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v))

ax1.plot_surface(x, y, z, edgecolor = 'black', linewidth = .1, alpha = .25)
ax2.plot(0*gamma[2],gamma[2])


#Bottle canal 3
u = usplit[3]
r = p+c-np.sqrt(c**2-(u-t[4])**2)
rdot = (u-t[4])/np.sqrt(c**2-(u-t[4])**2)
gamma = (0,0,u)
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x = gamma[0] - r*rdot*T[0] + r*np.sqrt(1-(rdot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y = gamma[1] - r*rdot*T[1] + r*np.sqrt(1-(rdot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z = gamma[2] - r*rdot*T[2] + r*np.sqrt(1-(rdot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v))

ax1.plot_surface(x, y, z, edgecolor = 'black', linewidth = .1, alpha = .25)
ax2.plot(0*gamma[2],gamma[2])

#Handle 4
u = usplit[4]
r = p
rdot = 0
gamma = (0, a*(1+np.cos(np.pi-(u-t[4])/a)), d+h+a*np.sin(np.pi-(u-t[4])/a))
T = (0,np.sin(np.pi-(u-t[4])/a),-np.cos(np.pi-(u-t[4])/a))
N = (0,-np.cos(np.pi-(u-t[4])/a),-np.sin(np.pi-(u-t[4])/a))
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x = gamma[0] - r*rdot*T[0] + r*np.sqrt(1-(rdot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y = gamma[1] - r*rdot*T[1] + r*np.sqrt(1-(rdot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z = gamma[2] - r*rdot*T[2] + r*np.sqrt(1-(rdot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v))

ax1.plot_surface(x, y, z, edgecolor = 'black', linewidth = .1, alpha = .25)
ax2.plot(gamma[1],gamma[2])

#Handle 5
u = usplit[5]
gamma = (0,((u-t[5])/magPQ)*Q[1] + (1-(u-t[5])/magPQ)*P[1],((u-t[5])/magPQ)*Q[2] + (1-(u-t[5])/magPQ)*P[2])
T = (0,PQhat[1],PQhat[2])
N = (0,PQhat[2],-PQhat[1])
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x = gamma[0] - r*rdot*T[0] + r*np.sqrt(1-(rdot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y = gamma[1] - r*rdot*T[1] + r*np.sqrt(1-(rdot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z = gamma[2] - r*rdot*T[2] + r*np.sqrt(1-(rdot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v))

ax1.plot_surface(x, y, z, edgecolor = 'black', linewidth = .1, alpha = .25)
ax2.plot(gamma[1],gamma[2])

#Handle 6
u = usplit[6]
gamma = (0, b+b*np.cos(np.pi-theta+(u-t[6])/b), d+b*np.sin(np.pi-theta+(u-t[6])/b))
T = (0,-np.sin(np.pi-theta+(u-t[6])/b),np.cos(np.pi-theta+(u-t[6])/b))
N = (0,np.cos(np.pi-theta+(u-t[6])/b),np.sin(np.pi-theta+(u-t[6])/b))
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x = gamma[0] - r*rdot*T[0] + r*np.sqrt(1-(rdot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y = gamma[1] - r*rdot*T[1] + r*np.sqrt(1-(rdot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z = gamma[2] - r*rdot*T[2] + r*np.sqrt(1-(rdot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v))

ax1.plot_surface(x, y, z, edgecolor = 'black', linewidth = .1, alpha = .25)
ax2.plot(gamma[1],gamma[2])

#Base 7
u = usplit[7]
r = (p+e)/2-((e-p)/2)*np.sqrt(1-((u-u[0])/d)**2)
gamma = (0,0,h+2*d+a*(np.pi+theta)+magPQ+b*theta-u)
T = (0,0,-1)
N = (0,-1,0)
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x = gamma[0] + r*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y = gamma[1] + r*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z = gamma[2] + r*(N[2]*np.cos(v)+B[2]*np.sin(v))

ax1.plot_surface(x, y, z, edgecolor = 'black', linewidth = .1, alpha = .25)
ax2.plot(0*u,h+2*d+a*(np.pi+theta)+magPQ+b*theta-u)

ax1.set_aspect('equal')
ax1.view_init(elev=0, azim=0, roll=0)

ax2.set_aspect('equal')

plt.show()

:::
::::

#### Bottle stats

Total height: d+h+a+p={eval}`float(np.round(d+h+a+p,dp))`

Handle:
- Pipe radius: p={eval}`p`
- Circular section 1: Outer $(a+p)(\theta+\pi)=${eval}`float(np.round((a+p)*(theta+np.pi),dp))` , inner $(a+p)(\theta+\pi)=${eval}`float(np.round((a-p)*(theta+np.pi),dp))` 
- Straight section: $\Vert Q-P\Vert=${eval}`float(np.round(magPQ,dp))`
- Circular section 2: Outer $(b+p)\theta=${eval}`float(np.round((b+p)*theta,dp))`, inner $(b-p)\theta=${eval}`float(np.round((b-p)*theta,dp))`  

Base: 
- Stretched half-torus, cross section half-ellipse with height $d=${eval}`float(np.round(d,dp))` and width $e-p=${eval}`float(np.round(w/2-p,dp))`  

Rest of main bottle: 
- Height h={eval}`float(np.round(h,dp))`
- Canal surface with directrix $\gamma(u)=(0,0,u)$, and Frenet-Serret apparatus $\mathbf{T}=(0,0,1)$, $\mathbf{N}=(0,1,0)$, $\mathbf{B}=(1,0,0)$. Parametrisation:
$$
\mathbf{x}(u,v)=\left(\begin{array}{c} x \\ y \\ z\end{array}\right) =\gamma(u) -r\dot r\mathbf{T} +r\sqrt{1-\dot r^2}(\mathbf{N}\cos(v)+\mathbf{B}\sin(v)) 
$$

So
$$
\left\{\begin{array}{rcl} x&=&r\sqrt{1-\dot r^2}\sin(v) \\ y&=&r\sqrt{1-\dot r^2}\cos(v) \\ z&=&u-r\dot r\end{array}\right.
$$

Moreover,

$$
r(u)=\left\{\begin{array}{lr} \sqrt{e^2-(u-d)^2} & \text{ for } d\leq u\leq d+e\sin\phi \\
d\tan\phi+e\sec\phi-u\tan\phi & \text{ for } d+e\sin\phi\leq u\leq d+h-c\sin\phi \\
p+c-\sqrt{c^2-(u-d-h)^2} & \text{ for } d+h-c\sin\phi\leq u\leq h.
\end{array}\right.
$$

and 

$$
\dot r(u)=\left\{\begin{array}{lr} -\frac{u}{\sqrt{e^2-(u-d)^2}} & \text{ for } d\leq u\leq d+e\sin\phi \\
-\tan\phi & \text{ for } d+e\sin\phi\leq u\leq d+h-c\sin\phi \\
\frac{u-d-h}{\sqrt{c^2-(u-d-h)^2}} & \text{ for } d+h-c\sin\phi\leq u\leq h.
\end{array}\right.
$$


Circumference at height $z=u-r\dot r$ is 
$$
2\pi r\sqrt{1-\dot r^2}.
$$ 

#### Canal surface vs surface of revolution

A canal surface with directrix the $z$ axis is a surface of revolution, but not of the radius function.

To illustrate this, suppose we have unit speed directrix $\gamma(u)=(0,0,u)$ and Frenet-Serret apparatus $\mathbf{T}=(0,0,1)$, $\mathbf{N}=(0,1,0)$, $\mathbf{B}=(1,0,0)$. The canal parametrisation [](#eq:canal-param) simplifies to
$$
\begin{aligned}
\mathbf{x}(u,v) &= \gamma-r\dot{r}\mathbf{T} +r\sqrt{1-\dot{r}^2}\Big( \mathbf{N}\cos(v)+\mathbf{B}\sin(v)\Big) \\
&= \left(r\sqrt{1-\dot{r}^2}\sin(v),r\sqrt{1-\dot{r}^2}\cos(v),u-r\dot{r}\right)
\end{aligned}
$$

In contrast, the surface obtained by rotating $z=r(z)$ around the $z$ axis has parametrisation
$$
\mathbf{x}(u,v) = \left(r\sin(v),r\cos(v),u\right).
$$
Importantly, this is *not* an envelope of spheres, and does not satisfy the usual envelope condition [](#eq:envelope-cond) for a canal surface. 

Below is a comparison of the two parametrisations, when used for the main body of the Klein bottle.
::::{figure}
:::{code-cell} python
:label: canal-vs-surf-of-ref
:tags: remove-input

import numpy as np
import matplotlib.pyplot as plt

import numpy as np
import matplotlib.pyplot as plt
from matplotlib import cm
#%matplotlib ipympl


#Rounding
dp = 2

#Bottle parameters
p = 1
a = 3.4
b = 4
theta = .7
phi = .45
w = 8.6
#e = w/2
#print('e =', e)
h = (a+b+(a-b)*np.cos(theta))/np.sin(theta)
#print('h =', h)
d = np.sqrt((w/2)*((w/2)-p)/2)
#print('d =', d)
c = (h*np.sin(phi)+p*np.cos(phi)-(w/2))/(1-np.cos(phi))
#print('c =',c)

P = np.array([0,a*(1+np.cos(theta)),d+h-a*np.sin(theta)])
Q = np.array([0,b*(1-np.cos(theta)),d+b*np.sin(theta)])
PQ = Q-P
magPQ = np.sqrt(PQ[1]**2+PQ[2]**2)
PQhat = PQ/magPQ

X = np.array([0,(w/2)*np.cos(phi),d+(w/2)*np.sin(phi)])
Y = np.array([0,p+c-c*np.cos(phi),h-c*np.sin(phi)])

# Set up a figure and axes
fig = plt.figure(figsize=plt.figaspect(.5))
fig.suptitle('Comparative Klein bottle parametrisations')

ax1 = fig.add_subplot(1, 2, 1, projection='3d')
ax2 = fig.add_subplot(1, 2, 2, projection='3d')

t = np.array([
    0,
    d, 
    (w/2)*np.sin(phi)+d, 
    H-c*np.sin(phi), 
    H, 
    H+a*(np.pi+theta), 
    H+a*(np.pi+theta)+magPQ, 
    H+a*(np.pi+theta)+magPQ+b*theta
])

u = np.linspace(0, H+a*(np.pi+theta)+magPQ+b*theta, 100)

d_index = int(np.max(np.argwhere(u<=d)))
ubase = u[:d_index]
urest = u[d_index:]

#Plot base (not canal)
u = np.linspace(0,np.pi,2*d_index)
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x = (((w/2)+p)/2-(((w/2)-p)/2)*np.cos(u))*np.cos(v)
y = (((w/2)+p)/2-(((w/2)-p)/2)*np.cos(u))*np.sin(v)
z = d-d*np.sin(u)
ax1.plot_surface(x, y, z, color = 'blue', edgecolor = 'black', linewidth = .1, alpha = .25)
ax2.plot_surface(x, y, z, color = 'blue', edgecolor = 'black', linewidth = .1, alpha = .25)

#Rest of bottle
u = urest
l = len(u)
gamma = np.zeros((3,l))
T = np.zeros((3,l))
N = np.zeros((3,l))
B = np.zeros((3,l))
r = np.zeros(l)
rdot = np.zeros(l)

for j in range(l):
    if u[j] < t[4]:
        #Diretrix
        gamma[:,j] = (0,0,u[j])
        #ON frame
        T[:,j] = (0,0,1)
        N[:,j] = (0,-1,0)
        B[:,j] = (1,0,0)
        #Radius function
        if u[j] <= t[2]:
            r[j] = np.sqrt((w/2)**2-(u[j]-d)**2)
            rdot[j] = -(u[j]-d)/np.sqrt((w/2)**2-(u[j]-d)**2)
        elif t[2] < u[j] <= t[3]:
            r[j] = (w/2)/np.cos(phi)-(u[j]-d)*np.tan(phi)
            rdot[j] = -np.tan(phi)
        elif t[3] < u[j] <= t[4]:
            r[j] = p+c-np.sqrt(c**2-(u[j]-t[4])**2)
            rdot[j] = (u[j]-t[4])/np.sqrt(c**2-(u[j]-t[4])**2)
    elif u[j] >= t[4]:
        #Handle
        #Radius function
        r[j] = p
        rdot[j] = 0            
        if u[j] <= t[5]:
            #Directrix
            gamma[:,j] = (0, a*(1+np.cos(np.pi-(u[j]-t[4])/a)), d+h+a*np.sin(np.pi-(u[j]-t[4])/a))
            #ON frame
            T[:,j] = (0,np.sin(np.pi-(u[j]-t[4])/a),-np.cos(np.pi-(u[j]-t[4])/a))
            N[:,j] = (0,np.cos(np.pi-(u[j]-t[4])/a),np.sin(np.pi-(u[j]-t[4])/a))
            B[:,j] = (1,0,0)
        elif t[5] < u[j] <= t[6]:
            #Directrix
            gamma[:,j] = ((u[j]-t[5])/magPQ)*Q + (1-(u[j]-t[5])/magPQ)*P
            #ON frame
            T[:,j] = (0,PQhat[1],PQhat[2])
            N[:,j] = (0,-PQhat[2],PQhat[1])
            B[:,j] = (1,0,0)
        elif t[6] < u[j] <= t[7]:
            #Directrix
            gamma[:,j] = (0, b+b*np.cos(np.pi-theta+(u[j]-t[6])/b), d+b*np.sin(np.pi-theta+(u[j]-t[6])/b))
            #ON frame
            T[:,j] = (0,-np.sin(np.pi-theta+(u[j]-t[6])/b),np.cos(np.pi-theta+(u[j]-t[6])/b))
            N[:,j] = (0,-np.cos(np.pi-theta+(u[j]-t[6])/b),-np.sin(np.pi-theta+(u[j]-t[6])/b))
            B[:,j] = (1,0,0)

#Surface of revolution vs canal surface - plots
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x = gamma[0] - r*rdot*T[0] + r*np.sqrt(1-(rdot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y = gamma[1] - r*rdot*T[1] + r*np.sqrt(1-(rdot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z = gamma[2] - r*rdot*T[2] + r*np.sqrt(1-(rdot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v)) 

ax1.plot_surface(x, y, z, color = 'blue', edgecolor = 'black', linewidth = .1, alpha = .25)
ax1.set_title('Canal surface')

x = gamma[0] + r*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y = gamma[1] + r*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z = gamma[2] + r*(N[2]*np.cos(v)+B[2]*np.sin(v)) 

ax2.plot_surface(x, y, z, color = 'blue', edgecolor = 'black', linewidth = .1, alpha = .25)

ax2.set_title('Surface of revolution + pipe')

# Axis labels
ax1.set_xlabel('y')
ax1.set_ylabel('z')

ax2.set_xlabel('x')
ax2.set_ylabel('y')
ax2.set_zlabel('z')

ax1.set_aspect('equal')
ax2.set_aspect('equal')
ax1.view_init(elev=0, azim=0, roll=0)
ax2.view_init(elev=0, azim=0, roll=0)

plt.show()
:::

Left: Klein bottle produced using canal surface parametrisation; Right: Klein bottle produced using combination of surface of revolution (main body) and pipe surface (handle).
::::

## Towards crochetability
The first problem to solve is to decide how to measure rows. The most accurate would be to measure them along meridians of constant $v$, according to the canal surface parametrisation.

The line element/Riemannian metrix for a parametrised surface $\mathbf{x}(u,v)$ is
$$
ds^2 = Edu^2 + 2Fdudv + Gdv^2,
$$
where coefficients $E=\mathbf{x}_u^2$, $F=\mathbf{x}u\cdot\mathbf{x}_v$ and $F=\mathbf{x}_v^2$. We should calculate $E$ for row count, as this measures distances along constant $v$-meridians.

### Main body
For the main body of the Klein bottle, we have unit speed directrix 
$$
\gamma(u)=(0,0,u), \hspace{1em} \text{ for } 0\leq u\leq d+h,
$$
and constant Frenet-Serret apparatus $\mathbf{T}=(0,0,1)$, $\mathbf{N}=(0,1,0)$, $\mathbf{B}=(1,0,0)$ along $\gamma$. Moreover,
:::{math}
:enumerated: true
:label: eq:body
\mathbf{x}(u,v) = \gamma-r\dot{r}\mathbf{T} +r\sqrt{1-\dot{r}^2}\Big( \mathbf{N}\cos(v)+\mathbf{B}\sin(v)\Big),
:::
for $0\leq u\leq d+h$, $0\leq v\leq 2\pi$. The radius $r$ function comes in three pieces:
$$
r(u)=\left\{\begin{array}{lr} \sqrt{e^2-(u-d)^2} & \text{ for } d\leq u\leq d+e\sin\phi \\
d\tan\phi+e\sec\phi-u\tan\phi & \text{ for } d+e\sin\phi\leq u\leq d+h-c\sin\phi \\
p+c-\sqrt{c^2-(u-d-h)^2} & \text{ for } d+h-c\sin\phi\leq u\leq d+h.
\end{array}\right.
$$
Its first and second derivatives are
$$
\dot{r}(u)=\left\{\begin{array}{lr} -\frac{u-d}{\sqrt{e^2-(u-d)^2}} & \text{ for } d\leq u<d+e\sin\phi \\
-\tan\phi & \text{ for } d+e\sin\phi\leq u<d+h-c\sin\phi \\
\frac{u-d-h}{\sqrt{c^2-(u-d-h)^2}} & \text{ for } d+h-c\sin\phi\leq u\leq d+h,
\end{array}\right.
$$
and
$$
\ddot{r}(u)=\left\{\begin{array}{lr} -e^2\left(e^2-(u-d)^2\right)^{-\frac{3}{2}} & \text{ for } d<u<d+e\sin\phi \\
0 & \text{ for } d+e\sin\phi<u<d+h-c\sin\phi \\
c^2\left(c^2-(u-d-h)^2\right)^{-\frac{3}{2}} & \text{ for } d+h-c\sin\phi<u<d+h.
\end{array}\right.
$$
Observe that $\dot{r}$ is well-defined and continuous at the boundary points where its formula changes, but is not differentiable there.

Now,
so,
$$
\begin{aligned}
\mathbf{x}_u  &= \left(1-r\ddot{r}-\dot{r}^2\right)\mathbf{T} + \left(\dot{r}\sqrt{1-{\dot r}^2}-\frac{r\dot{r}\ddot{r}}{\sqrt{1-\dot{r}^2}}\right)\cos(v)\mathbf{N} \\
&\hspace{8em}+ \left(\dot{r}\sqrt{1-{\dot r}^2}-\frac{r\dot{r}\ddot{r}}{\sqrt{1-\dot{r}^2}}\right)\sin(v)\mathbf{B},
\\[1em]
&= \left(1-r\ddot{r}-\dot{r}^2\right)\mathbf{T} + \frac{\dot{r}\left(1-{\dot r}^2-r\ddot{r}\right)}{\sqrt{1-\dot{r}^2}}\cos(v)\mathbf{N} \\
&\hspace{8em}+ \frac{\dot{r}\left(1-{\dot r}^2-r\ddot{r}\right)}{\sqrt{1-\dot{r}^2}}\sin(v)\mathbf{B},
\end{aligned}
$$
and
$$
\mathbf{x}_v &= -r\sqrt{1-\dot{r}^2}\sin(v)\mathbf{N} + r\sqrt{1-\dot{r}^2}\cos(v)\mathbf{B}.
$$
So:
\begin{gather}
E = \mathbf{x}_u^2  = \left(1-r\ddot{r}-\dot{r}^2\right)^2 + \frac{\dot{r}^2(1-\dot{r}^2-r\ddot{r})^2}{1-\dot{r}^2},\\[1em]
F = \mathbb{x}_u\cdot\mathbb{x}_v = 0, \hspace{2em} G = \mathbb{x}_v^2 = r^2\left(1-\dot{r}^2\right)
\end{gather}

This is another example of a Clairaut parametrisation, where the Riemannian metric/line element
$$
ds^2 = Edu^2 + Gdv^2
$$
depends only on $u$, not on $v$. In particular, if we hold $v$ constant and move along the surface, lengths $ds$ are related to changes in $u$ by
$$
ds = \sqrt{E}du.
$$

Suppose we have crocheted $n$ rows. We'd expect the length of produced fabric to satisfy
$$
nh \approx \sqrt{E}u
$$
(remember $u$ is arc length for the directrix, and begins at $0$). To work out the total number of rows needed we need only evaluate at $u=d+h$ and divide by stitch height.

We can also calculate the $u$-steps required (this will not be constant). Write $0=u_0,u_1,u_2,\ldots$ for the $u$ values associated with rows $n=0,1,2\ldots,N$. Then,
:::{math}
:enumerated: true
:label: eq:un
\text{Stitch height} \approx \sqrt{E|_{u=u_n}}\cdot (u_n-u_{n-1}), \hspace{1em} n=1,2,\ldots,N.
:::
Stitch width and stitch count per row will behave more regularly. On a given row $n$ (i.e. for $u=u_n$), circumference can be calculated two ways:
:::{math}
:enumerated: true
:label: eq:stitch-count
(\text{Stitch width})\cdot(\text{Row $n$ stitch count}) \approx 2\pi r\sqrt{1-\dot{r}^2}.
:::
Assuming a constant crochet gauge, and for a given set of bottle parameters, equations [](#eq:un) and [](#eq:stitch-count) together can be solved numerically to obtain a pattern for the main body.