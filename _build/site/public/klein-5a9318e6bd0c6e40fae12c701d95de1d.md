---
kernelspec:
  name: python3
  display_name: 'Python 3'
---
(klein)=
# Klein bottle crochet

:::{code-cell} Python
:label: glance-pattern
:tag: remove-input
import numpy as np
import matplotlib.pyplot as plt
from matplotlib import cm
#%matplotlib ipympl
from tabulate import tabulate

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


#Model
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
#fig2, directrix = plt.subplots(1, 1, figsize=(6, 9))

#directrix.set_title('Directrix')
#surf.set_title('Surface')

#fig = plt.figure(figsize=plt.figaspect(.5))
#fig.suptitle('Klein bottle as canal surface')

#directrix = fig.add_subplot(1, 2, 1)
#surf = fig.add_subplot(1, 2, 2, projection='3d')

#directrix.set_title('Directrix')
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
#surf.plot_surface(x0, y0, z0, edgecolor = 'gray', linewidth = .1, alpha = .25)
#directrix.plot(gamma0[1],gamma0[2])

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
#surf.plot_surface(x1, y1, z1, edgecolor = 'gray', linewidth = .1, alpha = .25)
#directrix.plot(gamma1[1],gamma1[2])

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
#surf.plot_surface(x2, y2, z2, edgecolor = 'gray', linewidth = .1, alpha = .25)
#directrix.plot(gamma2[1],gamma2[2])

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
#surf.plot_surface(x3, y3, z3, edgecolor = 'gray', linewidth = .1, alpha = .25)
#directrix.plot(gamma3[1],gamma3[2])

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
#surf.plot_surface(x4, y4, z4, edgecolor = 'gray', linewidth = .1, alpha = .25)
#directrix.plot(gamma4[1],gamma4[2])


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
#surf.plot_surface(x5, y5, z5, edgecolor = 'gray', linewidth = .1, alpha = .25)
#directrix.plot(gamma5[1],gamma5[2])


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
#surf.plot_surface(x6, y6, z6, edgecolor = 'gray', linewidth = .1, alpha = .25)
#directrix.plot(gamma6[1],gamma6[2])


#Base 7
#Not canal
u = usplit[7]
gamma7 = (0*u,0*u,d*np.sin(u))
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x7 = ((e+p)/2+((e-p)/2)*np.cos(u))*np.sin(v)
y7 = ((e+p)/2+((e-p)/2)*np.cos(u))*np.cos(v)
z7 = d-d*np.sin(u)
#surf.plot_surface(x7, y7, z7, edgecolor = 'gray', linewidth = .1, alpha = .25)
#directrix.plot(gamma7[1],gamma7[2])

x = np.concatenate([x0,x1,x2,x3,x4,x5,x6,x7],axis=1)
y = np.concatenate([y0,y1,y2,y3,y4,y5,y6,y7],axis=1)
z = np.concatenate([z0,z1,z2,z3,z4,z5,z6,z7],axis=1)

surf.plot_surface(x,y,z, cmap='viridis',  alpha = .5)


surf.set_aspect('equal')
surf.view_init(elev=10, azim=-30, roll=0)
#surf.set_axis_off()

plt.show()
:::

## Pattern example

### Handle
::::{figure}
:label: handle-fig
:::{code-cell} python
:label: handle
:tags: remove-input
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
#ax.set_axis_off()

plt.show()
:::

Klein bottle handle
::::

:::{code-cell} python
:label: klein-base-pattern-1
:tags: remove-input

#stitch dimensions
w = .4
h = .4

#Base 0
u0 = np.flip(usplit[0])
z = d-d*np.sin(u0)
E0 = ( (e-p)*np.sin(u0)/2 )**2 + ( d*np.cos(u0) )**2
G0 = ((e+p)/2+((e-p)/2*np.cos(u0)))**2
l = len(u0)
du0 = np.zeros(l)
for j in range(1,l):
    du0[j] = u0[j-1]-u0[j]

ds0 = np.sqrt(E0)*du0

l = len(u0)
dz0 = np.zeros(l)
for j in range(1,l):
    dz0[j] = z[j]-z[j-1]

s0 = np.zeros(l)
for j in range(1,l):
    s0[j] = s0[j-1]+ds0[j]

row0 = [int(i/h) for i in s0]

#circumference
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

index=[0]
for j in range(1,l):
    if row0[j]>row0[j-1]:
        index = np.append(index,j)

index = [int(i) for i in index]

base0 = base0[index]
:::


#### Inner base

:::{code-cell} python
:label: inner-base
:tags: remove-input

fig, ax = plt.subplots(1, 1, figsize=(4, 6), subplot_kw={'projection': '3d'})

ax.plot_surface(x0,y0,z0, cmap='viridis',  alpha = .5)

ax.set_aspect('equal')
ax.view_init(elev=10, azim=-30, roll=0)
ax.set_axis_off()
plt.show()
:::

**Row 0:** Make a foundation chain of {eval}`int(base0[0,1])` stitches, and join into a ring. 

**Rows 1–{eval}`max(row0)`:**  Work the following stitch counts evenly throughout each round to form the inside of the base/very bottom of the handle:

:::{code-cell} python
:label: handle-table
:tags: remove-input
table = tabulate(base0,headers=['Row','Stitch count','u','z','E','G','du','dz','ds','s','Circumference'],tablefmt='html')
display(table)
:::

#### Lower curved section.


:::{code-cell} python
:label: lower-handle
:tags: remove-input
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

:::{code-cell} python
:label: pipe-turns-for-handle
:tags: remove-input

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

#Row count shorthand
rc = [
    max(row0)+1,
    max(row0)+2*int(np.round(theta/turning_angle_b))+1,
    max(row0)+2*int(np.round(theta/turning_angle_b))+2,
    max(row0)+2*int(np.round(theta/turning_angle_b))+int(np.round(magPQ/h))+2,
    max(row0)+2*int(np.round(theta/turning_angle_b))+int(np.round(magPQ/h))+3,
    max(row0)+2*int(np.round(theta/turning_angle_b))+int(np.round(magPQ/h))+2*int(np.round((np.pi+theta)/turning_angle_a))+3
    ]
:::

Turn work upside down, to work the next round into the other side of the foundation chain.
:::{tip} Important note!
To have the "right side" facing outwards for the majority of the visible surface, you should crochet with stitches facing the **opposite** way around for this next section of work.
:::

**Rows {eval}`rc[0]`–{eval}`rc[1]`:** Repeat the following two rows {eval}`int(np.round(theta/turning_angle_b))` times.
 - Work {eval}`int(base0[0,1])` dc around. 
 - Work a short row of {eval}`dual_st_count_b[1]` stitches, placed on outside of bend.

Pipe should have turned approximately {eval}`float(np.round(theta,dp))` radians.



**Rows {eval}`rc[2]`–{eval}`rc[3]`:** {eval}`dual_st_count_b[0]` dc in each st around, {eval}`int(np.round(magPQ/h))` times.

:::{code-cell} python
:label: straight-section
:tags: remove-input
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

**Rows {eval}`rc[4]`–{eval}`rc[5]`:** 
Repeat the following two rows {eval}`int(np.round((np.pi+theta)/turning_angle_a))` times:
 - Work {eval}`dual_st_count_a[0]` dc around. 
 - Work another short row, this time of {eval}`dual_st_count_a[1]` stitches, and placed on the **opposite** side to the previous short rows, so that the pipe turns in the opposite direction.

:::{code-cell} python
:label: lower-handle
:tags: remove-input
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

Pipe should have turned approximately {eval}`float(np.round(np.pi+theta,dp))` radians.

### Main body - still need to sort self-intersection


:::{code-cell} python
:label: body-pattern
:tags: remove-input

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

s = np.zeros(l)
for j in range(1,l):
    s[j] = s[j-1]+ds[j]

row = [rc[5]+1+int(i/h) for i in s]

#circumference
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

index=[0]
for j in range(1,l):
    if row[j]>row[j-1]: 
        index = np.append(index,j)

index = [int(i) for i in index]

data = data[index]

table = tabulate(data,headers=['Row','Stitch count','u','z','E','G','du','dz','ds','s','Circumference'],tablefmt='html')

rc = np.concatenate([rc,rc[5]+1,max(row)],axis=None)
rc = [int(i) for i in rc]
:::

:::::{grid} 1 1 2 2
::::{grid-item}
Worked from the top down.


::::
::::{grid-item}
:::{code-cell} python
:tags: remove-input
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
::::
:::::


**Rows {eval}`min(row)`–{eval}`max(row)`:** Work the following row counts, spacing increases evenly throughout each round.

:::{code-cell} python
:tags: remove-input
display(table)
:::
## Derivation

The majority of the Klein bottle pattern comes from its parametrisation as a canal surface. Discussion of the choice of directrix and radius function are below.

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
The gradient of $L$ is
$$
\cot\theta = \frac{d+b\sin\theta - H + a\sin\theta}{b-b\cos\theta - a-a\cos\theta},
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
:::{figure} figs/klein-radius.png
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
- $r_3(u)=p+c-\sqrt{c^2-(u-H)^2}$, $\dot r_3(u)=\frac{u-H}{\sqrt{c^2-(u-H)^2}}$ for $u\in[H-c\sin\phi,H]$.

For canal surface, it is required that $|\dot r|<\Vert\dot\gamma\Vert=1$, The smaller $|\dot r|$, the smoother (less lumpy) the resulting surface.

### Matplotlib model

::::{figure}
:::{code-cell} python
:label: klein-canal
:tags: hide-input

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
    e*np.sin(phi)+d,
    H-c*np.sin(phi), 
    H, 
    H+a*(np.pi+theta), 
    H+a*(np.pi+theta)+magPQ, 
    H+a*(np.pi+theta)+magPQ+b*theta,
    H+d+a*(np.pi+theta)+magPQ+b*theta
])

n=100

usplit = [np.linspace(-np.pi, 0, n),   #t[0], t[1], n),
          np.linspace(t[1], t[2], 2*n),
          np.linspace(t[2], t[3], n),
          np.linspace(t[3], t[4], n),
          np.linspace(t[4], t[5], n),
          np.linspace(t[5], t[6], n),
          np.linspace(t[6], t[7], n),
          np.linspace(t[7], t[8], n)]


# Set up a figure and axes
#fig1, surf = plt.subplots(1, 1, figsize=(6, 9), subplot_kw={'projection': '3d'})
#fig2, directrix = plt.subplots(1, 1, figsize=(6, 9))

#directrix.set_title('Directrix')
#surf.set_title('Surface')

fig = plt.figure(figsize=plt.figaspect(.5))
fig.suptitle('Klein bottle as canal surface')

directrix = fig.add_subplot(1, 2, 1)
surf = fig.add_subplot(1, 2, 2, projection='3d')

directrix.set_title('Directrix')
surf.set_title('Surface')


#Base 0
#Not canal
u = usplit[0]
gamma0 = (0*u,0*u,-d*np.sin(u))
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x0 = ((e+p)/2+((e-p)/2)*np.cos(u))*np.sin(v)
y0 = ((e+p)/2+((e-p)/2)*np.cos(u))*np.cos(v)
z0 = d+d*np.sin(u)
surf.plot_surface(x0, y0, z0, edgecolor = 'gray', linewidth = .1, alpha = .25)
directrix.plot(gamma0[1],gamma0[2])

#Bottle canal 1
u = usplit[1]
r1 = np.sqrt(e**2-(u-d)**2)
r1dot = -(u-d)/np.sqrt(e**2-(u-d)**2)
r1ddot = -e**2*np.power((e**2-(u-d)**2),-1.5)
gamma1 = (0*u,0*u,u)
T = (0,0,1)
N = (0,1,0)
B = (1,0,0)
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x1 = gamma1[0] - r1*r1dot*T[0] + r1*np.sqrt(1-(r1dot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y1 = gamma1[1] - r1*r1dot*T[1] + r1*np.sqrt(1-(r1dot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z1 = gamma1[2] - r1*r1dot*T[2] + r1*np.sqrt(1-(r1dot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v))

surf.plot_surface(x1, y1, z1, edgecolor = 'gray', linewidth = .1, alpha = .25)
directrix.plot(gamma1[1],gamma1[2])

#Bottle canal 2
u = usplit[2]
r = e/np.cos(phi)-(u-d)*np.tan(phi)
rdot = -np.tan(phi)
gamma2 = (0*u,0*u,u)
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x2 = gamma2[0] - r*rdot*T[0] + r*np.sqrt(1-(rdot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y2 = gamma2[1] - r*rdot*T[1] + r*np.sqrt(1-(rdot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z2 = gamma2[2] - r*rdot*T[2] + r*np.sqrt(1-(rdot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v))
surf.plot_surface(x2, y2, z2, edgecolor = 'gray', linewidth = .1, alpha = .25)
directrix.plot(gamma2[1],gamma2[2])

#Bottle canal 3
u = usplit[3] #[H-c*np.sin(phi), H]
r = p+c-np.sqrt(c**2-(u-H)**2)
rdot = (u-H)/np.sqrt(c**2-(u-H)**2)
gamma3 = (0*u,0*u,u)
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x3 = gamma3[0] - r*rdot*T[0] + r*np.sqrt(1-(rdot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y3 = gamma3[1] - r*rdot*T[1] + r*np.sqrt(1-(rdot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z3 = gamma3[2] - r*rdot*T[2] + r*np.sqrt(1-(rdot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v))
surf.plot_surface(x3, y3, z3, edgecolor = 'gray', linewidth = .1, alpha = .25)
directrix.plot(gamma3[1],gamma3[2])

#Handle 4
u = usplit[4]
r = p
rdot = 0
gamma4 = (0*u, a*(1+np.cos(np.pi-(u-t[4])/a)), H+a*np.sin(np.pi-(u-t[4])/a))
T = (0,np.sin(np.pi-(u-t[4])/a),-np.cos(np.pi-(u-t[4])/a))
N = (0,-np.cos(np.pi-(u-t[4])/a),-np.sin(np.pi-(u-t[4])/a))
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x4 = gamma4[0] - r*rdot*T[0] + r*np.sqrt(1-(rdot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y4 = gamma4[1] - r*rdot*T[1] + r*np.sqrt(1-(rdot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z4 = gamma4[2] - r*rdot*T[2] + r*np.sqrt(1-(rdot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v))
surf.plot_surface(x4, y4, z4, edgecolor = 'gray', linewidth = .1, alpha = .25)
directrix.plot(gamma4[1],gamma4[2])

#Handle 5
u = usplit[5]
gamma5 = (0*u,((u-t[5])/magPQ)*Q[1] + (1-(u-t[5])/magPQ)*P[1],((u-t[5])/magPQ)*Q[2] + (1-(u-t[5])/magPQ)*P[2])
T = (0,PQhat[1],PQhat[2])
N = (0,PQhat[2],-PQhat[1])
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x5 = gamma5[0] - r*rdot*T[0] + r*np.sqrt(1-(rdot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y5 = gamma5[1] - r*rdot*T[1] + r*np.sqrt(1-(rdot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z5 = gamma5[2] - r*rdot*T[2] + r*np.sqrt(1-(rdot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v))
surf.plot_surface(x5, y5, z5, edgecolor = 'gray', linewidth = .1, alpha = .25)
directrix.plot(gamma5[1],gamma5[2])

#Handle 6
u = usplit[6]
gamma6 = (0*u, b+b*np.cos(np.pi-theta+(u-t[6])/b), d+b*np.sin(np.pi-theta+(u-t[6])/b))
T = (0,-np.sin(np.pi-theta+(u-t[6])/b),np.cos(np.pi-theta+(u-t[6])/b))
N = (0,np.cos(np.pi-theta+(u-t[6])/b),np.sin(np.pi-theta+(u-t[6])/b))
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x6 = gamma6[0] - r*rdot*T[0] + r*np.sqrt(1-(rdot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y6 = gamma6[1] - r*rdot*T[1] + r*np.sqrt(1-(rdot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z6 = gamma6[2] - r*rdot*T[2] + r*np.sqrt(1-(rdot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v))
surf.plot_surface(x6, y6, z6, edgecolor = 'gray', linewidth = .1, alpha = .25)
directrix.plot(gamma6[1],gamma6[2])

#Base 7
u = usplit[7]
r = (p+e)/2-((e-p)/2)*np.sqrt(1-((u-u[0])/d)**2)
gamma7 = (0*u,0*u,H+d+a*(np.pi+theta)+magPQ+b*theta-u)
T = (0,0,-1)
N = (0,-1,0)
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x7 = gamma7[0] + r*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y7 = gamma7[1] + r*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z7 = gamma7[2] + r*(N[2]*np.cos(v)+B[2]*np.sin(v))
surf.plot_surface(x7, y7, z7, edgecolor = 'gray', linewidth = .1, alpha = .25)
#directrix.plot(gamma7[1],gamma7[2])

surf.set_aspect('equal')
surf.view_init(elev=0, azim=0, roll=0)
xyticks = [-b,-a,0,a,b,P[1],Q[1]]
zticks = [0,d,z2[0,0],z3[0,0],z4[0,0],z5[0,0],z6[0,0],z7[0,0]]
surf.set_xticks(xyticks)
surf.set_yticks(xyticks)
surf.set_zticks(zticks)

directrix.set_aspect('equal')
directrix.set_xticks([0,a,b,P[1],Q[1]])
directrix.set_yticks([t[0],t[1],t[2],t[3],t[4],P[2],Q[2]])
directrix.grid(True)

plt.show()

:::
::::


### Canal surface vs surface of revolution

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

X = np.array([0,e*np.cos(phi),d+e*np.sin(phi)])
Y = np.array([0,p+c-c*np.cos(phi),H-c*np.sin(phi)])

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

## Towards crochetability
The first problem to solve is to decide how to measure rows. The most accurate would be to measure them along meridians of constant $v$, according to the canal surface parametrisation.

The line element/Riemannian metrix for a parametrised surface $\mathbf{x}(u,v)$ is
$$
ds^2 = Edu^2 + 2Fdudv + Gdv^2,
$$
where coefficients $E=\mathbf{x}_u^2$, $F=\mathbf{x}u\cdot\mathbf{x}_v$ and $F=\mathbf{x}_v^2$. We should calculate $E$ for row count, as this measures distances along constant $v$-meridians.

### Main body excluding base
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

### Base
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

## Pattern 

### Body
:::{code-cell} python
:label: body-patt
:tags: remove-input
import numpy as np
import matplotlib.pyplot as plt
from matplotlib import cm
%matplotlib ipympl
from tabulate import tabulate

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

X = np.array([0,e*np.cos(phi),d+e*np.sin(phi)])
Y = np.array([0,p+c-c*np.cos(phi),H-d-c*np.sin(phi)])

t = np.array([
    0,
    d, 
    e*np.sin(phi)+d,
    H-c*np.sin(phi), 
    H, 
    H+a*(np.pi+theta), 
    H+a*(np.pi+theta)+magPQ, 
    H+a*(np.pi+theta)+magPQ+b*theta,
    H+d+a*(np.pi+theta)+magPQ+b*theta
])

n=1000

usplit = [np.linspace(-np.pi, 0, n),   #t[0], t[1], n),
          np.linspace(t[1], t[2], 2*n),
          np.linspace(t[2], t[3], n),
          np.linspace(t[3], t[4], n),
          np.linspace(t[4], t[5], n),
          np.linspace(t[5], t[6], n),
          np.linspace(t[6], t[7], n),
          np.linspace(t[7], t[8], n)]


# Set up a figure and axes
fig = plt.figure()
ax = fig.add_subplot(projection='3d',label=' ')

#Base 0
#Not canal
u = usplit[0]
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x = ((e+p)/2+((e-p)/2)*np.cos(u))*np.sin(v)
y = ((e+p)/2+((e-p)/2)*np.cos(u))*np.cos(v)
z = d+d*np.sin(u)
ax.plot_surface(x, y, z, edgecolor = 'none', linewidth = .1, alpha = .25)

#Bottle canal 1
u = usplit[1]
r1 = np.sqrt(e**2-(u-d)**2)
r1dot = -(u-d)/np.sqrt(e**2-(u-d)**2)
r1ddot = -e**2*np.power((e**2-(u-d)**2),-1.5)
gamma = (0,0,u)
T = (0,0,1)
N = (0,1,0)
B = (1,0,0)
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x = gamma[0] - r1*r1dot*T[0] + r1*np.sqrt(1-(r1dot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y = gamma[1] - r1*r1dot*T[1] + r1*np.sqrt(1-(r1dot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z = gamma[2] - r1*r1dot*T[2] + r1*np.sqrt(1-(r1dot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v))

ax.plot_surface(x, y, z, edgecolor = 'none', linewidth = .1, alpha = .25)

#Bottle canal 2
u = usplit[2]
r2 = e/np.cos(phi)-(u-d)*np.tan(phi)
r2dot = -np.tan(phi)*np.ones(len(u))
r2ddot = np.zeros(len(u))
gamma = (0,0,u)
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x = gamma[0] - r2*r2dot*T[0] + r2*np.sqrt(1-(r2dot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y = gamma[1] - r2*r2dot*T[1] + r2*np.sqrt(1-(r2dot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z = gamma[2] - r2*r2dot*T[2] + r2*np.sqrt(1-(r2dot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v))

ax.plot_surface(x, y, z, edgecolor = 'none', linewidth = .1, alpha = .25)

#Bottle canal 3
u = usplit[3]
r3 = p+c-np.sqrt(c**2-(u-t[4])**2)
r3dot = (u-t[4])/np.sqrt(c**2-(u-t[4])**2)
r3ddot = c**2*np.power(c**2-(u-H)**2,-1.5)
gamma = (0,0,u)
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x = gamma[0] - r3*r3dot*T[0] + r3*np.sqrt(1-(r3dot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y = gamma[1] - r3*r3dot*T[1] + r3*np.sqrt(1-(r3dot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z = gamma[2] - r3*r3dot*T[2] + r3*np.sqrt(1-(r3dot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v))

ax.plot_surface(x, y, z, edgecolor = 'none', linewidth = .1, alpha = .25)

#Handle 4
u = usplit[4]
r = p
rdot = 0
gamma = (0, a*(1+np.cos(np.pi-(u-t[4])/a)), H+a*np.sin(np.pi-(u-t[4])/a))
T = (0,np.sin(np.pi-(u-t[4])/a),-np.cos(np.pi-(u-t[4])/a))
N = (0,-np.cos(np.pi-(u-t[4])/a),-np.sin(np.pi-(u-t[4])/a))
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x = gamma[0] - r*rdot*T[0] + r*np.sqrt(1-(rdot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y = gamma[1] - r*rdot*T[1] + r*np.sqrt(1-(rdot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z = gamma[2] - r*rdot*T[2] + r*np.sqrt(1-(rdot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v))

#ax.plot_surface(x, y, z, edgecolor = 'black', linewidth = .1, alpha = .25)

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

#ax.plot_surface(x, y, z, edgecolor = 'black', linewidth = .1, alpha = .25)

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

#ax.plot_surface(x, y, z, edgecolor = 'black', linewidth = .1, alpha = .25)

#Base 7
u = usplit[7]
r = (p+e)/2-((e-p)/2)*np.sqrt(1-((u-u[0])/d)**2)
gamma = (0,0,H+d+a*(np.pi+theta)+magPQ+b*theta-u)
T = (0,0,-1)
N = (0,-1,0)
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x = gamma[0] + r*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y = gamma[1] + r*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z = gamma[2] + r*(N[2]*np.cos(v)+B[2]*np.sin(v))

#ax.plot_surface(x, y, z, edgecolor = 'black', linewidth = .1, alpha = .25)

ax.set_aspect('equal')
ax.view_init(elev=20, azim=0, roll=0)

plt.show()

#Pattern
#base
u0 = usplit[0]
z0 = d+d*np.sin(u0)
E0 = ( (e-p)*np.sin(u0)/2 )**2 + ( d*np.cos(u0) )**2
G0 = ((e+p)/2+((e-p)/2*np.cos(u0)))**2
l = len(u0)
du0 = np.zeros(l)
for j in range(1,l):
    du0[j] = u0[j]-u0[j-1]

#canal
u = np.concatenate([usplit[1][1:],usplit[2][1:],usplit[3][1:]])
r = np.concatenate([r1[1:],r2[1:],r3[1:]])
rdot = np.concatenate([r1dot[1:],r2dot[1:],r3dot[1:]])
rddot = np.concatenate([r1ddot[1:],r2ddot[1:],r3ddot[1:]])
z = np.zeros(l)
z = u-r*rdot
E = (1-r*rddot-rdot**2)**2 + (rdot**2*(1-rdot**2-r*rddot)**2)/(1-rdot**2)
G = r**2*(1-rdot**2)
l = len(u)
du = np.zeros(l)
for j in range(1,l):
    du[j] = u[j]-u[j-1]

#add base
u = np.concatenate([usplit[0],u])
z = np.concatenate([z0,z])
E = np.concatenate([E0,E])
G = np.concatenate([G0,G])
du = np.concatenate([du0,du])

ds = np.sqrt(E)*du

l = len(u)
dz = np.zeros(l)
for j in range(1,l):
    dz[j] = z[j]-z[j-1]

s = np.zeros(l)
for j in range(1,l):
    s[j] = s[j-1]+ds[j]

#stitch dimensions
w = .4
h = .4

row = [int(i/h) for i in s]

#circumference
C = 2*np.pi*np.sqrt(G)
st_count = np.round(C/w)

data = np.transpose([row,
                     st_count,
                     u,#r,rdot,rddot,
                     z,
                     E,
                     G,
                     du,
                     dz,
                     ds,
                     s,
                     C])

index=[0]
for j in range(1,l):
    if row[j]>row[j-1]:
        index = np.append(index,j)

index = [int(i) for i in index]

data = data[index]

table = tabulate(data,headers=['Row','Stitch count','u','z','E','G','du','dz','ds','s','Circumference'],tablefmt='html')

print('Number of rows: ',max(row))
print('Stitch width: ',w)
print('Stitch height: ',h)
display(table)
:::

### Handle
:::{code-cell} python
:label: handle-patt
:tags: remove-input

fig = plt.figure()
ax = fig.add_subplot(projection='3d',label=' ')

#Handle 4
u = usplit[4]
r = p
rdot = 0
gamma = (0, a*(1+np.cos(np.pi-(u-t[4])/a)), H+a*np.sin(np.pi-(u-t[4])/a))
T = (0,np.sin(np.pi-(u-t[4])/a),-np.cos(np.pi-(u-t[4])/a))
N = (0,-np.cos(np.pi-(u-t[4])/a),-np.sin(np.pi-(u-t[4])/a))
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x = gamma[0] - r*rdot*T[0] + r*np.sqrt(1-(rdot)**2)*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y = gamma[1] - r*rdot*T[1] + r*np.sqrt(1-(rdot)**2)*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z = gamma[2] - r*rdot*T[2] + r*np.sqrt(1-(rdot)**2)*(N[2]*np.cos(v)+B[2]*np.sin(v))

ax.plot_surface(x, y, z, edgecolor = 'none', linewidth = .1, alpha = .25)

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

ax.plot_surface(x, y, z, edgecolor = 'none', linewidth = .1, alpha = .25)

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

ax.plot_surface(x, y, z, edgecolor = 'none', linewidth = .1, alpha = .25)

#Base 7
u = usplit[7]
r = (p+e)/2-((e-p)/2)*np.sqrt(1-((u-u[0])/d)**2)
gamma = (0,0,H+d+a*(np.pi+theta)+magPQ+b*theta-u)
T = (0,0,-1)
N = (0,-1,0)
v = np.linspace(0,2*np.pi,100)
u, v = np.meshgrid(u, v)
x = gamma[0] + r*(N[0]*np.cos(v)+B[0]*np.sin(v)) 
y = gamma[1] + r*(N[1]*np.cos(v)+B[1]*np.sin(v)) 
z = gamma[2] + r*(N[2]*np.cos(v)+B[2]*np.sin(v))

ax.plot_surface(x, y, z, edgecolor = 'none', linewidth = .1, alpha = .25)

ax.set_aspect('equal')
ax.view_init(elev=20, azim=0, roll=0)

plt.show()

turning_angle_a = float(np.round(2*np.arcsin(w/(2*(a-p))),dp))
repeats_a = int(np.round((np.pi+theta)/turning_angle_a))

turning_angle_b = float(np.round(2*np.arcsin(w/(2*(b-p))),dp))
repeats_b = int(np.round(theta/turning_angle_b))
:::

Still working with stitch dimension w={eval}`w`, h={eval}`h`, work a curved pipe formed of {eval}`int(np.round(2*np.pi*p/w))` stitches using the technique described in [](#pipe). 

For the top part of the handle, use radii R=a={eval}`a` and r=p={eval}`p`, repeating the rows of [Example 1](#curved-pipe-parameters-1) until you have turned a total angle of {eval}`float(np.round(np.pi+theta,dp))` radians. Each iteration produces a turning angle of $\alpha_a=2\arcsin\left(\frac{w}{2(a-p)}\right)=${eval}`turning_angle_a`, so the number of repeats needed should be approximately $\left[\frac{\pi+\theta}{\alpha_a}\right]=${eval}`repeats_a`.

Then, work a straight section consisting of {eval}`int(np.round(magPQ/h))` rows.

Finally, work another curved pipe, this time with radii R=b={eval}`b` and r=p={eval}`p`, turning a total of {eval}`theta` radians. This is described by [Example 2](#curved-pipe-parameters-2), in which each iteration produces a turning angle of $\alpha_b=2\arcsin\left(\frac{w}{2(b-p)}\right)=${eval}`turning_angle_b`. So the number of repeats needed here should be $\left[\frac{\theta}{\alpha_b}\right]=${eval}`repeats_b`. 

(In theory) you should now match up with the last part of the base, described above.