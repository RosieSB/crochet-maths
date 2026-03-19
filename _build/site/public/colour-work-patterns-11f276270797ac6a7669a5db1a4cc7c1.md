---
kernelspec:
  name: python3
  display_name: 'Python 3'
---

# Colour work patterns

## Torus 7-colouring

According to the [Heawood conjecture](https://en.wikipedia.org/wiki/Heawood_conjecture) (formulated in 1890 by P.J. Heawood and proven in 1968 by Gerhard Ringel and J.W.T. Youngs), the chromatic number of a surface of genus $g$ is

$$
\gamma(g) = \left\lfloor\frac{7+\sqrt{1+48g}}{2}\right\rfloor.
$$

In particular, the torus has chromatic number $\gamma(1)=7$.



:::{code-cell} python
:name: 7-torus-prams
:tag: [remove-input]

import numpy as np

from mpl_toolkits import mplot3d
import matplotlib.colors as mcolors
import matplotlib.pyplot as plt
from matplotlib.colors import TABLEAU_COLORS, same_color
%matplotlib ipympl

R = 2.4
r = .8
w = .4
h = .4
:::

Below is a pattern and colour chart for a 7-coloured torus, created with the following parameters: R={eval}`R`, r={eval}`r`, w={eval}`w`, h={eval}`h`.

:::{code-cell} python
:name: 7-torus-pattern
:tag: [remove-input]


# Pattern instructions
N = round(r * np.pi/h)
st_count = [0]*(int(N)+1)
pattern = [0]*(int(N)+1)
row = [0]*(int(N)+1)
for n in range(int(N)+1):
    st_count[n]=round(2*np.pi*(R-r*np.cos(n*np.pi/int(N)))/w)
    row[n] = n #"Row "+str(n)+": "+str(st_count[n])+" st."


# Colours
c = [
    'gray',
    'xkcd:red',
    'xkcd:magenta',
    'xkcd:blue',
    'xkcd:kelly green',
    'xkcd:lime green',
    'xkcd:yellow',
    'xkcd:tangerine'
]

colours_upper = [0]*(N+1)

# UPPER
for n in range(int(N)+1):
    colours_upper[n] = [c[0] for i in range(st_count[n])]
    for k in range(st_count[n]):
        theta = (n+.5)*np.pi/N
        phi = (k+.5)*2*np.pi/st_count[n] # slight offset to better reflect crochet stitch distribution
        m=-(2/3)*N/st_count[n]
        L1 = m*k+(2/3)*N
        L2 = m*k+(4/3)*N
        L3 = m*k+2*N
        if phi <= np.pi/4:
            if n<=L1:
                colours_upper[n][k] = c[1]
            elif L1<=n<=L2:
                colours_upper[n][k] = c[4]
        elif np.pi/4 <= phi <= 3*np.pi/4:
            if n<=L1:
                colours_upper[n][k] = c[1]
            elif L1<=n<=L2:
                colours_upper[n][k] = c[3]
        elif 3*np.pi/4 <= phi <= np.pi:
            if n<=L1:
                colours_upper[n][k] = c[7]
            elif L1<=n<=L2:
                colours_upper[n][k] = c[3]
        elif np.pi <= phi <= 5*np.pi/4:
            if n<=L1:
                colours_upper[n][k] = c[7]
            elif L1<=n<=L2:
                colours_upper[n][k] = c[2]
            elif L2<=n:
                colours_upper[n][k] = c[5]
        elif 5*np.pi/4 <= phi <= 3*np.pi/2:
            if n<=L1:
                colours_upper[n][k] = c[7]
            elif L1<=n<=L2:
                colours_upper[n][k] = c[2]
            elif L2<=n:
                colours_upper[n][k] = c[4]
        elif 3*np.pi/2 <= phi <= 7*np.pi/4:
            if n<=L1:
                colours_upper[n][k] = c[6]
            elif L1<=n<=L2:
                colours_upper[n][k] = c[2]
            elif L2<=n:
                colours_upper[n][k] = c[4]
        elif 3*np.pi/2 <= phi <= 2*np.pi:
            if n<=L1:
                colours_upper[n][k] = c[6]
            elif L1<=n<=L2:
                colours_upper[n][k] = c[1]
            elif L2<=n:
                colours_upper[n][k] = c[4]

# LOWER
colours_lower = [0]*N
colours_lower[0] = colours_upper[0]
for n in range(1,int(N)):
    colours_lower[n] = ['yellow' for i in range(st_count[n])]
    for k in range(0,st_count[n]):
        phi = (k+.5)*2*np.pi/st_count[n]
        m=(2/3)*N/st_count[n]
        L1 = m*k
        L2 = m*k+(2/3)*N
        L3 = m*k+(4/3)*N
        if phi <= np.pi/4:
            if n<=L1:
                colours_lower[n][k] = c[1]
            elif L1<=n<=L2:
                colours_lower[n][k] = c[6]
            elif L2<=n<=L3:
                colours_lower[n][k] = c[4]
        elif np.pi/4 <= phi <= np.pi/2:
            if n<=L1:
                colours_lower[n][k] = c[1]
            elif L1<=n<=L2:
                colours_lower[n][k] = c[6]
            elif L2<=n<=L3:
                colours_lower[n][k] = c[3]
        elif np.pi/2 <= phi <= 3*np.pi/4:
            if n<=L1:
                colours_lower[n][k] = c[1]
            elif L1<=n<=L2:
                colours_lower[n][k] = c[5]
            elif L2<=n<=L3:
                colours_lower[n][k] = c[3]
        elif 3*np.pi/4 <= phi <= np.pi:
            if n<=L1:
                colours_lower[n][k] = c[7]
            elif L1<=n<=L2:
                colours_lower[n][k] = c[5]
            elif L2<=n<=L3:
                colours_lower[n][k] = c[3]
        elif np.pi <= phi <= 5*np.pi/4:
            if n<=L1:
                colours_lower[n][k] = c[7]
            elif L1<=n<=L2:
                colours_lower[n][k] = c[5]
        elif 5*np.pi/4 <= phi <= 3*np.pi/2:
            if n<=L1:
                colours_lower[n][k] = c[7]
            elif L1<=n<=L2:
                colours_lower[n][k] = c[4]
        elif 3*np.pi/2 <= phi <= 2*np.pi:
            if n<=L1:
                colours_lower[n][k] = c[6]
            elif L1<=n<=L2:
                colours_lower[n][k] = c[4]
:::

### Stitch count table
:::{code-cell} python
:name: 7-torus-table
:tag: [remove-input]

from tabulate import tabulate

table = np.transpose([row, st_count])
pattern = tabulate(table,headers=["Row","Stitch count"],tablefmt="html")
display(pattern)
:::

### Tapestry crochet chart

**Colours:** 

:::{code-cell} python
:name: 7-torus-tapestry-chart
:tag: [remove-input]

fig = plt.figure(figsize = (10,4),label=' ',layout='tight')
ax = plt.axes()

for n in range(N+1):
    for k in range(st_count[n]):
        # upper
        ax.scatter(k*2*np.pi/(st_count[n]-1), n, color=colours_upper[n][k])
        #lower
        if 0<n<N:
            ax.scatter(k*2*np.pi/(st_count[n]-1), -n, color=colours_lower[n][k])
        
# Axis labels 
ax.set_xlabel('Longitude')
ax.set_ylabel('Row')
ax.set_xticks([0, .5*np.pi, np.pi, 1.5*np.pi, 2*np.pi], labels=["$0$", "$\frac{\pi}{2}$", "$\pi$", "$\frac{3}{2}\pi$", "$2\pi$"])
ax.set_aspect(.25)

plt.show()
:::

### 3D model
Use tabs to view from different angles

:::::{tab-set}
::::{tab-item} View 1
:::{code-cell} python
:name: 7-torus-3d-model-1
:tag: [remove-input]
fig = plt.figure(figsize = (10,10),label=' ')
ax = plt.axes(projection='3d')
ax.view_init(elev=30, azim=-60, roll=0)

for n in range(N+1):
    for k in range(st_count[n]):
        # upper
        u = np.linspace((n-.5)*np.pi/N, (n+.5)*np.pi/N, 2)
        v = np.linspace((k-.5)*2*np.pi/st_count[n], (k+.5)*2*np.pi/st_count[n], 2)
        u, v = np.meshgrid(u, v)
        x = (R-r*np.cos(u))*np.cos(v)
        y = (R-r*np.cos(u))*np.sin(v)
        z = r*np.sin(u)
        
        # Highlight row 0
        if n==0:
            lw=.5
        else:
            lw=.1
        ax.plot_surface(x, y, z, color=colours_upper[n][k], edgecolor='black', linewidth=lw)
        #lower
        if 0<n<N:
            u = np.linspace((-n-.5)*np.pi/N, (-n+.5)*np.pi/N, 2)
            v = np.linspace((k-.5)*2*np.pi/st_count[n], (k+.5)*2*np.pi/st_count[n], 2)
            u, v = np.meshgrid(u, v)
            x = (R-r*np.cos(u))*np.cos(v)
            y = (R-r*np.cos(u))*np.sin(v)
            z = r*np.sin(u)
            ax.plot_surface(x, y, z, color=colours_lower[n][k], edgecolor='black', linewidth=lw)
        
# Axis labels for 3d plots
ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_zlabel('z')
ax.set(zlim=(-R-r,R+r))
ax.set_aspect('equal')

plt.show()
:::
::::
::::{tab-item} View 2
:::{code-cell} python
:name: 7-torus-3d-model-2
:tag: [remove-input]
fig = plt.figure(figsize = (10,10),label=' ')
ax = plt.axes(projection='3d')
ax.view_init(elev=30, azim=60, roll=0)

for n in range(N+1):
    for k in range(st_count[n]):
        # upper
        u = np.linspace((n-.5)*np.pi/N, (n+.5)*np.pi/N, 2)
        v = np.linspace((k-.5)*2*np.pi/st_count[n], (k+.5)*2*np.pi/st_count[n], 2)
        u, v = np.meshgrid(u, v)
        x = (R-r*np.cos(u))*np.cos(v)
        y = (R-r*np.cos(u))*np.sin(v)
        z = r*np.sin(u)
        
        # Highlight row 0
        if n==0:
            lw=.5
        else:
            lw=.1
        ax.plot_surface(x, y, z, color=colours_upper[n][k], edgecolor='black', linewidth=lw)
        #lower
        if 0<n<N:
            u = np.linspace((-n-.5)*np.pi/N, (-n+.5)*np.pi/N, 2)
            v = np.linspace((k-.5)*2*np.pi/st_count[n], (k+.5)*2*np.pi/st_count[n], 2)
            u, v = np.meshgrid(u, v)
            x = (R-r*np.cos(u))*np.cos(v)
            y = (R-r*np.cos(u))*np.sin(v)
            z = r*np.sin(u)
            ax.plot_surface(x, y, z, color=colours_lower[n][k], edgecolor='black', linewidth=lw)
        
# Axis labels for 3d plots
ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_zlabel('z')
ax.set(zlim=(-R-r,R+r))
ax.set_aspect('equal')

plt.show()
:::
::::
::::{tab-item} View 3
:::{code-cell} python
:name: 7-torus-3d-model-3
:tag: [remove-input]
fig = plt.figure(figsize = (10,10),label=' ')
ax = plt.axes(projection='3d')
ax.view_init(elev=30, azim=120, roll=0)

for n in range(N+1):
    for k in range(st_count[n]):
        # upper
        u = np.linspace((n-.5)*np.pi/N, (n+.5)*np.pi/N, 2)
        v = np.linspace((k-.5)*2*np.pi/st_count[n], (k+.5)*2*np.pi/st_count[n], 2)
        u, v = np.meshgrid(u, v)
        x = (R-r*np.cos(u))*np.cos(v)
        y = (R-r*np.cos(u))*np.sin(v)
        z = r*np.sin(u)
        
        # Highlight row 0
        if n==0:
            lw=.5
        else:
            lw=.1
        ax.plot_surface(x, y, z, color=colours_upper[n][k], edgecolor='black', linewidth=lw)
        #lower
        if 0<n<N:
            u = np.linspace((-n-.5)*np.pi/N, (-n+.5)*np.pi/N, 2)
            v = np.linspace((k-.5)*2*np.pi/st_count[n], (k+.5)*2*np.pi/st_count[n], 2)
            u, v = np.meshgrid(u, v)
            x = (R-r*np.cos(u))*np.cos(v)
            y = (R-r*np.cos(u))*np.sin(v)
            z = r*np.sin(u)
            ax.plot_surface(x, y, z, color=colours_lower[n][k], edgecolor='black', linewidth=lw)
        
# Axis labels for 3d plots
ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_zlabel('z')
ax.set(zlim=(-R-r,R+r))
ax.set_aspect('equal')

plt.show()
:::
::::
::::{tab-item} View 4
:::{code-cell} python
:name: 7-torus-3d-model-4
:tag: [remove-input]
fig = plt.figure(figsize = (10,10),label=' ')
ax = plt.axes(projection='3d')
ax.view_init(elev=-30, azim=120, roll=180)

for n in range(N+1):
    for k in range(st_count[n]):
        # upper
        u = np.linspace((n-.5)*np.pi/N, (n+.5)*np.pi/N, 2)
        v = np.linspace((k-.5)*2*np.pi/st_count[n], (k+.5)*2*np.pi/st_count[n], 2)
        u, v = np.meshgrid(u, v)
        x = (R-r*np.cos(u))*np.cos(v)
        y = (R-r*np.cos(u))*np.sin(v)
        z = r*np.sin(u)
        
        # Highlight row 0
        if n==0:
            lw=.5
        else:
            lw=.1
        ax.plot_surface(x, y, z, color=colours_upper[n][k], edgecolor='black', linewidth=lw)
        #lower
        if 0<n<N:
            u = np.linspace((-n-.5)*np.pi/N, (-n+.5)*np.pi/N, 2)
            v = np.linspace((k-.5)*2*np.pi/st_count[n], (k+.5)*2*np.pi/st_count[n], 2)
            u, v = np.meshgrid(u, v)
            x = (R-r*np.cos(u))*np.cos(v)
            y = (R-r*np.cos(u))*np.sin(v)
            z = r*np.sin(u)
            ax.plot_surface(x, y, z, color=colours_lower[n][k], edgecolor='black', linewidth=lw)
        
# Axis labels for 3d plots
ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_zlabel('z')
ax.set(zlim=(-R-r,R+r))
ax.set_aspect('equal')

plt.show()
:::
::::
::::{tab-item} View 5
:::{code-cell} python
:name: 7-torus-3d-model-5
:tag: [remove-input]
fig = plt.figure(figsize = (10,10),label=' ')
ax = plt.axes(projection='3d')
ax.view_init(elev=-30, azim=60, roll=180)

for n in range(N+1):
    for k in range(st_count[n]):
        # upper
        u = np.linspace((n-.5)*np.pi/N, (n+.5)*np.pi/N, 2)
        v = np.linspace((k-.5)*2*np.pi/st_count[n], (k+.5)*2*np.pi/st_count[n], 2)
        u, v = np.meshgrid(u, v)
        x = (R-r*np.cos(u))*np.cos(v)
        y = (R-r*np.cos(u))*np.sin(v)
        z = r*np.sin(u)
        
        # Highlight row 0
        if n==0:
            lw=.5
        else:
            lw=.1
        ax.plot_surface(x, y, z, color=colours_upper[n][k], edgecolor='black', linewidth=lw)
        #lower
        if 0<n<N:
            u = np.linspace((-n-.5)*np.pi/N, (-n+.5)*np.pi/N, 2)
            v = np.linspace((k-.5)*2*np.pi/st_count[n], (k+.5)*2*np.pi/st_count[n], 2)
            u, v = np.meshgrid(u, v)
            x = (R-r*np.cos(u))*np.cos(v)
            y = (R-r*np.cos(u))*np.sin(v)
            z = r*np.sin(u)
            ax.plot_surface(x, y, z, color=colours_lower[n][k], edgecolor='black', linewidth=lw)
        
# Axis labels for 3d plots
ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_zlabel('z')
ax.set(zlim=(-R-r,R+r))
ax.set_aspect('equal')

plt.show()
:::
::::
::::{tab-item} View 6
:::{code-cell} python
:name: 7-torus-3d-model-6
:tag: [remove-input]
fig = plt.figure(figsize = (10,10),label=' ')
ax = plt.axes(projection='3d')
ax.view_init(elev=-30, azim=-60, roll=180)

for n in range(N+1):
    for k in range(st_count[n]):
        # upper
        u = np.linspace((n-.5)*np.pi/N, (n+.5)*np.pi/N, 2)
        v = np.linspace((k-.5)*2*np.pi/st_count[n], (k+.5)*2*np.pi/st_count[n], 2)
        u, v = np.meshgrid(u, v)
        x = (R-r*np.cos(u))*np.cos(v)
        y = (R-r*np.cos(u))*np.sin(v)
        z = r*np.sin(u)
        
        # Highlight row 0
        if n==0:
            lw=.5
        else:
            lw=.1
        ax.plot_surface(x, y, z, color=colours_upper[n][k], edgecolor='black', linewidth=lw)
        #lower
        if 0<n<N:
            u = np.linspace((-n-.5)*np.pi/N, (-n+.5)*np.pi/N, 2)
            v = np.linspace((k-.5)*2*np.pi/st_count[n], (k+.5)*2*np.pi/st_count[n], 2)
            u, v = np.meshgrid(u, v)
            x = (R-r*np.cos(u))*np.cos(v)
            y = (R-r*np.cos(u))*np.sin(v)
            z = r*np.sin(u)
            ax.plot_surface(x, y, z, color=colours_lower[n][k], edgecolor='black', linewidth=lw)
        
# Axis labels for 3d plots
ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_zlabel('z')
ax.set(zlim=(-R-r,R+r))
ax.set_aspect('equal')

plt.show()
:::
::::
:::::

## Tonnetz torus


I would like to make a better version of a [tonnetz torus](#fig:Jan26_torus), now I have a pattern I am more satisfied with. This involves mapping the following triangulation onto a torus.

:::{embed} #fig:tonnetz-grid
:::

Below is some code that will do this, along with its output pattern. 

:::{code-cell} python
:label: tonnetz-crochet
:tags: [remove-input]
from tabulate import tabulate

import numpy as np

from mpl_toolkits import mplot3d
import matplotlib.pyplot as plt
from matplotlib.colors import TABLEAU_COLORS, same_color
%matplotlib ipympl

R = 6
r = 2
w = .6
h = .6

# Pattern instructions
N = round(r * np.pi/h)
st_count = [0]*(int(N)+1)
pattern = [0]*(int(N)+1)
row = [0]*(int(N)+1)
for n in range(int(N)+1):
    st_count[n]=round(2*np.pi*(R-r*np.cos(n*np.pi/int(N)))/w)
    row[n] = n #"Row "+str(n)+": "+str(st_count[n])+" st."
:::

Parameter values[^prams] R={eval}`R`, r={eval}`r`, w={eval}`w` and h={eval}`h` have been chosen fairly arbitrarily and may need to change.

[^prams]: C.f. [](#CT) for what these do.

:::{code-cell} python
:label: pattern-table
:tag: [remove-input]
table = np.transpose([row, st_count])
pattern = tabulate(table,headers=["Row","Stitch count"],tablefmt="html")
display(pattern)
:::

:::{code-cell} python
:name: tonnetz-visual
:tag: [remove-input]
fig = plt.figure(figsize = (15,15),label=' ')
ax = plt.axes(projection='3d')
ax.grid()

colours = [0]*(N+1)

for n in range(int(N)+1):
    colours[n] = ['xkcd:emerald' for i in range(st_count[n])]
    colours[0] = ['yellow' for i in range(st_count[0])] # highlight row 0
    for k in range(0,st_count[n]):
        theta = n*np.pi/N
        phi = (k+.5)*2*np.pi/st_count[n] # slight offset to better reflect crochet stitch distribution
        u = np.linspace((n-.5)*np.pi/N, (n+.5)*np.pi/N, 2)
        v = np.linspace((k)*2*np.pi/st_count[n], (k+1)*2*np.pi/st_count[n], 2)
        u, v = np.meshgrid(u, v)
        x = (R-r*np.cos(u))*np.cos(v)
        y = (R-r*np.cos(u))*np.sin(v)
        z = r*np.sin(u)
        tol=.45 
        if n==N or n==round(N/3) or k==0 or k==round(st_count[n]/4) or k==int(st_count[n]/2) or k==round(3*st_count[n]/4):
            colours[n][k] = 'xkcd:light green'
        elif abs(4*phi-3*theta-np.pi)<tol or abs(4*(phi-np.pi/2)-3*theta-np.pi)<tol or abs(4*(phi-np.pi/2)-3*(theta-2*np.pi/3)+np.pi)<tol or abs(4*(phi-np.pi)-3*theta-np.pi)<tol or abs(4*(phi-3*np.pi/2)-3*theta-np.pi)<tol:
            colours[n][k] = 'xkcd:light green'
        ax.plot_surface(x, y, z, color=colours[n][k], edgecolor='black', linewidth=.1)
        if n>0 and n<N:
            ax.plot_surface(x, -y, -z, color=colours[n][k], edgecolor='black', linewidth=.1)
# Axis labels
ax.set_xlabel('x', labelpad=20)
ax.set_ylabel('y', labelpad=20)
ax.set_zlabel('z', labelpad=20)

# Axis limits
ax.set(zlim=(-R-r,R+r))
    
ax.set_aspect('equal')

plt.show() 
:::
