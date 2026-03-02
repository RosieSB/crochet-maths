---
kernelspec:
  name: python3
  display_name: 'Python 3'
---
(interactive-torus)=
# Interactive torus crochet pattern

::::{margin}
:::{tip} 
Press power and play buttons to display content.
:::
::::

:::{code-cell} Python
:label: interactive-torus
:tag: hide-input

import ipywidgets as widgets
from ipywidgets import Layout

import numpy as np

from mpl_toolkits import mplot3d
import matplotlib.pyplot as plt
from matplotlib.colors import TABLEAU_COLORS, same_color
prop_cycle = plt.rcParams['axes.prop_cycle']
colors = prop_cycle.by_key()['color']

plt.style.use('seaborn-v0_8-poster')
out2 = widgets.Output()

R = widgets.FloatText(value=4,min=0,max=30,step=.01,description='Major radius')
r = widgets.FloatText(value=1.78,min=0,max=30,step=.01,description='Minor radius')
w = widgets.FloatText(value=.4,min=0,max=5,step=.01,description='Stitch width')
h = widgets.FloatText(value=.4,min=0,max=5,step=.01,description='Stitch height')

B=widgets.ToggleButton(value=False,description='Click for 3d visualisation',layout=Layout(width='50%',height='80px'))

# Previously had spherical stitches (overkill)
#def st(w,h): # Stitch of width w and heaight h, modelled as a spheroid, centered at (x0,y0,z_0)
#            u = np.linspace(0, np.pi, 10)
#            v = np.linspace(0, 2 * np.pi, 10)
#            u, v = np.meshgrid(u, v)
#            x = (w/2)*np.sin(u)*np.cos(v)
#            y = (w/2)*np.sin(u)*np.sin(v)
#            z = (h/2)*np.cos(u)
#            return x, y, z

# Pattern instructions
def f(R,r,w,h):
    if r>=R:
        pattern=[widgets.HTML(value='Error! Major radius must exceed minor radius')]
    else:
        N = round(r * np.pi/h)
        st_count = [0]*(int(N)+1)
        pattern = [0]*(int(N)+1)
        row = [0]*(int(N)+1)
        for n in range(int(N)+1):
            st_count[n]=round(2*np.pi*(R-r*np.cos(n*np.pi/int(N)))/w)
            pattern[n] = widgets.HBox([widgets.ToggleButton(value=False,description="Row "+str(n)+": "+str(st_count[n])+" st",indent=True)])
    display(widgets.VBox(pattern))


# Pattern model
def g(R,r,w,h,B):
    if r>=R:
        pattern=[widgets.HTML(value='Error! Major radius must exceed minor radius')]
    elif B==True:
        plt.clf()
        plt.close()
        N = round(r * np.pi/h)
        fig = plt.figure(figsize = (8,8))
        ax = plt.axes(projection='3d')
        ax.grid()
        for n in range(int(N)+1):
            st_cnt=round(2*np.pi*(R-r*np.cos(n*np.pi/int(N)))/w)            
            for k in range(1,st_cnt+1):
                theta = n*np.pi/N
                phi = (k-.5*n)*2*np.pi/st_cnt # slight offset to better reflect crochet stitch distribution
                u = np.linspace((n-.5)*np.pi/N, (n+.5)*np.pi/N, 2)
                v = np.linspace((k-.5-.5*n)*2*np.pi/st_cnt, (k+.5-.5*n)*2*np.pi/st_cnt, 2)
                u, v = np.meshgrid(u, v)
                x = (R-r*np.cos(u))*np.cos(v)
                y = (R-r*np.cos(u))*np.sin(v)
                z = r*np.sin(u)
                ax.plot_surface(x, y, z, color=colors[k%10], edgecolor='black', linewidth=.1)
                if n>1 and n<N:
                    ax.plot_surface(x, y, -z, color=colors[9-k%10], edgecolor='black', linewidth=.1)
        # Axis labels
        ax.set_xlabel('x', labelpad=20)
        ax.set_ylabel('y', labelpad=20)
        ax.set_zlabel('z', labelpad=20)

        # Axis limits
        ax.set(zlim=(-R-r,R+r))
    
        ax.set_aspect('equal')
        
        plt.show()       

pattern_output = widgets.interactive_output(f, {'R': R, 'r': r, 'w': w, 'h': h})

figure_output =  widgets.interactive_output(g, {'R': R, 'r': r, 'w': w, 'h': h, 'B': B})

pattern_text = widgets.VBox([
    widgets.HTML(value="<b>Upper half:</b> Begin with a foundation chain joined into a loop. Work the stitch counts displayed below in each row. Increases are made using 2 st in st of previous row, evenly distributed for a symmetrical look."),
    widgets.HTML(value="<b>Lower half:</b> Same as upper half, but with first and last rows omitted."),
    widgets.HTML(value="<b>Finishing:</b> Stuff and sew up. Weave in ends.")
])

parameters = widgets.VBox([widgets.HTML(value="<b>Choose pattern dimensions (cm):"), R, r, w, h],layout=Layout(margin='0px 50px 0px 0px'))

display(pattern_text)

widgets.VBox(
    [widgets.HBox([parameters,pattern_output],layout=Layout()),#justify_content='space-around')),
     widgets.HBox([figure_output],layout=Layout()),#justify_content='space-around')),
     widgets.HBox([B],layout=Layout(padding='10px'))#,justify_content='space-around'))
    ], 
    layout=Layout(display='flex')
)
:::