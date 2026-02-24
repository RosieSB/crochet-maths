---
kernelspec:
  name: python3
  display_name: 'Python 3'
---

# Interactive torus pattern

:::{code-cell} Python
:label: interactive-torus

widget = widgets.HTML(value= '<style>p{word-wrap: break-word}</style> <p>'+ [variable containing long text goes here] +' </p>')import ipywidgets as widgets
from ipywidgets import Layout
import numpy as np

R = widgets.FloatText(value=4,min=0,max=30,step=.01,description='Major radius')
r = widgets.FloatText(value=1.78,min=0,max=30,step=.01,description='Minor radius')
w = widgets.FloatText(value=.4,min=0,max=5,step=.01,description='Stitch width')
h = widgets.FloatText(value=.4,min=0,max=5,step=.01,description='Stitch height')
# N = widgets.IntText(value=14,min=0,max=30,step=1,description='Number of rows')


def f(R,r,w,h):
    N = round(r * np.pi/h)
#    print('For the upper half of the torus, ',
#    'begin with a foundation chain joined into a loop. ',
#    'Work the following stitch counts in each row. ',
#    'Increases made using 2 st in st of previous row. ',
#    'Try to evenly distribute increases for a symmetrical look.', 
#    'For the lower half, turn work upside down and repeat, omitting the first last rows of the upper half pattern. Stuff and sew up.' 
    st_count = [0]*(int(N)+1)
    for n in range(int(N)+1):
        st_count[n]=round(2*np.pi*(R-r*np.cos(n*np.pi/int(N)))/w)
        if n<10:
            print('Row  ',n,': ',st_count[n],' st')
        else:
            print('Row ',n,': ',st_count[n],' st')
    print('Row ',n,': ',st_count[n],' st')

out = widgets.interactive_output(f, {'R': R, 'r': r, 'w': w, 'h': h})

pattern_text_upper = widget = widgets.HTML(value="<b> Upper half: </b> Begin with a foundation chain joined into a loop. Work the following stitch counts in each row. Increases are made using 2 st in st of previous row, evenly distributed for a symmetrical look. </p>")
pattern_text_lower = widgets.HTML(value="<b> Lower half: </b> Same as upper half, but with first and last rows omitted.")
pattern_text_finish = widgets.HTML(value="<b> Finishing: </b> Stuff and sew up. Weave in ends.")

widgets.VBox([widgets.HTML(value="Choose pattern parameters:"),
              widgets.GridBox([R,r,w,h],),
              widgets.HBox([pattern_text_upper]),
              out,
              pattern_text_lower])

:::