---
kernelspec:
  name: python3
  display_name: 'Python 3'
---

# Interactive toral geodesics

Press power/play buttons for interactivity.

See [](#geodesics) for mathematical background.


:::{code-cell}
:label: interactive-toral-geos
:tag: hide-input

import ipywidgets as widgets
from ipywidgets import Layout

import matplotlib.pyplot as plt
from matplotlib import colormaps

import numpy as np
from scipy.integrate import solve_ivp

plt.style.use('seaborn-v0_8-poster')

# torus dimensions
R = widgets.FloatText(value=4,min=0,max=30,step=.01,description='Major radius')
r = widgets.FloatText(value=1.78,min=0,max=30,step=.01,description='Minor radius')

# Inital point
theta_0 = widgets.FloatText(value=np.pi,min=-np.pi,max=np.pi,step=.1,description='Initial θ')
phi_0 = widgets.FloatText(value=np.pi/6,min=0,max=2*np.pi,step=.1,description='Initial ϕ')

# c parameter
c = widgets.FloatText(value=2,step=.01,description='c')

# arclength
tmax = widgets.FloatText(value=100,step=.1,description='tmax')


def g(R,r,theta_0,phi_0,c,tmax):
    if abs(c) >= R-r:
        print("Error! |c| must not exceed ", R-r)
    else:
        # TORUS GEODESIC EQUATIONS 
        # Geodesic from P=r(t0,f0) with specified initial angle 
        def F(t,y): #y=(theta,phi), dy/dt=F
            theta_dot = (1/r)*np.sqrt(1-c**2/(R-r*np.cos(y[0]))**2)
            phi_dot = c/(R-r*np.cos(y[0]))**2
            return [theta_dot,phi_dot]
            
        # GEODESIC
        t0, t1 = [0,tmax]
        t_eval = np.arange(t0, t1, 0.01)
        geodesic = solve_ivp(F, [t0, t1],[theta_0,phi_0], t_eval=t_eval)
        
        #PLOTTING
        # Create a grid of the u and v values for torus surface
        u = np.linspace(-np.pi, np.pi, 100)
        v = np.linspace(0, 2 * np.pi, 100)
        u, v = np.meshgrid(u, v)
        
        # Plot the torus
        ax = plt.figure().add_subplot(projection='3d')
        ax.view_init(elev=28, azim=60, roll=0)
        x = (R - r * np.cos(u)) * np.cos(v)
        y = (R - r * np.cos(u)) * np.sin(v)
        z = r * np.sin(u)
        ax.plot_surface(x, y, z,cmap='viridis', edgecolor='black', linewidth=.1, alpha=0.5)
        
        # Plot geodesic from P
        x = (R - r * np.cos(geodesic.y[0])) * np.cos(geodesic.y[1])
        y = (R - r * np.cos(geodesic.y[0])) * np.sin(geodesic.y[1])
        z = r * np.sin(geodesic.y[0])
        
        ax.plot3D(x, y, z,linewidth=3,color='k')
        
        
        # PLOT P    
        x = (R - r * np.cos(theta_0)) * np.cos(phi_0)
        y = (R - r * np.cos(theta_0)) * np.sin(phi_0)
        z = r * np.sin(theta_0)
        ax.scatter(x,y,z,s=30,color='k')
        
        
        # Axis limits
        ax.set(zlim=(-R-r,R+r))
        
        # Set axes label
        ax.set_xlabel('x', labelpad=20)
        ax.set_ylabel('y', labelpad=20)
        ax.set_zlabel('z', labelpad=20)
        
        plt.show()

out = widgets.interactive_output(g, {'R': R, 'r':r, 'theta_0': theta_0, 'phi_0': phi_0 ,'c': c, 'tmax': tmax})

widgets.HBox(
    children = [widgets.VBox([R,r,theta_0,phi_0,c,tmax]),widgets.HBox([out],layout=Layout(max_width='50%'))],
    layout = Layout(display='flex',flex_flow='row wrap',align_items='center',justify_content='space-around')
)
:::