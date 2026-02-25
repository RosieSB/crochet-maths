---
kernelspec:
  name: python3
  display_name: 'Python 3'
---

# Interactive torus pattern

::::{margin}
:::{tip} 
Press the power button for full interactivity.
:::
::::

**Upper half:** Begin with a foundation chain joined into a loop. Work the stitch counts displayed below in each row. Increases are made using 2 st in st of previous row, evenly distributed for a symmetrical look. 

**Lower half:** Same as upper half, but with first and last rows omitted. 

**Finishing:** Stuff and sew up. Weave in ends.

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
    if r>=R:
        pattern=[widgets.HTML(value='Error! Major radius must exceed minor radius')]
    else:
        N = round(r * np.pi/h)
        st_count = [0]*(int(N)+1)
        pattern = [0]*(int(N)+1)
        for n in range(int(N)+1):
            st_count[n]=round(2*np.pi*(R-r*np.cos(n*np.pi/int(N)))/w)
            #if n<10:
            #    print('  Row  ',n,': ',st_count[n],' st')
            #else:
            #    print('  Row ',n,': ',st_count[n],' st')
            pattern[n] = widgets.HBox([widgets.ToggleButton(value=False,description="Row "+str(n)+": "+str(st_count[n])+" st",indent=True)])
    display(widgets.VBox(pattern))

out = widgets.interactive_output(f, {'R': R, 'r': r, 'w': w, 'h': h})


pattern_layout = Layout(display='flex',
                    flex_flow='column wrap', 
                    align_items='center', 
                    border='solid')

widgets.HBox(
    [
    widgets.VBox([widgets.HTML(value="<center><b>Choose pattern dimensions (cm):"), R, r, w, h]),
    widgets.VBox([widgets.HTML(value="<center><b>Stitch count per row:"),out])#,layout=pattern_layout)
    ],
    layout=Layout(display='flex',align_items='center',justify_content='space-around',width='80%')
)
:::