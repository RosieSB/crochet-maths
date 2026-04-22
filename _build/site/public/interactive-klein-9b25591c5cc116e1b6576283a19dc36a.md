---
kernelspec:
  name: python3
  display_name: 'Python 3'
---

# Interactive Klein bottle 


As discussed in [](#klein), a Klein bottle is an example of a channel surface with directrix as in [](#directrix-param). 

Below is an interactive widget to help determine aesthetically pleasing parameter values.

To use it, click the power and play buttons displayed somewhere in the top right-hand side of the page.


:::{code-cell} python
:label: interactive-klein-directrix
:tag: remove-input
import numpy as np
import matplotlib.pyplot as plt

import ipywidgets as widgets
from ipywidgets import Layout

import numpy as np
import matplotlib.pyplot as plt
from matplotlib import cm
%matplotlib ipympl

p = widgets.FloatText(value=1,min=0,max=5,step=.1,description='Pipe radius')
a = widgets.FloatText(value=3.4,min=0,max=5,step=.1,description='Handle radius')
b = widgets.FloatText(value=4,min=0,max=5,step=.1,description='b')
theta = widgets.FloatText(value=.7,min=0,max=np.pi/2,step=.01,description='θ')
phi = widgets.FloatText(value=.45,min=0,max=np.pi/2,step=.01,description='ϕ')
w = widgets.FloatText(value=8.6,min=0,max=20,step=.1,description='Bulb radius')

#Directrix
def g(p,a,b,theta,w):
    plt.clf()
    plt.close()
    
    h = (a+b+(a-b)*np.cos(theta))/np.sin(theta)
    d = np.sqrt( (w/2)*((w/2)-p)/2 )
    
    P = np.array([a*(1+np.cos(theta)),h-a*np.sin(theta)])    
    Q = np.array([b*(1-np.cos(theta)),b*np.sin(theta)])
    PQ = Q-P
    magPQ = np.sqrt(PQ[0]**2+PQ[1]**2)
    
    fig1 = plt.figure(figsize = (7,8), label = ' ')
    ax1 = plt.axes()#projection='3d')
    
    #Straight segment 1
    t = np.linspace(0, h-d, 2)
    
    #x = 0
    y = 0*t
    z = d+t
    
    ax1.plot( y, z, color='tab:blue')
    ax1.text( -.35*a ,h/2 ,'γ₁', color='tab:blue', fontsize=16)
    
    #Circular segment 1
    
    t = np.linspace(0, a*(np.pi+theta), 100)
    
    #x = 0
    y = a*(1+np.cos(np.pi-t/a))
    z = h+a*np.sin(np.pi-t/a)
    
    ax1.plot( y, z, color='tab:orange')
    ax1.text( 2.05*a ,1.1*h ,'γ₂', color='tab:orange', fontsize=16)
    
    #Straight segment 2
    
    t = np.linspace(0, magPQ, 2)
    
    y = (t/magPQ)*Q[0] + (1-t/magPQ)*P[0]
    z = (t/magPQ)*Q[1] + (1-t/magPQ)*P[1]
    
    ax1.plot( y, z, color='tab:green')
    ax1.text( (P[0]+Q[0])/2-.1*a ,(P[1]+Q[1])/2-.3*a ,'γ₃', color='tab:green', fontsize=16)
    
    #Circular segment 2
    
    t = np.linspace(0, b*theta, 100)
    
    #x = 0
    y = b+b*np.cos(np.pi-theta+t/b)
    z = b*np.sin(np.pi-theta+t/b)
    
    ax1.plot( y, z, color='tab:red')
    ax1.text( b+b*np.cos(np.pi-theta/2)+.1*a, b*np.sin(np.pi-theta/2)-.1*a, 'γ₄', color='tab:red', fontsize=16)
    
    #Straight segment 3
    
    t = np.linspace(0, -d, 2)
    
    #x = 0
    y = 0*t
    z = -t
    
    ax1.plot( y, z, color='tab:purple')
    ax1.text( -.35*a, d/2, 'γ₅', color='tab:purple', fontsize=16)
    
    
    #Axis labels
    ax1.set_xlabel('y')
    ax1.set_ylabel('z')
    
    ax1.set(xlim=(-1.5*a,2.5*a), ylim=(0,1.1*(h+a)))
    
    ax1.set_aspect('equal')
    
    #ax1.view_init(elev=0, azim=0, roll=0)
    plt.show()

parameters = widgets.VBox([widgets.HTML(value="Bottle control points:"), p, a, b, theta, phi, w],layout=Layout())
display(parameters)
directrix = widgets.interactive_output(g, {'p': p, 'a': a, 'b': b, 'theta': theta, 'w': w})
display(directrix)
:::

:::{code-cell} python
:label: interactive-klein-surface
:tag: remove-input
colour = 'blue'

def f(p,a,b,theta,phi,w):
    plt.clf()
    plt.close()
        
    e = w/2
    h = (a+b+(a-b)*np.cos(theta))/np.sin(theta)
    d = np.sqrt((w/2)*((w/2)-p)/2)
    c = (h*np.sin(phi)+p*np.cos(phi)-(w/2))/(1-np.cos(phi))

    P = np.array([0,a*(1+np.cos(theta)),d+h-a*np.sin(theta)])
    Q = np.array([0,b*(1-np.cos(theta)),d+b*np.sin(theta)])
    PQ = Q-P
    magPQ = np.sqrt(PQ[1]**2+PQ[2]**2)
    PQhat = PQ/magPQ

    X = np.array([0,(w/2)*np.cos(phi),d+(w/2)*np.sin(phi)])
    Y = np.array([0,p+c-c*np.cos(phi),h-c*np.sin(phi)])

    
    t = np.array([
        0,
        d, 
        (w/2)*np.sin(phi)+d, 
        h-c*np.sin(phi)+d, 
        h+d, 
        h+d+a*(np.pi+theta), 
        h+d+a*(np.pi+theta)+magPQ, 
        h+d+a*(np.pi+theta)+magPQ+b*theta,
        h+2*d+a*(np.pi+theta)+magPQ+b*theta
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

surface =  widgets.interactive_output(f, {'p': p, 'a': a, 'b': b, 'theta': theta, 'phi': phi, 'w': w})
display(parameters)
display(surface)
:::