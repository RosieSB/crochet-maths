---
kernelspec:
  name: python3
  display_name: 'Python 3'
---

# Interactive torus pattern

:::{code-cell} Python
:label: parameters
:tags: [remove-input]
import ipywidgets as widgets
import numpy as np

# Major radius (R)
MajorRadiusSlider = widgets.FloatSlider(min=0, max=30, step=1, value=4, description="Major radius: ")
MajorRadiusText = widgets.BoundedFloatText(
    value=4,
    min=0,
    max=30,
    step=.01,
)
widgetLink = widgets.jslink((MajorRadiusSlider, 'value'), (MajorRadiusText, 'value'))

# Minor radius (r)
MinorRadiusSlider = widgets.FloatSlider(min=0, max=30, step=.01, value=1.78, description="Minor radius: ")
MinorRadiusText = widgets.BoundedFloatText(
    value=1.78,
    min=0,
    max=30,
    step=.01,
)
widgetLink = widgets.jslink((MinorRadiusSlider, 'value'), (MinorRadiusText, 'value'))

# Stitch width (w)
StitchWidthSlider = widgets.FloatSlider(min=0, max=5, step=.01, value=.4, description="Stitch width: ")
StitchWidthText = widgets.BoundedFloatText(
    value=.4,
    min=0,
    max=5,
    step=.01,
)
widgetLink = widgets.jslink((StitchWidthSlider, 'value'), (StitchWidthText, 'value'))


# Stitch height (h)
StitchHeightSlider = widgets.FloatSlider(min=0, max=5, step=.01, value=.4, description="Stitch height: ")
StitchHeightText = widgets.BoundedFloatText(
    value=.4,
    min=0,
    max=5,
    step=.01,
)
widgetLink = widgets.jslink((StitchHeightSlider, 'value'), (StitchHeightText, 'value'))


# Number of rows
N = widgets.Label(value=f'{round(MinorRadiusSlider.value * np.pi/StitchHeightSlider.value)}')

# Initial stitch count
init_st_cnt = widgets.Label(value=f'{round(2 * np.pi * (MajorRadiusSlider.value-MinorRadiusSlider.value)/StitchWidthSlider.value)}')

def update_N_1(n):
    N.value = f'{round(n["owner"].value * np.pi/StitchHeightSlider.value)}'
    init_st_cnt.value = f'{round(2*np.pi*(MajorRadiusSlider.value-MinorRadiusSlider.value)/StitchWidthSlider.value)}'
MinorRadiusSlider.observe(update_N_1)

def update_N_2(m):
    N.value = f'{round(MinorRadiusSlider.value * np.pi/m["owner"].value)}'
    init_st_cnt.value = f'{round(2*np.pi*(MajorRadiusSlider.value-MinorRadiusSlider.value)/StitchWidthSlider.value)}'
StitchHeightSlider.observe(update_N_2)
:::

Major radius: R = {eval}`MajorRadiusText` 

Minor radius: r = {eval}`MinorRadiusText` 

Stitch width: w = {eval}`StitchWidthText`

Stitch height: h = {eval}`StitchHeightText` 

Number of rows: N = {eval}`N`. 

**Row 0.** Chain {eval}`init_st_cnt` and join into round. 1 st into each chain sp around.

**Subsequent rows:** Work the following stitch counts in subsequent rows. Increases made using 2 st in st of previous row. Try to evenly distribute increases for a symmetrical look.

:::{code-block} Python
:tags: [remove-input]
out = widgets.Output()
with out:
    st_count = [0]*(int(N.value)+1)
    row = [0]*(int(N.value)+1)
    for n in range(int(N.value)+1):
        st_count[n] = round(2*np.pi*(MajorRadiusSlider.value-MinorRadiusSlider.value*np.cos(n*np.pi/int(N.value)))/StitchWidthSlider.value)
        row[n] = n
        if n<10:
            print('Row  ',n,': ',st_count[n],' st')
        else:
            print('Row ',n,': ',st_count[n],' st')
    
def on_value_change(k):
    out.clear_output()
    with out:
        st_count = [0]*(int(N.value)+1)
        row = [0]*(int(N.value)+1)
        for n in range(int(N.value)+1):
            st_count[n]=round(2*np.pi*(MajorRadiusSlider.value-MinorRadiusSlider.value*np.cos(n*np.pi/int(N.value)))/StitchWidthSlider.value)
            row[n] = n
            if n<10:
                print('Row  ',n,': ',st_count[n],' st')
            else:
                print('Row ',n,': ',st_count[n],' st')

N.observe(on_value_change, names='value')
MajorRadiusSlider.observe(on_value_change, names='value')
MinorRadiusSlider.observe(on_value_change, names='value')
StitchHeightSlider.observe(on_value_change, names='value')
StitchWidthSlider.observe(on_value_change, names='value')

out
:::

This completes the top half of the torus. The bottom half is almost identical --- just turn the work upside down and repeat, omitting the very last row (row {eval}`N`). 