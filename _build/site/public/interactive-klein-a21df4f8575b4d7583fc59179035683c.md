---
kernelspec:
  name: python3
  display_name: 'Python 3'
---

# Interactive Klein bottle directrix

As discussed in [](#klein), a Klein bottle is an example of a channel surface with directrix as in [](#directrix-param). 

Below is an interactive widget version of [](#fig:klein-directrix1).

To use it, click the power and play buttons displayed somewhere in the top right-hand side of the page.

:::{code-cell} python
:label: interactive-klein
import numpy as np
import matplotlib.pyplot as plt

import ipywidgets as widgets
from ipywidgets import Layout

import numpy as np
import matplotlib.pyplot as plt
%matplotlib ipympl

plt.rcParams['text.usetex'] = True

d = widgets.FloatSlider(value=-1,min=-5,max=0,step=.1,description='d')
a = widgets.FloatSlider(value=1,min=0,max=5,step=.1,description='a')
b = widgets.FloatSlider(value=2,min=0,max=5,step=.1,description='b')
theta = widgets.FloatSlider(value=1,min=0,max=np.pi/2,step=.1,description='θ')

def f(d,a,b,theta):
    plt.clf()
    plt.close()
    
    h = (a+b+(a-b)*np.cos(theta))/np.sin(theta)
    
    P = np.array([0,a*(1+np.cos(theta)),h-a*np.sin(theta)])    
    Q = np.array([0,b*(1-np.cos(theta)),b*np.sin(theta)])
    PQ = Q-P
    magPQ = np.sqrt(PQ[1]**2+PQ[2]**2)
    
    fig = plt.figure(figsize = (7,8), label = ' ')
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
    dp=2
    
    if b<=a:
        ax.set_yticks([0,b,a], labels=["0", str(float(np.round(b,dp))), str(float(np.round(a,dp)))])
    else:
        ax.set_yticks([0,a,b], labels=["0", str(float(np.round(a,dp))), str(float(np.round(b,dp)))])
    
    ax.set_zticks([d, 0,h,h+a], labels=[str(d), "0", str(float(np.round(h,dp))), str(float(np.round(h+a,dp)))])
    
    ax.set_aspect('equal')
    
    ax.view_init(elev=0, azim=0, roll=0)

output =  widgets.interactive_output(f, {'d': d, 'a': a, 'b': b, 'theta': theta})

parameters = widgets.VBox([widgets.HTML(value="Bottle control points:"), d, a, b, theta],layout=Layout())
display(parameters)

:::