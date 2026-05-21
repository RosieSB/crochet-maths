---
kernelspec:
  name: python3
  display_name: 'Python 3'
---
(klein)=
# Klein bottle crochet

(patt_eg)=
## Pattern example

:::{code-cell} Python
:label: klein-pattern
:tag: hide-input

import numpy as np
import matplotlib.pyplot as plt
from matplotlib import cm

#Rounding
dp = 2

#Bottle parameters
p = 1
a = 3.4
b = 4
e = 4.3
theta = .7
phi = .45

d = np.sqrt(e*(e-p)/2)
H = (a+b+(a-b)*np.cos(theta))/np.sin(theta) + d
c = ((H-d)*np.sin(phi)+p*np.cos(phi)-e)/(1-np.cos(phi))

P = np.array([0,a*(1+np.cos(theta)),H-a*np.sin(theta)])
Q = np.array([0,b*(1-np.cos(theta)),d+b*np.sin(theta)])
PQ = Q-P
magPQ = np.sqrt(PQ[1]**2+PQ[2]**2)
PQhat = PQ/magPQ

t = np.array([
    0,
    d,
    d+b*theta,
    d+b*theta+magPQ,
    d+b*theta+magPQ+a*(np.pi+theta), 
    d+b*theta+magPQ+a*(np.pi+theta)+c*np.sin(phi),
    b*theta+magPQ+a*(np.pi+theta)+H-e*np.sin(phi),
    b*theta+magPQ+a*(np.pi+theta)+H,
    b*theta+magPQ+a*(np.pi+theta)+H+d
])

#Matplotlib model
n=1000

usplit = [np.linspace(np.pi/2, np.pi, n),   #np.linspace(t[0], t[1], n),
          np.linspace(t[1], t[2], 2*n),
          np.linspace(t[2], t[3], n),
          np.linspace(t[3], t[4], n),
          np.linspace(t[4], t[5], n),
          np.linspace(t[5], t[6], n),
          np.linspace(t[6], t[7], n),
          np.linspace(0,np.pi/2,  n)   #np.linspace(t[7], t[8], n)
          ]


# Set up a figure and axes
fig, surf = plt.subplots(1, 1, figsize=(6, 9), subplot_kw={'projection': '3d'})
surf.set_title(' ')


#Base 0
#Not canal
u = usplit[0]
gamma0 = (0*u,0*u,d*np.sin(u))
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x0 = ((e+p)/2+((e-p)/2)*np.cos(u))*np.sin(v)
y0 = ((e+p)/2+((e-p)/2)*np.cos(u))*np.cos(v)
z0 = d-d*np.sin(u)

#Handle 1
u = usplit[1]
r = p
rdot = 0
gamma1 = (0*u, b+b*np.cos(np.pi-(u-t[1])/b), d+b*np.sin(np.pi-(u-t[1])/b))
T = (0,np.sin(np.pi-(u-t[1])/b),-np.cos(np.pi-(u-t[1])/b))
N = (0,np.cos(np.pi-(u-t[1])/b),np.sin(np.pi-(u-t[1])/b))
B = (1,0,0)
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x1 = gamma1[0] - r*rdot*T[0] + r*np.sqrt(1-(rdot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y1 = gamma1[1] - r*rdot*T[1] + r*np.sqrt(1-(rdot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z1 = gamma1[2] - r*rdot*T[2] + r*np.sqrt(1-(rdot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v))

#Handle 2
u = usplit[2]
gamma2 = (0*u,((u-t[2])/magPQ)*P[1] + (1-(u-t[2])/magPQ)*Q[1],((u-t[2])/magPQ)*P[2] + (1-(u-t[2])/magPQ)*Q[2])
T = (0,-PQhat[1],-PQhat[2])
N = (0,PQhat[2],-PQhat[1])
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x2 = gamma2[0] - r*rdot*T[0] + r*np.sqrt(1-(rdot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y2 = gamma2[1] - r*rdot*T[1] + r*np.sqrt(1-(rdot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z2 = gamma2[2] - r*rdot*T[2] + r*np.sqrt(1-(rdot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v))

#Handle 3
u = usplit[3]
gamma3 = ( 0*u, a*(1+np.cos( (u-t[3])/a-theta )), H+a*np.sin( (u-t[3])/a-theta ) )
T = (0,-np.sin((u-t[3])/a-theta),np.cos((u-t[3])/a-theta))
N = (0,-np.cos((u-t[3])/a-theta),-np.sin((u-t[3])/a-theta))
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x3 = gamma3[0] - r*rdot*T[0] + r*np.sqrt(1-(rdot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y3 = gamma3[1] - r*rdot*T[1] + r*np.sqrt(1-(rdot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z3 = gamma3[2] - r*rdot*T[2] + r*np.sqrt(1-(rdot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v))

#Bottle canal 4
u = usplit[4]
r4 = p+c-np.sqrt(c**2-(u-t[4])**2)
r4dot = (u-t[4])/np.sqrt(c**2-(u-t[4])**2)
r4ddot = c**2*np.power(c**2-(u-t[4])**2,-1.5)
gamma4 = (0*u,0*u,H-u+t[4])
T = (0,0,-1)
N = (0,1,0)
B = (1,0,0)
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x4 = gamma4[0] - r4*r4dot*T[0] + r4*np.sqrt(1-(r4dot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y4 = gamma4[1] - r4*r4dot*T[1] + r4*np.sqrt(1-(r4dot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z4 = gamma4[2] - r4*r4dot*T[2] + r4*np.sqrt(1-(r4dot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v))

#Bottle canal 5
u = usplit[5]
r5 = d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)-u+t[5])*np.tan(phi)
r5dot = np.tan(phi)*np.ones(len(u))
r5ddot = np.zeros(len(u))
gamma5 = (0*u,0*u,H-c*np.sin(phi)-u+t[5])
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x5 = gamma5[0] - r5*r5dot*T[0] + r5*np.sqrt(1-(r5dot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y5 = gamma5[1] - r5*r5dot*T[1] + r5*np.sqrt(1-(r5dot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z5 = gamma5[2] - r5*r5dot*T[2] + r5*np.sqrt(1-(r5dot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v))

#Bottle canal 6
u = usplit[6]
r6 = np.sqrt(e**2-(e*np.sin(phi)-u+t[6])**2)
r6dot = (e*np.sin(phi)-u+t[6])/np.sqrt(e**2-(e*np.sin(phi)-u+t[6])**2)
r6ddot = -e**2*np.power((e**2-(e*np.sin(phi)-u+t[6])**2),-1.5)
gamma6 = (0*u,0*u,e*np.sin(phi)-u+t[6]+d)
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x6 = gamma6[0] - r6*r6dot*T[0] + r6*np.sqrt(1-(r6dot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y6 = gamma6[1] - r6*r6dot*T[1] + r6*np.sqrt(1-(r6dot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z6 = gamma6[2] - r6*r6dot*T[2] + r6*np.sqrt(1-(r6dot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v))

#Base 7
#Not canal
u = usplit[7]
gamma7 = (0*u,0*u,d*np.sin(u))
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x7 = ((e+p)/2+((e-p)/2)*np.cos(u))*np.sin(v)
y7 = ((e+p)/2+((e-p)/2)*np.cos(u))*np.cos(v)
z7 = d-d*np.sin(u)

x = np.concatenate([x0,x1,x2,x3,x4,x5,x6,x7],axis=1)
y = np.concatenate([y0,y1,y2,y3,y4,y5,y6,y7],axis=1)
z = np.concatenate([z0,z1,z2,z3,z4,z5,z6,z7],axis=1)

surf.plot_surface(x,y,z, cmap='viridis',  alpha = .5)

surf.set_aspect('equal')
surf.view_init(elev=10, azim=-30, roll=0)

plt.show()
:::


### Handle
::::{figure}
:label: handle-fig
:::{code-cell} python
:tags: hide-input
fig, ax = plt.subplots(1, 1, figsize=(6, 9), subplot_kw={'projection': '3d'})

x = np.concatenate([x0,x1,x2,x3],axis=1)
y = np.concatenate([y0,y1,y2,y3],axis=1)
z = np.concatenate([z0,z1,z2,z3],axis=1)

ax.plot_surface(x,y,z, cmap='viridis',  alpha = .5)

v = np.linspace(0,2*np.pi,100)
x = 1.1*((e+p)/2-((e-p)/2))*np.sin(v)
y = 1.1*((e+p)/2-((e-p)/2))*np.cos(v)
z = d
ax.plot(x,y,z,color='black',linewidth=.5)
ax.text(0,1.3*((e+p)/2-((e-p)/2)),d-.1,'Row 0')

ax.set_aspect('equal')
ax.view_init(elev=10, azim=-30, roll=0)

plt.show()
:::

Klein bottle handle
::::

(inner-base)=
#### Inner base
::::{figure}
:label: inner-base-pattern

:::{code-cell} python
:tags: hide-input

from tabulate import tabulate

#stitch dimensions
w = .4
h = .4

#Calculate first fundamental form (inner base)
u0 = np.flip(usplit[0])
z = d-d*np.sin(u0)
E0 = ( (e-p)*np.sin(u0)/2 )**2 + ( d*np.cos(u0) )**2
G0 = ((e+p)/2+((e-p)/2*np.cos(u0)))**2
l = len(u0)
du0 = np.zeros(l)
for j in range(1,l):
    du0[j] = u0[j-1]-u0[j]

#Line element in u direction (for row count)
ds0 = np.sqrt(E0)*du0

l = len(u0)
dz0 = np.zeros(l)
for j in range(1,l):
    dz0[j] = z[j]-z[j-1]

s0 = np.zeros(l)
for j in range(1,l):
    s0[j] = s0[j-1]+ds0[j]

#Row count (h-increments of u-direction line element)
row0 = [int(i/h) for i in s0]

#Circumference and stitch count
C0 = 2*np.pi*np.sqrt(G0)
st_count0 = np.round(C0/w)

base0 = np.transpose([row0,
                     st_count0,
                     u0,
                     z,
                     E0,
                     G0,
                     du0,
                     dz0,
                     ds0,
                     s0,
                     C0])

#Get rid of row repeats
index=[0]
for j in range(1,l):
    if row0[j]>row0[j-1]:
        index = np.append(index,j)
index = [int(i) for i in index]
base0 = base0[index]

#Tabulate (displayed later)
table = tabulate(base0,headers=['Row','Stitch count','u','z','E','G','du','dz','ds=√Edu','s≈(row no.)×(st height)','Circumference=2π√G'],tablefmt='html')

#Graphic
fig, ax = plt.subplots(1, 1, figsize=(4, 6), subplot_kw={'projection': '3d'})

ax.plot_surface(x0,y0,z0, cmap='viridis',  alpha = .5)

ax.set_aspect('equal')
ax.view_init(elev=10, azim=-30, roll=0)
ax.set_axis_off()
plt.show()
:::

Inner base
::::

**Row 0:** Make a foundation chain of {eval}`int(base0[0,1])` stitches, and join into a ring. 

**Rows 1–{eval}`max(row0)`:**  Work the following stitch counts evenly throughout each round to form the inside of the base/very bottom of the handle:

:::{code-cell} python
:label: handle-table
:tags: hide-input
display(table)
:::

#### Lower curved section.

::::{figure}
:label: lower-curved
:::{code-cell} python
:tags: hide-input

#Pipe-turns-for-handle
#Turning angle a
turning_angle_a = float(np.round(2*np.arcsin(w/(2*(a-p))),dp))

#Row count
N = round(p*np.pi/h)

#Stitch count
st_count = [0]*(N+1)
reduced_st_count = [0]*(N+1)
row = [0]*(N+1)
for n in range(N+1):
    st_count[n]=round(2*np.pi*(a-p*np.cos(n*np.pi/int(N)))/w)
    row[n] = n

#Reduced stitch count
for n in range(N+1):
    reduced_st_count[n] = round(st_count[n]/st_count[0])

#Dual stitch count a
dual_st_count_a = [2*N]
r = 0
rnd = [0]
for n in range(1,N+1):
    if reduced_st_count[n]>reduced_st_count[n-1]:
        dual_st_count_a.append(2*N-(2*n-1))
        r = r + 1
        rnd.append(r)


#Turning angle b
turning_angle_b = float(np.round(2*np.arcsin(w/(2*(b-p))),dp))

#Row count
N = round(p*np.pi/h)

#Stitch count
st_count = [0]*(N+1)
reduced_st_count = [0]*(N+1)
row = [0]*(N+1)
for n in range(N+1):
    st_count[n]=round(2*np.pi*(b-p*np.cos(n*np.pi/int(N)))/w)
    row[n] = n

#Reduced stitch count
for n in range(N+1):
    reduced_st_count[n] = round(st_count[n]/st_count[0])

#Dual stitch count b
dual_st_count_b = [2*N]
r = 0
rnd = [0]
for n in range(1,N+1):
    if reduced_st_count[n]>reduced_st_count[n-1]:
        dual_st_count_b.append(2*N-(2*n-1))
        r = r + 1
        rnd.append(r)

#Row count shorthand (for readability)
rc = [
    max(row0)+1,
    max(row0)+2*int(np.round(theta/turning_angle_b))+1,
    max(row0)+2*int(np.round(theta/turning_angle_b))+2,
    max(row0)+2*int(np.round(theta/turning_angle_b))+int(np.round(magPQ/h))+2,
    max(row0)+2*int(np.round(theta/turning_angle_b))+int(np.round(magPQ/h))+3,
    max(row0)+2*int(np.round(theta/turning_angle_b))+int(np.round(magPQ/h))+2*int(np.round((np.pi+theta)/turning_angle_a))+3
    ]

#Graphic
fig, ax = plt.subplots(1, 1, figsize=(4, 6), subplot_kw={'projection': '3d'})

x = np.concatenate([x0,x1],axis=1)
y = np.concatenate([y0,y1],axis=1)
z = np.concatenate([z0,z1],axis=1)

ax.plot_surface(x,y,z, cmap='viridis',  alpha = .5)

ax.set_aspect('equal')
ax.view_init(elev=10, azim=-30, roll=0)
ax.set_axis_off()
plt.show()
:::

Lower curved section
::::

Turn work upside down, to work the next round into the other side of the foundation chain.
:::{tip} Important note!
To have the "right side" facing outwards for the majority of the visible surface, you should crochet with stitches facing the **opposite** way around for this next section of work.
:::

**Rows {eval}`rc[0]`–{eval}`rc[1]`:** Repeat the following two rows {eval}`int(np.round(theta/turning_angle_b))` times.
 - Work {eval}`int(base0[0,1])` dc around. 
 - Work a short row of {eval}`dual_st_count_b[1]` stitches, placed on outside of bend.

Pipe should have turned approximately {eval}`float(np.round(theta,dp))` radians.

#### Straight section
::::{figure}
:label: straight-section-graphic

:::{code-cell} python
:tags: hide-input
fig, ax = plt.subplots(1, 1, figsize=(4, 6), subplot_kw={'projection': '3d'})

x = np.concatenate([x0,x1,x2],axis=1)
y = np.concatenate([y0,y1,y2],axis=1)
z = np.concatenate([z0,z1,z2],axis=1)

ax.plot_surface(x,y,z, cmap='viridis',  alpha = .5)

ax.set_aspect('equal')
ax.view_init(elev=10, azim=-30, roll=0)
ax.set_axis_off()
plt.show()
:::

Straight section
::::

**Rows {eval}`rc[2]`–{eval}`rc[3]`:** {eval}`dual_st_count_b[0]` dc in each st around, {eval}`int(np.round(magPQ/h))` times.

#### Upper curved section

::::{figure}
:label: upper-handle-plot
:::{code-cell} python
:tags: hide-input
fig, ax = plt.subplots(1, 1, figsize=(4, 6), subplot_kw={'projection': '3d'})

x = np.concatenate([x0,x1,x2,x3],axis=1)
y = np.concatenate([y0,y1,y2,y3],axis=1)
z = np.concatenate([z0,z1,z2,z3],axis=1)

ax.plot_surface(x,y,z, cmap='viridis',  alpha = .5)

ax.set_aspect('equal')
ax.view_init(elev=10, azim=-30, roll=0)
ax.set_axis_off()
plt.show()
:::

Upper curved section
::::
**Rows {eval}`rc[4]`–{eval}`rc[5]`:** 
Repeat the following two rows {eval}`int(np.round((np.pi+theta)/turning_angle_a))` times:
 - Work {eval}`dual_st_count_a[0]` dc around. 
 - Work a short row of {eval}`dual_st_count_a[1]` stitches, and placed on the **opposite** side to the previous set of short rows, so that the pipe turns in the opposite direction.

Pipe should have turned approximately {eval}`float(np.round(np.pi+theta,dp))` radians.

### Main body - still need to sort self-intersection

::::{figure}
:label: body-pattern

:::{code-cell} python
:tags: hide-input

#Calculate first fundamental form Edu^2+Gdv^2
#canal 
u = np.concatenate([usplit[4],usplit[5],usplit[6]])
r=np.concatenate([r4,r5,r6])
rdot=np.concatenate([r4dot,r5dot,r6dot])
rddot=np.concatenate([r4ddot,r5ddot,r6ddot])
gamma = np.concatenate([gamma4[2],gamma5[2],gamma6[2]])
z = gamma+r*rdot #T=(0,0,-1)
E = (1-r*rddot-rdot**2)**2 + (rdot**2*(1-rdot**2-r*rddot)**2)/(1-rdot**2)
G = r**2*(1-rdot**2)
l = len(u)
du = np.zeros(l)
for j in range(1,l):
    du[j] = u[j]-u[j-1]


#add base
#base
u7 = usplit[7]
E7 = ( (e-p)*np.sin(u7)/2 )**2 + ( d*np.cos(u7) )**2
G7 = ((e+p)/2+((e-p)/2*np.cos(u7)))**2
l = len(u7)
du7 = np.zeros(l)
for j in range(1,l):
    du7[j] = u7[j]-u7[j-1]

u = np.concatenate([u,u7])
z = np.concatenate([z,d-d*np.sin(u7)])
E = np.concatenate([E,E7])
G = np.concatenate([G,G7])
du = np.concatenate([du,du7])

#line element
ds = np.sqrt(E)*du

l=len(u)
dz = np.zeros(l)
for j in range(1,l):
    dz[j] = z[j]-z[j-1]

s = np.ones(l)*(rc[5]+1)*h
for j in range(1,l):
    s[j] = s[j-1]+ds[j]

#Row count (h-increments of u-direction line element)
row = [int(i/h) for i in s]

#Circumference and stitch count
C = 2*np.pi*np.sqrt(G)
st_count = np.round(C/w)

data = np.transpose([row,
                     st_count,
                     u,
                     z,
                     E,
                     G,
                     du,
                     dz,
                     ds,
                     s,
                     C])

#Get rid of row repeats
index=[0]
for j in range(1,l):
    if row[j]>row[j-1]: 
        index = np.append(index,j)
index = [int(i) for i in index]
data = data[index]

#Tabulate (displayed later)
table = tabulate(data,headers=['Row','Stitch count','u','z','E','G','du','dz','ds=√Edu','s≈(row no.)×(st height)','Circumference=2π√G'],tablefmt='html')

#Row count shorthand (for readability)
rc = np.concatenate([rc,rc[5]+1,max(row)],axis=None)
rc = [int(i) for i in rc]

#Graphic
fig, ax = plt.subplots(1, 1, figsize=(4, 6), subplot_kw={'projection': '3d'})

x = np.concatenate([x4,x5,x6,x7],axis=1)
y = np.concatenate([y4,y5,y6,y7],axis=1)
z = np.concatenate([z4,z5,z6,z7],axis=1)

ax.plot_surface(x,y,z, cmap='viridis',  alpha = .5)

ax.set_aspect('equal')
ax.view_init(elev=10, azim=-30, roll=0)
ax.set_axis_off()
plt.show()
:::

Main body
::::
Worked from the top down.

**Rows {eval}`min(row)`–{eval}`max(row)`:** Work the following row counts, spacing increases evenly throughout each round.

:::{code-cell} python
:tags: hide-input
display(table)
:::

## Derivation
The majority of the Klein bottle pattern comes from a canal surface parametrisation with the following directrix.

::::{figure}
:label: fig:klein-canal
:::{code-cell} python
:tags: remove-input
colour = ['magenta', 'darkviolet', 'blue', 'cyan', 'limegreen', 'yellow', 'orangered', 'red']

fig = plt.figure(figsize=plt.figaspect(.5))
fig.suptitle(' ')

directrix = fig.add_subplot(1, 2, 1)
surf = fig.add_subplot(1, 2, 2, projection='3d')

directrix.set_title('Directrix')
surf.set_title('Canal surface')

surf.plot_surface(x1, y1, z1, color=colour[1], edgecolor = 'gray', linewidth = .1, alpha = .25)
directrix.plot(gamma1[1],gamma1[2], color=colour[1])

surf.plot_surface(x2, y2, z2, color=colour[2], edgecolor = 'gray', linewidth = .1, alpha = .25)
directrix.plot(gamma2[1],gamma2[2], color=colour[2])

surf.plot_surface(x3, y3, z3, color=colour[3], edgecolor = 'gray', linewidth = .1, alpha = .25)
directrix.plot(gamma3[1],gamma3[2], color=colour[3])

surf.plot_surface(x4, y4, z4, color=colour[4], edgecolor = 'gray', linewidth = .1, alpha = .25)
directrix.plot(gamma4[1],gamma4[2], color=colour[4])

surf.plot_surface(x5, y5, z5, color=colour[5], edgecolor = 'gray', linewidth = .1, alpha = .25)
directrix.plot(gamma5[1],gamma5[2], color=colour[5])

surf.plot_surface(x6, y6, z6, color=colour[6], edgecolor = 'gray', linewidth = .1, alpha = .25)
directrix.plot(gamma6[1],gamma6[2], color=colour[6])

directrix.set_aspect('equal')

surf.set_aspect('equal')
surf.view_init(elev=0, azim=0, roll=0)
#surf.set_axis_off()

plt.show()
:::

Klein bottle as canal surface.
::::

The base cannot be realised as part of this canal surface parametrisation, as the radius function would have unbounded derivative at the very bottom. Instead, we add the base as a more pedestrian surface of revolution. 

::::{figure}
:label: fig:klein-with-base
:::{code-cell} python
:tags: remove-input
#add base
fig, ax = plt.subplots(1, 1, figsize=(4, 6), subplot_kw={'projection': '3d'})
ax.set_title(' ')

x = np.concatenate([x1,x2,x3,x4,x5,x6],axis=1)
y = np.concatenate([y1,y2,y3,y4,y5,y6],axis=1)
z = np.concatenate([z1,z2,z3,z4,z5,z6],axis=1)

ax.plot_surface(x,y,z, edgecolor = 'gray', linewidth = .1, alpha = .25)

x = np.concatenate([x0,x7],axis=1)
y = np.concatenate([y0,y7],axis=1)
z = np.concatenate([z0,z7],axis=1)

ax.plot_surface(x,y,z, edgecolor = 'gray', linewidth = .1, alpha = .25)

ax.set_aspect('equal')
ax.view_init(elev=0, azim=0, roll=0)

plt.show()
:::

Klein bottle with base
::::

Row count and stitch count per row are calculated using the first fundamental form
$$
Edu^2+2Fdudv+Gdv^2
$$
associated with the parametrised surface.  In fact, $F=0$ for each parametrisation (another example of a Clairaut surface). Moreover, $E:=\mathbf{x}_u^2$ determines row count, and $G:=\mathbf{x}_v^2$ determines stitch count per row.See [](#towards-crochetability) for more details.

The pattern has 6 free parameters, $p$ (pipe thickness), $a$, $b$ (handle turning radii), $e$ (bottle radius), $\theta$ (handle angle) and $\phi$ (neck angle). In addition, $d$ (base depth), $H$ (bottle height) and $c$ (neck turning radius) are dependent parameters determined by [](#eq:hab-constraint), [](#eq:de-constraint) and [](#eq:pephi-constraint). These constraints ensure the directrix and radius function are $C^1$.

:::::{grid} 1 1 2 2
::::{grid-item}
:::{figure} figs/klein-directrix.png
:label: fig:directrix
:height: 450

Directrix, $\gamma$.
:::
::::
::::{grid-item}
:::{figure} figs/klein-radius.png
:label: fig:radius-fn
:height: 450

Radius function, $r$.
:::
::::
:::::

Both functions are composed of straight and circular segments. The directrix $\gamma$ ([](#fig:directrix)) is parametrised by arc length, so it is unit speed. This simplifies the canal surface parametrisation [](#eq:canal-param) somewhat:
:::{math}
:enumerated: true
:label: eq:canal-unit-speed
\mathbf{x}(u,v) = \gamma-r\dot{r}\mathbf{T} +r\sqrt{1-\dot{r}^2}\Big( \mathbf{N}\cos(v)+\mathbf{B}\sin(v)\Big), u\geq d,\; 0\leq v\leq 2\pi.
:::

Observe that the radius function $r$ ([](#fig:radius-fn)) has an unbounded derivative near $z=0$. For this reason, the canal surface parametrisation will not work in this section, and we use a surface of revolution instead. 

:::{math}
:enumerated: true
:label: eq:base-SoR
\mathbf{x}(u,v) = \left(
    \begin{array}{c}
    \left(\frac{e+p}{2}-\frac{e-p}{2}\cos(u)\right)\cos(v) \\[.5em]
    \left(\frac{e+p}{2}-\frac{e-p}{2}\cos(u)\right)\sin(v) \\[.5em]
    d\sin(u)
    \end{array}
    \right), \hspace{1em} 0\leq u\leq d,\; 0\leq v\leq 2\pi.
:::

We will see that, for an adequate choice of $d$, these two parametrisations agree along the boundary $u=d$, and are $C^1$. See [](#canal_v_SoR) for more detailed discussion of these two methods for generating parametrised surfaces from curves.

More specific formulae and dependencies between the parameters ($H$, $a$, $b$, $\theta$, etc.) are discussed next.

### Directrix shape

Using the notation of [](#fig:directrix),
$$
P=(a+a\cos\theta,H-a\sin\theta), \; \text{ and } \;Q=(b-b\cos\theta,d+b\sin\theta)
$$
and so gradient of $L$ is
$$
\cot\theta = \frac{d+b\sin\theta - H + a\sin\theta}{b-b\cos\theta - a-a\cos\theta},
$$
which rearranges to give the following contraint on our choice of parameters $H$, $a$, $b$,  $d$ and $\theta$:
:::{math}
:enumerated: true
:label: eq:hab-constraint
(H-d)\sin\theta=a+b+(a-b)\cos\theta.
:::
The $z$-intercept is 
$$
d+b\sin\theta - \cot\theta(b-b\cos\theta) =d+b(\csc\theta-\cot\theta).
$$  

<!---On the other hand, using coordinates of $P$,
$$
c=H-a\sin\theta-\cot\theta(a+a\cos\theta) = H-a(\csc\theta+\cot\theta).
$$

So $H-a(\csc\theta+\cot\theta)=d+b(\csc\theta-\cot\theta)$, which rearranges to give [](#eq:hab-constraint).--->

Hence $L$ has Cartesian equation $z=(\cot\theta)y+d+b(\csc\theta-\cot\theta)$, subject to [](#eq:hab-constraint).


(directrix-param)=
#### Arc length parametrisation
We take the concatenation of the following arc length-parametrised curves.

\begin{gather*}
\gamma_1(u) = \left(0, b+b\cos\left(\pi-\frac{u}{b}\right), d+b\sin\left(\pi-\frac{u}{b}\right)\right), \hspace{1em} 0\leq u\leq b\theta, \\
\gamma_2(u) = \left(\frac{u}{\Vert\overrightarrow{PQ}\Vert}\right)P + \left(1-\frac{u}{\Vert\overrightarrow{PQ}\Vert}\right)Q, \hspace{1em} 0\leq u\leq \Vert\overrightarrow{PQ}\Vert, \\
\gamma_3(u) = \left( 0, a\left(1+\cos\left(\frac{u}{a}-\theta\right)\right), H+a\sin\left(\frac{u}{a}-\theta\right)\right), \hspace{1em} 0\leq u\leq a(\pi+\theta), \\
\gamma_4(u) = (0,0,H-u), \hspace{1em} 0\leq u\leq H-d.
\end{gather*}

This gives a unit speed parametrised curve $\gamma:[0,T]\to\mathbb{R}^3$, where $T=H+a(\pi+\theta)+\Vert\overrightarrow{PQ}\Vert+b\theta$ is the total length of $\gamma$.

The Frenet-Serret apparatus are then simple to compute from these formulae: $\mathbf{T}=\dot{\gamma}$, $\mathbf{N}=\frac{\ddot{\gamma}}{\Vert\ddot{\gamma}\Vert}$, $\mathbf{B}=(1,0,0)$.


### Radius function
:::{embed} fig:radius-fn
:::

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
$$
Applying this to $A=\frac{1}{2}(e-p)$, $B=d$ and $t=0$ tells us that the half-ellipse has curvature $\kappa=\frac{A}{B^2} = \frac{e-p}{2d^2}$ at $(e,0$). In other words, we should choose $d$ so that $
\frac{e-p}{2d^2}=\frac{1}{e}$. So, we take 
:::{math}
:enumerated: true
:label: eq:de-constraint
d := \sqrt{\frac{e(e-p)}{2}}.
:::

Middle circular section: 
$$
r=\sqrt{e^2-(z-d)^2} \hspace{1em} d\leq z\leq d+e\sin\phi.
$$

Linear section: we have end points $X=(e\cos\phi,d+e\sin\phi)$, $Y=(p+c-c\cos\phi,H-c\sin\phi)$, so line through $X$ and $Y$ has equation $z=(-\cot\phi)r +k$, where
$$
k=d+e\sin\phi+(\cot\phi)e\cos\phi=d+e\csc\phi.
$$
On the other hand
$$
k=H-c\sin\phi+(\cot\phi)(p+c-c\cos\phi) = H+(c+p)\cot\phi-c\csc\phi.
$$
So we have the following constraint on our choice of new parameters $p$, $e$ and $\phi$:
$$
e = (H-d)\sin\phi+(c+p)\cos\phi-c
$$
or in other words,
:::{math}
:enumerated: true
:label: eq:pephi-constraint
c = \frac{(H-d)\sin\phi+p\cos\phi-e}{1-\cos\phi}.
:::

Linear section equation:
$$
r=(d+e\csc\phi-z)\tan\phi=d\tan\phi+e\sec\phi-z\tan\phi,
$$
for $d+e\sin\phi\leq z\leq H-c\sin\phi$.

Final circular section: subject to [](#eq:pephi-constraint), 
$$
r=p+c-\sqrt{c^2-(z-H)^2}, \hspace{1em}z\in[H-c\sin\phi,H]
$$

Parametrisation:
- $r_1(u)=\sqrt{e^2-(u-d)^2}$, $\dot r_1(u)=-\frac{u-d}{\sqrt{e^2-(u-d)^2}}$, for $u\in[d,d+e\sin\phi]$.
- $r_2(u)=d\tan\phi+e\sec\phi-u\tan\phi$, $\dot r_2(u)=-\tan\phi$, for $u\in[d+e\sin\phi,H-c\sin\phi]$.
- $r_3(u)=p+c-\sqrt{c^2-(u-H)^2}$, $\dot r_3(u)=\frac{u-H}{\sqrt{c^2-(u-H)^2}}$ for $u\in[H-c\sin\phi,H]$.


(canal_v_SoR)=
### Canal surface vs surface of revolution
While it is true that a canal surface with directrix the $z$ axis is a surface of revolution, it is not the same as the surface obtained by revolving the radius function around the $z$ axis. This is a consequence of the envelope conditon [](#eq:envelope-cond) of {numref}`canal`.

To illustrate this, suppose we have unit speed directrix $\gamma(u)=(0,0,u)$ and Frenet-Serret apparatus $\mathbf{T}=(0,0,1)$, $\mathbf{N}=(0,1,0)$, $\mathbf{B}=(1,0,0)$. The [canal parametrisation](#eq:canal-unit-speed) simplifies to
$$
\mathbf{x}(u,v) = \left(r\sqrt{1-\dot{r}^2}\sin(v),r\sqrt{1-\dot{r}^2}\cos(v),u-r\dot{r}\right).
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



# Set up a figure and axes
fig = plt.figure(figsize=plt.figaspect(.5))
fig.suptitle('Comparative Klein bottle parametrisations')

ax1 = fig.add_subplot(1, 2, 1, projection='3d')
ax2 = fig.add_subplot(1, 2, 2, projection='3d')

t = np.array([
    0,
    d, 
    e*np.sin(phi)+d, 
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
x = ((e+p)/2-((e-p)/2)*np.cos(u))*np.sin(v)
y = ((e+p)/2-((e-p)/2)*np.cos(u))*np.cos(v)
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
            r[j] = np.sqrt(e**2-(u[j]-d)**2)
            rdot[j] = -(u[j]-d)/np.sqrt(e**2-(u[j]-d)**2)
        elif t[2] < u[j] <= t[3]:
            r[j] = e/np.cos(phi)-(u[j]-d)*np.tan(phi)
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
            gamma[:,j] = (0, a*(1+np.cos(np.pi-(u[j]-t[4])/a)), H+a*np.sin(np.pi-(u[j]-t[4])/a))
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

### Towards crochetability
The first problem to solve is to decide how to measure rows. The most accurate would be to measure them along meridians of constant $v$, according to the canal surface parametrisation. This requires the line element in terms of the first fundamental form (Riemannian metric),
$$
ds^2 = Edu^2 + 2Fdudv + Gdv^2,
$$
where coefficients $E=\mathbf{x}_u^2$, $F=\mathbf{x}_u\cdot\mathbf{x}_v$ and $F=\mathbf{x}_v^2$. We should calculate $E$ for row count, as this measures distances along constant $v$-meridians.

#### Main body excluding base
For the main body of the Klein bottle, we have unit speed directrix 
$$
\gamma(u)=(0,0,u), \hspace{1em} \text{ for } 0\leq u\leq H,
$$
and constant Frenet-Serret apparatus $\mathbf{T}=(0,0,1)$, $\mathbf{N}=(0,1,0)$, $\mathbf{B}=(1,0,0)$ along $\gamma$. Moreover,
:::{math}
:enumerated: true
:label: eq:body
\mathbf{x}(u,v) = \gamma-r\dot{r}\mathbf{T} +r\sqrt{1-\dot{r}^2}\Big( \mathbf{N}\cos(v)+\mathbf{B}\sin(v)\Big),
:::
for $0\leq u\leq H$, $0\leq v\leq 2\pi$. The radius $r$ function comes in three pieces:
$$
r(u)=\left\{\begin{array}{lr} \sqrt{e^2-(u-d)^2} & \text{ for } d\leq u\leq d+e\sin\phi \\
d\tan\phi+e\sec\phi-u\tan\phi & \text{ for } d+e\sin\phi\leq u\leq H-c\sin\phi \\
p+c-\sqrt{c^2-(u-H)^2} & \text{ for } H-c\sin\phi\leq u\leq H.
\end{array}\right.
$$
Its first and second derivatives are
$$
\dot{r}(u)=\left\{\begin{array}{lr} -\frac{u-d}{\sqrt{e^2-(u-d)^2}} & \text{ for } d\leq u<d+e\sin\phi \\
-\tan\phi & \text{ for } d+e\sin\phi\leq u<H-c\sin\phi \\
\frac{u-H}{\sqrt{c^2-(u-H)^2}} & \text{ for } H-c\sin\phi\leq u\leq H,
\end{array}\right.
$$
and
$$
\ddot{r}(u)=\left\{\begin{array}{lr} -e^2\left(e^2-(u-d)^2\right)^{-\frac{3}{2}} & \text{ for } d<u<d+e\sin\phi \\
0 & \text{ for } d+e\sin\phi<u<H-c\sin\phi \\
c^2\left(c^2-(u-H)^2\right)^{-\frac{3}{2}} & \text{ for } H-c\sin\phi<u<H.
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
\mathbf{x}_v = -r\sqrt{1-\dot{r}^2}\sin(v)\mathbf{N} + r\sqrt{1-\dot{r}^2}\cos(v)\mathbf{B}.
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

Suppose we have crocheted $n$ rows with stitch height $h>0$. We'd expect the length of produced fabric to satisfy
$$
nh \approx \sqrt{E}u
$$
(remember $u$ is arc length for the directrix, and begins at $0$). To work out the total number of rows needed we need only evaluate at $u=H$ and divide by stitch height (note $z=H$ here, too, since $r$ is stationary).

We can also calculate the $u$-steps required (this will not be constant). Write $0=u_0,u_1,u_2,\ldots$ for the $u$ values associated with rows $n=0,1,2\ldots,N$. Then,
:::{math}
:enumerated: true
:label: eq:un
\text{Stitch height: } h \approx \sqrt{E|_{u=u_n}}\cdot (u_n-u_{n-1}), \hspace{1em} n=1,2,\ldots,N.
:::
Stitch width and stitch count per row will behave more regularly. On a given row $n$ (i.e. for $u=u_n$), circumference can be calculated two ways:
:::{math}
:enumerated: true
:label: eq:stitch-count
(\text{Stitch width})\cdot(\text{Row $n$ stitch count}) \approx 2\pi\sqrt{G} = 2\pi r\sqrt{1-\dot{r}^2}.
:::
Assuming a constant crochet gauge, and for a given set of bottle parameters, equations [](#eq:un) and [](#eq:stitch-count) together can be solved numerically to obtain a pattern for the main body.

#### Base
For $0\leq u\leq \pi$ and $0\leq v\leq 2\pi$,
$$
\mathbf{x}(u,v) = \left(\begin{array}{c}
\left(\frac{e+p}{2}+\frac{e-p}{2}\cos(u)\right)\sin(v) \\
\left(\frac{e+p}{2}+\frac{e-p}{2}\cos(u)\right)\cos(v) \\
d-d\sin(u)
\end{array}\right),
$$
so
$$
\mathbf{x}_u = \left(\begin{array}{c}
-\frac{e-p}{2}\sin(u)\sin(v) \\
-\frac{e-p}{2}\sin(u)\cos(v) \\
-d\cos(u)
\end{array}\right),
$$
$$
\mathbf{x}_v = \left(\begin{array}{c}
\left(\frac{e+p}{2}+\frac{e-p}{2}\cos(u)\right)\cos(v) \\
-\left(\frac{e+p}{2}+\frac{e-p}{2}\cos(u)\right)\sin(v) \\
0
\end{array}\right),
$$
So for the base (which we recall is **not** a canal surface)
$$
E = \left(\frac{e-p}{2}\right)^2\sin^2(u)+d^2\cos^2(u), \hspace{1em} F = 0,\hspace{1em} G = \left(\frac{e+p}{2}+\frac{e-p}{2}\cos(u)\right)^2.
$$

### Self intersection

For the pattern example we have been considering, the self-intersection occurs at the straight section of handle, and conical section of main bottle.

::::{figure} 
:::{code-cell}
:tags: remove-input
fig, ax = plt.subplots(1, 1, figsize=(6, 12), subplot_kw={'projection': '3d'})
ax.set_title(' ')

ax.plot_surface(x0,y0,z0, color=colour[0], edgecolor = 'gray', linewidth = .1, alpha = .25)
ax.plot_surface(x1,y1,z1, color=colour[1], edgecolor = 'gray', linewidth = .1, alpha = .25)
ax.plot_surface(x2,y2,z2, color=colour[2], edgecolor = 'gray', linewidth = .1, alpha = .25)
ax.plot_surface(x3,y3,z3, color=colour[3], edgecolor = 'gray', linewidth = .1, alpha = .25)
ax.plot_surface(x4,y4,z4, color=colour[4], edgecolor = 'gray', linewidth = .1, alpha = .25)
ax.plot_surface(x5,y5,z5, color=colour[5], edgecolor = 'gray', linewidth = .1, alpha = .25)
ax.plot_surface(x6,y6,z6, color=colour[6], edgecolor = 'gray', linewidth = .1, alpha = .25)
ax.plot_surface(x7,y7,z7, color=colour[7], edgecolor = 'gray', linewidth = .1, alpha = .25)

ax.set_aspect('equal')
ax.view_init(elev=0, azim=0, roll=0)

plt.show()
::::

In other words, we should look at solving $\mathbf{x}_1(u_1,v_1)=\mathbf{x}_2(u_2,v_2)$, where 
$$
u_1\in[d+b\theta+\Vert\overrightarrow{PQ}\Vert+a(\pi+\theta)+c\sin\phi, b\theta+\Vert\overrightarrow{PQ}\Vert+a(\pi+\theta)+H-e\sin\phi],
$$

$$
u_2\in[d+b\theta, d+b\theta+\Vert\overrightarrow{PQ}\Vert], 
$$

$$
v_1,v_2\in[0,2\pi], 
$$

$$
\mathbf{x}_1 = \gamma_1 - r_1\dot{r_1}\mathbf{T}_1 + r_1\sqrt{1-\dot{r_1}^2}(\mathbf{N}_1\cos(v)+\mathbf{B}_1\sin(v)) ,
$$

$$
\mathbf{x}_2 = \gamma_2 + p(\mathbf{N}_2\cos(v)+\mathbf{B}_2\sin(v))
$$

$$
\gamma_1 = \left(0,0,H+d+b\theta+\Vert\overrightarrow{PQ}\Vert+a(\pi+\theta)-u)\right)
$$

$$
r_1 = e\sec\phi-(H+b\theta+\Vert\overrightarrow{PQ}\Vert+a(\pi+\theta))\tan\phi+u\tan\phi
$$

$$
\dot{r_1} = \tan\phi
$$

$\mathbf{T}_1 = (0,0,-1)$, $\mathbf{N}_1 = (0,1,0)$, $\mathbf{B}_1 = (1,0,0)$.

$$
\gamma_2 = Q+ (d+b\theta)\hat{\overrightarrow{PQ}}- u\hat{\overrightarrow{PQ}}
$$

$$
P = (0,a(1+\cos\theta),H-a\sin\theta)
$$

$$
Q = (0,b(1-\cos\theta),d+b\sin\theta)
$$

$r_2 = p$, $\dot{r_2}=0$

$\mathbf{T}_2 = (0,-\hat{\overrightarrow{PQ}}_1,-\hat{\overrightarrow{PQ}}_2)$, $\mathbf{N}_2 = (0,\hat{\overrightarrow{PQ}}_2,-\hat{\overrightarrow{PQ}}_1)$, $\mathbf{B}_2 = (1,0,0)$.


So,
$$
\begin{aligned}
\mathbf{x}_1(u_1,v_1) &= \left(\begin{array}{c}
r_1(u_1)\sqrt{1-\tan^2\phi}\sin(v_1) \\
r_1(u_1)\sqrt{1-\tan^2\phi}\cos(v_1) \\
(\gamma_1(u_1))_2+r_1\tan\phi
\end{array}\right) \\
&= \left(\begin{array}{c}
p\sin(v_2)\\
(\gamma_2(u_2))_1+p\hat{\overrightarrow{PQ}}_2 \\
(\gamma_2(u_2))_2-p\hat{\overrightarrow{PQ}}_1
\end{array}\right) = \mathbf{x}_2(u_2,v_2) 
\end{aligned}
$$


$$
\left\{\begin{array}{rclr}
r_1(u_1)\sqrt{1-\tan^2\phi}\sin(v_1) &=& p\sin(v_2) \\
r_1(u_1)\sqrt{1-\tan^2\phi}\cos(v_1) &=& (\gamma_2(u_2))_1+p\hat{\overrightarrow{PQ}}_2\cos(v_2) \\
(\gamma_1(u_1))_2+r_1\tan\phi &=& (\gamma_2(u_2))_2-p\hat{\overrightarrow{PQ}}_1\cos(v_2)
\end{array}\right.
$$

To get the range of rows affected, we first focus on the case where $\sin(v_1)=\sin(v_2)=0$ and $\cos(v_1)=\cos(v_2)=1$.

$$
\left\{\begin{array}{rclr}
r_1(u_1)\sqrt{1-\tan^2\phi} &=& (\gamma_2(u_2))_1+p\hat{\overrightarrow{PQ}}_2 &\text{ (A)}\\
(\gamma_1(u_1))_2+r_1\tan\phi &=& (\gamma_2(u_2))_2-p\hat{\overrightarrow{PQ}}_1 &\text{ (B)}
\end{array}\right.
$$


$$
\left\{\begin{array}{rclr}
A_1+A_2u_1 &=& A_3+A_4u_2 &\text{ (A)}\\
B_1+B_2u_1 &=& B_3+B_4u_2 &\text{ (B)}
\end{array}\right.
$$

where $A_2=\tan\phi\sqrt{1-\tan^2\phi}$,
$$
A_1 = \big(e\sec\phi-(H+b\theta+\Vert\overrightarrow{PQ}\Vert+a(\pi+\theta))\tan\phi\big)\sqrt{1-\tan^2\phi}
$$

$$
A_3 = Q_1+ (d+b\theta)\hat{\overrightarrow{PQ}}_1+p\hat{\overrightarrow{PQ}}_2, \hspace{1em} A_4=-\hat{\overrightarrow{PQ}}_1
$$

$$
B_1 = H+d+b\theta+\Vert\overrightarrow{PQ}\Vert+a(\pi+\theta)+e\sec\phi\tan\phi-(H+b\theta+\Vert\overrightarrow{PQ}\Vert+a(\pi+\theta))\tan^2\phi,\hspace{1em} B_2=\tan^2\phi-1
$$

$$
B_3 = Q_2+ (d+b\theta)\hat{\overrightarrow{PQ}}_2-p\hat{\overrightarrow{PQ}}_1, \hspace{1em}B_4=-\hat{\overrightarrow{PQ}}_2
$$
:::{code-cell} python
:tags: hide-input
v1 = 0
v2 = 0

print('Case 1: When v1 = ', v1,' and v2 = ',v2)

#Eqn X: X1u1+X2 = X3
#u1*( np.tan(phi)*np.sqrt(1-(np.tan(phi))**2)*np.sin(v1) )
#+(d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)+t[5])*np.tan(phi))*np.sqrt(1-(np.tan(phi))**2)*np.sin(v1) = p*np.sin(v2)) 
X1 =  np.tan(phi)*np.sqrt(1-(np.tan(phi))**2)*np.sin(v1) 
X2 = (d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)+t[5])*np.tan(phi))*np.sqrt(1-(np.tan(phi))**2)*np.sin(v1) 
X3 = p*np.sin(v2)

#Eqn A: B1u1+AB2 = B3u2+AB4
#u1*( np.tan(phi)*np.sqrt(1-(np.tan(phi))**2)*np.cos(v) )
#+(d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)+t[5])*np.tan(phi))*np.sqrt(1-(np.tan(phi))**2)*np.cos(v1) 
#= u2*( -PQhat[1] )+t[2]*PQhat[1] + Q[1] + p*PQhat[2]*np.cos(v2)
B1 = np.tan(phi)*np.sqrt(1-(np.tan(phi))**2)*np.cos(v1)
B2 = (d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)+t[5])*np.tan(phi))*np.sqrt(1-(np.tan(phi))**2)*np.cos(v1) 
B3 = -PQhat[1]
B4 = t[2]*PQhat[1] + Q[1] + p*PQhat[2]*np.cos(v2)

print(B1,'u1 + ',B2,' = ',B3,'u2 + ',B4)

#Eqn B: C1u1+C2 = C3u2+C4
#u1*( (np.tan(phi))**2-1 )
#+H-c*np.sin(phi)+t[5] +(d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)+t[5])*np.tan(phi))*np.tan(phi) 
#= u2*( -PQhat[2] )+t[2]*PQhat[2] + Q[2] - p*PQhat[1]*np.cos(v2)
C1 = (np.tan(phi))**2-1
C2 = H-c*np.sin(phi)+t[5] +(d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)+t[5])*np.tan(phi))*np.tan(phi) 
C3 = -PQhat[2]
C4 = t[2]*PQhat[2] + Q[2] - p*PQhat[1]*np.cos(v2)

print(C1,'u1 + ',C2,' = ',C3,'u2 + ',C4)

#B1*u1 + B2 = B3*u2 + B4
#u2 = (B1/B3)*u1 + (B2-B4)/B3
#C1*u1 + C2 = C3*u2 + C4
#u1 = (C3/C1)*u2 + (C4-C2)/C1 = (C3/C1)*( (B1/B3)*u1 + (B2-B4)/B3 ) + (C4-C2)/C1
#= (C3/C1)*(B1/B3)*u1 + (C3/C1)*(B2-B4)/B3 + (C4-C2)/C1 
u1a = ( (C3/C1)*(B2-B4)/B3 + (C4-C2)/C1 )/(1 - (C3/C1)*(B1/B3))
print('Conclusion: When v1 = ',v1,'and v2 = ',v2,'\n ','\t'*6,' u1 = ',u1a)

v1 = 0
v2 = np.pi
print('\nCase 2: When v1 = ', v1,' and v2 = ',v2)

#Eqn X: X1u1+X2 = X3
#u1*( np.tan(phi)*np.sqrt(1-(np.tan(phi))**2)*np.sin(v1) )
#+(d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)+t[5])*np.tan(phi))*np.sqrt(1-(np.tan(phi))**2)*np.sin(v1) = p*np.sin(v2)) 
X1 =  np.tan(phi)*np.sqrt(1-(np.tan(phi))**2)*np.sin(v1) 
X2 = (d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)+t[5])*np.tan(phi))*np.sqrt(1-(np.tan(phi))**2)*np.sin(v1) 
X3 = p*np.sin(v2)

#Eqn A: B1u1+AB2 = B3u2+AB4
#u1*( np.tan(phi)*np.sqrt(1-(np.tan(phi))**2)*np.cos(v) )
#+(d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)+t[5])*np.tan(phi))*np.sqrt(1-(np.tan(phi))**2)*np.cos(v1) 
#= u2*( -PQhat[1] )+t[2]*PQhat[1] + Q[1] + p*PQhat[2]*np.cos(v2)
B1 = np.tan(phi)*np.sqrt(1-(np.tan(phi))**2)*np.cos(v1)
B2 = (d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)+t[5])*np.tan(phi))*np.sqrt(1-(np.tan(phi))**2)*np.cos(v1) 
B3 = -PQhat[1]
B4 = t[2]*PQhat[1] + Q[1] + p*PQhat[2]*np.cos(v2)

print(B1,'u1 + ',B2,' = ',B3,'u2 + ',B4)

#Eqn B: C1u1+C2 = C3u2+C4
#u1*( (np.tan(phi))**2-1 )
#+H-c*np.sin(phi)+t[5] +(d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)+t[5])*np.tan(phi))*np.tan(phi) 
#= u2*( -PQhat[2] )+t[2]*PQhat[2] + Q[2] - p*PQhat[1]*np.cos(v2)
C1 = (np.tan(phi))**2-1
C2 = H-c*np.sin(phi)+t[5] +(d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)+t[5])*np.tan(phi))*np.tan(phi) 
C3 = -PQhat[2]
C4 = t[2]*PQhat[2] + Q[2] - p*PQhat[1]*np.cos(v2)

print(C1,'u1 + ',C2,' = ',C3,'u2 + ',C4)

#B1*u1 + B2 = B3*u2 + B4
#u2 = (B1/B3)*u1 + (B2-B4)/B3
#C1*u1 + C2 = C3*u2 + C4
#u1 = (C3/C1)*u2 + (C4-C2)/C1 = (C3/C1)*( (B1/B3)*u1 + (B2-B4)/B3 ) + (C4-C2)/C1
#= (C3/C1)*(B1/B3)*u1 + (C3/C1)*(B2-B4)/B3 + (C4-C2)/C1 
u1b = ( (C3/C1)*(B2-B4)/B3 + (C4-C2)/C1 )/(1 - (C3/C1)*(B1/B3))
print('Conclusion: When v1 = ',v1,'and v2 = ',v2,'\n ','\t'*6,' u1 = ',u1b)

v1 = np.pi
v2 = np.pi
print('\nCase 3: When v1 = ', v1,' and v2 = ',v2)

#Eqn X: X1u1+X2 = X3
#u1*( np.tan(phi)*np.sqrt(1-(np.tan(phi))**2)*np.sin(v1) )
#+(d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)+t[5])*np.tan(phi))*np.sqrt(1-(np.tan(phi))**2)*np.sin(v1) = p*np.sin(v2)) 
X1 =  np.tan(phi)*np.sqrt(1-(np.tan(phi))**2)*np.sin(v1) 
X2 = (d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)+t[5])*np.tan(phi))*np.sqrt(1-(np.tan(phi))**2)*np.sin(v1) 
X3 = p*np.sin(v2)

#Eqn A: B1u1+AB2 = B3u2+AB4
#u1*( np.tan(phi)*np.sqrt(1-(np.tan(phi))**2)*np.cos(v) )
#+(d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)+t[5])*np.tan(phi))*np.sqrt(1-(np.tan(phi))**2)*np.cos(v1) 
#= u2*( -PQhat[1] )+t[2]*PQhat[1] + Q[1] + p*PQhat[2]*np.cos(v2)
B1 = np.tan(phi)*np.sqrt(1-(np.tan(phi))**2)*np.cos(v1)
B2 = (d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)+t[5])*np.tan(phi))*np.sqrt(1-(np.tan(phi))**2)*np.cos(v1) 
B3 = -PQhat[1]
B4 = t[2]*PQhat[1] + Q[1] + p*PQhat[2]*np.cos(v2)

print(B1,'u1 + ',B2,' = ',B3,'u2 + ',B4)

#Eqn B: C1u1+C2 = C3u2+C4
#u1*( (np.tan(phi))**2-1 )
#+H-c*np.sin(phi)+t[5] +(d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)+t[5])*np.tan(phi))*np.tan(phi) 
#= u2*( -PQhat[2] )+t[2]*PQhat[2] + Q[2] - p*PQhat[1]*np.cos(v2)
C1 = (np.tan(phi))**2-1
C2 = H-c*np.sin(phi)+t[5] +(d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)+t[5])*np.tan(phi))*np.tan(phi) 
C3 = -PQhat[2]
C4 = t[2]*PQhat[2] + Q[2] - p*PQhat[1]*np.cos(v2)

print(C1,'u1 + ',C2,' = ',C3,'u2 + ',C4)

#B1*u1 + B2 = B3*u2 + B4
#u2 = (B1/B3)*u1 + (B2-B4)/B3
#C1*u1 + C2 = C3*u2 + C4
#u1 = (C3/C1)*u2 + (C4-C2)/C1 = (C3/C1)*( (B1/B3)*u1 + (B2-B4)/B3 ) + (C4-C2)/C1
#= (C3/C1)*(B1/B3)*u1 + (C3/C1)*(B2-B4)/B3 + (C4-C2)/C1 
u1c = ( (C3/C1)*(B2-B4)/B3 + (C4-C2)/C1 )/(1 - (C3/C1)*(B1/B3))
print('Conclusion: When v1 = ',v1,'and v2 = ',v2,'\n ','\t'*6,' u1 = ',u1c)

v1 = np.pi
v2 = 0
print('\nCase 3: When v1 = ', v1,' and v2 = ',v2)

#Eqn X: X1u1+X2 = X3
#u1*( np.tan(phi)*np.sqrt(1-(np.tan(phi))**2)*np.sin(v1) )
#+(d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)+t[5])*np.tan(phi))*np.sqrt(1-(np.tan(phi))**2)*np.sin(v1) = p*np.sin(v2)) 
X1 =  np.tan(phi)*np.sqrt(1-(np.tan(phi))**2)*np.sin(v1) 
X2 = (d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)+t[5])*np.tan(phi))*np.sqrt(1-(np.tan(phi))**2)*np.sin(v1) 
X3 = p*np.sin(v2)

#Eqn A: B1u1+AB2 = B3u2+AB4
#u1*( np.tan(phi)*np.sqrt(1-(np.tan(phi))**2)*np.cos(v) )
#+(d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)+t[5])*np.tan(phi))*np.sqrt(1-(np.tan(phi))**2)*np.cos(v1) 
#= u2*( -PQhat[1] )+t[2]*PQhat[1] + Q[1] + p*PQhat[2]*np.cos(v2)
B1 = np.tan(phi)*np.sqrt(1-(np.tan(phi))**2)*np.cos(v1)
B2 = (d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)+t[5])*np.tan(phi))*np.sqrt(1-(np.tan(phi))**2)*np.cos(v1) 
B3 = -PQhat[1]
B4 = t[2]*PQhat[1] + Q[1] + p*PQhat[2]*np.cos(v2)

print(B1,'u1 + ',B2,' = ',B3,'u2 + ',B4)

#Eqn B: C1u1+C2 = C3u2+C4
#u1*( (np.tan(phi))**2-1 )
#+H-c*np.sin(phi)+t[5] +(d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)+t[5])*np.tan(phi))*np.tan(phi) 
#= u2*( -PQhat[2] )+t[2]*PQhat[2] + Q[2] - p*PQhat[1]*np.cos(v2)
C1 = (np.tan(phi))**2-1
C2 = H-c*np.sin(phi)+t[5] +(d*np.tan(phi) + e/np.cos(phi)-(H-c*np.sin(phi)+t[5])*np.tan(phi))*np.tan(phi) 
C3 = -PQhat[2]
C4 = t[2]*PQhat[2] + Q[2] - p*PQhat[1]*np.cos(v2)

print(C1,'u1 + ',C2,' = ',C3,'u2 + ',C4)

#B1*u1 + B2 = B3*u2 + B4
#u2 = (B1/B3)*u1 + (B2-B4)/B3
#C1*u1 + C2 = C3*u2 + C4
#u1 = (C3/C1)*u2 + (C4-C2)/C1 = (C3/C1)*( (B1/B3)*u1 + (B2-B4)/B3 ) + (C4-C2)/C1
#= (C3/C1)*(B1/B3)*u1 + (C3/C1)*(B2-B4)/B3 + (C4-C2)/C1 
u1d = ( (C3/C1)*(B2-B4)/B3 + (C4-C2)/C1 )/(1 - (C3/C1)*(B1/B3))
print('Conclusion: When v1 = ',v1,'and v2 = ',v2,'\n ','\t'*6,' u1 = ',u1d)
:::
<!---
:::{code-cell} python
:tags: hide-input
r1const = e/np.cos(phi)-(H+b*theta+magPQ+a*(np.pi+theta))*np.tan(phi)

#eqn A: A1+A2u1 = A3+A4u2
A1 = r1const*np.sqrt(1-(np.tan(phi))**2)
A2 = (np.tan(phi))**2
A3 = Q[1]+(d+b*theta)*PQhat[1]+p*PQhat[2]
A4 = -PQhat[1]

print('Eqn A: ',A1,' + ',A2,'u1 = ',A3,' + ',A4,' u2')

#eqn B: B1+B2u1 = B3+B4u2
B1 = H+d+b*theta+magPQ+a*(np.pi+theta)+r1const*np.tan(phi)
B2 = (np.tan(phi))**2-1
B3 = Q[2] + (d+b*theta)*PQhat[2]-p*PQhat[1]
B4 = -PQhat[2]

print('Eqn B: ',B1,' + ',B2,'u1 = ',B3,' + ',B4,' u2')

print('\nFrom B: u2 = ',(B1-B3)/B4,' + ',B2/B4,'u1')
print('So, \n',A1,' + ',A2,'u1 = ',A3 + A4*(B1-B3)/B4,' + ',A4*B2/B4,'u1')
print(A2-(A4*B2/B4),'u1 = ',A3+A4*(B1-B3)/B4-A1)
print('u1 = ',(A3+A4*(B1-B3)/B4-A1)/(A2-(A4*B2/B4)))


print(usplit[5])
tol = 1
for u2 in usplit[2]:
    for u1 in usplit[5]:
        if (A1+A2*u1 - (A3+A4*u2))**2<tol:
            print('u1=',u1,' and u2=',u2)


:::


For the pattern example in [](#patt_eg), the parameter values were $H =$ {eval}`float(np.round(H,dp))`, $a =$ {eval}`a`, $b =$ {eval}`b`, $d =$ {eval}`float(np.round(d,dp))`, and $\theta =$ {eval}`theta`. 

(One can also check that
$$
\begin{aligned}
\left\Vert \overrightarrow{PQ}\right\Vert &= \sqrt{\left(b-a-(a+b)\cos\theta\right)^2+\left((a+b)\sin\theta+d-H\right)^2} \\
&= \sqrt{ (b-a)^2+2(a-b)(a+b)\cos\theta +(a+b)^2-2(a+b)h\sin\theta+(H-d)^2} \\
&= \sqrt{ (b-a)^2 -(a+b)^2+(H-d)^2} \\
&= \sqrt{(H-d)^2-4ab},
\end{aligned}
$$
where we have applied [](#eq:hab-constraint) to the second line.)
--->