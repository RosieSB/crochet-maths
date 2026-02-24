---
kernelspec:
  name: python3
  display_name: 'Python 3'
---

# Interactive torus pattern

:::{tip} 
Press the power button for full interactivity.
:::

## Pattern instructions
**Upper half:** Begin with a foundation chain joined into a loop. Work the following stitch counts in each row. Increases are made using 2 st in st of previous row, evenly distributed for a symmetrical look. 

:::{code-cell} Python
:label: interactive-torus
:tag: remove-input
import ipywidgets as widgets
from ipywidgets import Layout
import numpy as np

R = widgets.FloatText(value=4,min=0,max=30,step=.01,description='Major radius')
r = widgets.FloatText(value=1.78,min=0,max=30,step=.01,description='Minor radius')
w = widgets.FloatText(value=.4,min=0,max=5,step=.01,description='Stitch width')
h = widgets.FloatText(value=.4,min=0,max=5,step=.01,description='Stitch height')


def f(R,r,w,h):
    print('Stitch counts per row:','\n','-------')
    N = round(r * np.pi/h)
    st_count = [0]*(int(N)+1)
    for n in range(int(N)+1):
        st_count[n]=round(2*np.pi*(R-r*np.cos(n*np.pi/int(N)))/w)
        if n<10:
            print('Row  ',n,': ',st_count[n],' st')
        else:
            print('Row ',n,': ',st_count[n],' st')

out = widgets.interactive_output(f, {'R': R, 'r': r, 'w': w, 'h': h})

widgets.VBox([widgets.HTML(value="Choose pattern parameters:"),
              widgets.GridBox([R,r,w,h],),
              out])
:::

**Lower half:** Same as upper half, but with first and last rows omitted. 

**Finishing:** Stuff and sew up. Weave in ends.